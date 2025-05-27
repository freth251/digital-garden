2025-05-26 14:45

[Code](https://github.com/freth251/tensor-opt-kernels)
*under construction...under construction...under construction...under construction...under construction...under construction...under construction...under construction...under construction...under construction...under construction...under construction...under construction...*

# Building a High-Performance GEMM Kernel

Matrix multiplications are central to scientific computing, machine learning, graphics rendering and so much more. At the same time they are very costly operations (O(n^3) without any algorithmic improvements). This makes them a good candidate for optimization efforts. 

In this blog, we will start with a naive implementation and work our way up to highly optimized implementation. 

## Naive Implementations

```cpp
void gemm_naive(const float* A, const float* B, float* C, int M, int N, int K) {
    // C[M][N] = A[M][K] * B[K][N]
    for (int i = 0; i < M; ++i)
        for (int j = 0; j < N; ++j) {
            for (int k = 0; k < K; ++k)
                C[i * N + j] += A[i * K + k] * B[k * N + j];
        }
}
```

![[gflops_naive.png]]

![[timesec_naive.png]]
## Basic Optimization

### Optimizing Compilers 

Modern compilers employ optimization techniques such as code selection and ordering, dead code elimination, register allocation and eliminating minor inefficiencies. Although these are helpful, there are limitations to how aggressively it can optimize our code because it operates under fundamental constraints ( no change in program behavior) and limited context (analysis performed only within procedures and based only on static information). *When in doubt the compiler must be conservative.*

One such optimization blockers is the potential for memory aliasing. 

### Memory Aliasing

If we take a closer look at the assembly code that corresponds to `C[i * N + j] += A[i * K + k] * B[k * N + j]` we can see the following in the third inner loop: 

```asm
        addss   %xmm0, %xmm1
        movss   %xmm1, 0(%r13)
```

The concerning line is the second one. It Means that we update `C[i * N + j]` on every iteration. 

This might seem like an obvious job for the compiler to optimize because we do not need to update the memory on every iteration only once. But the compiler must consider the possibility that `A` and/or `B` point to the same memory as `C`. If that were the case we are changing the value of the elements on each iteration thus need to update the memory. 

The compiler cannot perfectly detect if two pointers point to the same thing because the value of the pointers can be dependent on runtime behavior, and in the general case determining wether aliasing occurred is undecidable because of the halting problem. 

Compilers assume aliasing is possible unless they can prove it's not, which is possible in simple cases. 

The way we can resolve this is by accumulating the sum within the loop. 

```cpp
void gemm_mem_aliasing(const float* A, const float* B, float* C, int M, int N, int K) {
    for (int i = 0; i < M; ++i)
        for (int j = 0; j < N; ++j) {
            float sum = 0.0f;
            for (int k = 0; k < K; ++k)
                sum += A[i * K + k] * B[k * N + j];
            C[i * N + j] = sum;
        }
}
```

![[gflops_mem_aliasing_naive.png]]

(Why does the mem aliasing sart out slower)

![[timesec_mem_aliasing_naive.png]]

## Loop Unrolling

Modern processors don't execute instructions one at a time as the machine level code suggests. Instead they perform instruction level parallelism where complex mechanisms are employed to execute multiple instructions at the same time while presenting a view of a simple sequential instruction execution. 

In this model, computation is divided into stages and while one goes through the different stages another can start if they have no dependency. 

(For Haswell CPU)

| Instruction                 | Latency  | Cycles/Issue |
| --------------------------- | -------- | ------------ |
| Load / Store                | 4        | 1            |
| Integer Multiply            | 3        | 1            |
| **Integer/Long Divide**     | **3–30** | **3–30**     |
| Single/Double FP Multiply   | 5        | 1            |
| Single/Double FP Add        | 3        | 1            |
| **Single/Double FP Divide** | **3–15** | **3–15**     |
our inner loop 
```asm
.L4:
        movss   (%rax), %xmm0
        mulss   (%rdx), %xmm0
        addq    $4, %rax
        addq    %rsi, %rdx
        addss   %xmm0, %xmm1
        cmpq    %rax, %rdi
        jne     .L4
```


optimized version

```cpp
void gemm_loop_unrolling_x1(const float* A, const float* B, float* C, int M, int N, int K) {
    for (int i = 0; i < M; ++i)
        for (int j = 0; j < N; ++j) {
            float sum = 0.0f;
            for (int k = 0; k < K; k+=2){
                sum += A[i * K + k] * B[k * N + j];
                sum += A[i * K + k+1] * B[(k+1) * N + j];
            }
                
            C[i * N + j] = sum;
        }
}
```

```cpp
void gemm_loop_unrolling_x3(const float* A, const float* B, float* C, int M, int N, int K) {
    for (int i = 0; i < M; ++i)
        for (int j = 0; j < N; ++j) {
            float sum = 0.0f;
            for (int k = 0; k < K; k+=4){
                sum += A[i * K + k] * B[k * N + j];
                sum += A[i * K + k+1] * B[(k+1) * N + j];
                sum += A[i * K + k+2] * B[(k+2) * N + j];
                sum += A[i * K + k+3] * B[(k+3) * N + j];
            }
                
            C[i * N + j] = sum;
        }
}
```

results: 

![[gflops_loop_unrolling_x1_loop_unrolling_x3_mem_aliasing_naive.png]]

![[timesec_loop_unrolling_x1_loop_unrolling_x3_mem_aliasing_naive.png]]

## Cache Blocking

Cache Blocking is a technique that minimizes cache misses by working with a subset of the data (blocks) at a time so that it fits in the cache and minimizes expensive reads from memory.

```cpp
constexpr int BLOCK_M = 64;
constexpr int BLOCK_N = 64;
constexpr int BLOCK_K = 64;

void gemm_cache_blocking(const float* A, const float* B, float* C, int M, int N, int K) {
    for (int i0 = 0; i0 < M; i0 += BLOCK_M) {
        for (int j0 = 0; j0 < N; j0 += BLOCK_N) {
            for (int k0 = 0; k0 < K; k0 += BLOCK_K) {

                int i_max = std::min(i0 + BLOCK_M, M);
                int j_max = std::min(j0 + BLOCK_N, N);
                int k_max = std::min(k0 + BLOCK_K, K);

                for (int i = i0; i < i_max; ++i) {
                    for (int j = j0; j < j_max; ++j) {
                        float sum = C[i * N + j];
                        for (int k = k0; k < k_max; k+=4) {
                            sum += A[i * K + k] * B[k * N + j];
                            sum += A[i * K + k+1] * B[(k+1) * N + j];
                            sum += A[i * K + k+2] * B[(k+2) * N + j];
                            sum += A[i * K + k+3] * B[(k+3) * N + j];
                        }
                        C[i * N + j] = sum;
                    }
                }
            }
        }
    }
}

```

![[gflops_cache_blocking_loop_unrolling_x1_loop_unrolling_x3_mem_aliasing_naive.png]]

![[timesec_cache_blocking_loop_unrolling_x1_loop_unrolling_x3_mem_aliasing_naive.png]]

## AVX2/Neon/RVV


## References