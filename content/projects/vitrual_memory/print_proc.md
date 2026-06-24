


> **Disclaimer.** AI was used in preparing this document. All analysis, experimentation, and conclusions are the author's own and were verified against the primary documentation cited throughout. AI assistance was limited to organization and review. 

---

## Part 1 — Reading `/proc/self/maps`

`/proc/self/maps` prints a process's virtual memory layout.

```c
void print_proc()
{
    FILE *fp = fopen("/proc/self/maps", "r");
    int c;
    while ((c = getc(fp)) != EOF)
        putc(c, stdout);
    fclose(fp);
}
```

_How and where is this information stored?_

The kernel keeps a `task_struct` per process, and inside it an `mm_struct` describing the address space as a list of memory regions (VMAs). `/proc` is a window into these structures, generated when read. Reading `/proc/self/maps` makes the kernel walk the VMA list and format it as text.
![[how_linux_organizes_virtual_memory.png]]
<center><i>Fig 1. How linux organizes memory.</i></center>

> Ref: [proc(5)](https://man7.org/linux/man-pages/man5/proc.5.html), [proc_pid_maps(5)](https://man7.org/linux/man-pages/man5/proc_pid_maps.5.html) [The /proc file system](https://docs.kernel.org/filesystems/proc.html) 

From our code snippet, the following is printed: 

![[proc_self_maps_out.png]]

<center><i>Fig 2. /proc/self/maps output.</i></center>


_What do the columns mean?_

- (1) **address** — start–end of the region in virtual memory. Translation to physical memory is done by the hardware and kernel.
- (2) **perms** — `r` read, `w` write, `x` execute. The fourth character is `p` (private, copy-on-write) or `s` (shared).
- (3) **offset** — offset into the mapped file where the region starts.
- (4) **dev** — device (major:minor) the file lives on.
- (5) **inode** — inode of the backing file. `0` means anonymous (no file).
- (6) **pathname** — the file, or a tag such as `[heap]`, `[stack]`, `[vdso]`.


Overlaying this with memory-layout diagram from from _Computer Systems: A Programmer's Perspective_ (Bryant & O'Hallaron), we can obtain the following mapping: 
![[mem_layout_overlap.png]]
<center><i>Fig 3. Memory layout overlap.</i></center>

We continue our investigation by verifying this mapping. 

> The change to `aaaa…`/`ffff…` addresses come from running under `setarch -R`, which disables ASLR (Address Space Layout Randomization) so the layout repeats every run, I am too lazy to redo Fig2 with these addresses. Ref: [setarch(8)](https://man7.org/linux/man-pages/man8/setarch.8.html), [readelf(1)](https://man7.org/linux/man-pages/man1/readelf.1.html)

> We flipped the order from fig 2 to fig 3 to make the mapping cleaner. 


---

## Part 2 — Where do my variables live?

```c
int main()
{
    int i;
    int m = 9;
    printf("uninitialized: %p\n", &i);   // 0xffffffffed90
    printf("initialized:   %p\n", &m);   // 0xffffffffed94
}
```

_I expected uninitialized data in `.bss` and initialized in `.data`. Why are these on the stack?_

`.bss` and `.data` hold globals and statics. `i` and `m` are locals, so they sit on the stack. The `0xffff...` addresses confirm the stack region. Promoting them to globals moves them:

```c
int i;       // global, uninitialized -> .bss
int m = 9;   // global, initialized   -> .data
int main()
{

    printf("uninitialized: %p\n", &i); 
    printf("initialized:   %p\n", &m);
}
```

These land near `aaaaaaac0000`, inside the binary's writable region.

_The maps shows only three `a.out` regions. Where are `.data` and `.bss` separately?_

The maps groups memory by permission rather than by ELF section. `readelf` shows the real sections:

```
$ readelf -S a.out | grep -E "\.data|\.bss"
 [22] .data  PROGBITS  0000000000020000  00010000
 [23] .bss   NOBITS    0000000000020014  00010014
```

`.data` and `.bss` are both `rw-`, so the kernel reports them as a single `rw-p` mapping. The arithmetic confirms it: base `aaaaaaaa0000` + `.data` offset `0x20000` = `aaaaaaac0000`, which is the `rw-p` region. The three `a.out` lines are permission buckets: code (`r-x`), read-only data (`r--`), writable data (`rw-`).


---

## Part 3 — The heap and `brk`

The heap starts at 1.2 MB before any allocation:

```
1306624  1.2M  aaaaaaac1000-aaaaaac00000 rw-p ... [heap]
```

We can print `brk` using the following snippet: 
```c
void print_brk()
{
    void *current = sbrk(0);
    printf("brk = %p\n", current);
}
```

_`brk` prints as `0xaaaaaaac1000`. Shouldn't it be at the top of the heap, `0xaaaaaac00000`?_

`brk` is the program break, the current top of the heap. The printed value is the initial break (heap start), captured before any allocation.

> **hypo:** the first `printf` allocates memory, so the break moves.

```c
print_brk();                 // brk = 0xaaaaaaac1000
printf("Hello, World.\n");
print_brk();                 // brk = 0xaaaaaac00000
```

Confirmed. The first `printf` makes glibc allocate a stdio buffer; that allocation grows the heap. The `brk` variable was stale, read once at startup.

_The heap grew ~1.25 MB just for "Hello World". Does it depend on the string size?_

It does not. A much larger string produces the same growth. 

```c
int main()
{
    char arr[2306625];

    for (int i = 0; i <2306624; i++)
        arr[i]='i';
    arr[2306624] = "\n";

    print_brk();
    getchar();
    printf(arr);

    print_brk();
}
```

The stdio buffer itself is a few KB. `printf` streams output through that fixed buffer, so string length never becomes heap size. Separately, `malloc` requests a large slab (~1.25 MB here) from the kernel in one call and hands out small pieces from it, which amortizes future allocations.

```c
inc_brk(10);  // brk = ...0a  (+10)
dec_brk(1);   // brk = ...09  (-1)
```

The break can be moved directly with `sbrk`.

> Ref: [malloc(3)](https://man7.org/linux/man-pages/man3/malloc.3.html), [sbrk(2)](https://man7.org/linux/man-pages/man2/sbrk.2.html)

---

## Part 4 — Allocating to the heap

![[heap_before_alloc.png]]
<center><i>Fig 4. Memory layout before heap allocation.</i></center>


`malloc(10)` followed by `free` changes nothing in the maps; it is carved from the existing 1.2 MB slab. A larger allocation,  `malloc(1306624)` makes a new region appear above the heap:

![[heap_after_alloc_1306624_bytes.png]]
<center><i>Fig 5. Memory layout after 1306624 bytes allocated.</i></center>

We can see that a new area has appeared above the `[heap]`. 

_Is that new area also the heap?_

_What happens if we allocate wayyy more bytes?_

```c
void *p = malloc(50UL * 1000 * 1306620);   // ~50 GB
```

![[heap_after_alloc_50GB.png]]

<center><i>Fig 6. Memory layout after 50GB allocated.</i></center>


This allocation fails. 

After some trial and error, I found out that I can allocate only around 4GB at a time. 

![[heap_after_alloc_4GB.png]]
<center><i>Fig 7. Memory layout after 4GB allocated.</i></center>

I am limited to 4GB at a time, but it turns out that I can repeatedly make this allocation a large amount of times. 

```c
int *a[1000];
for (int i = 0; i < 1000; i++)
    a[i] = malloc(3UL * 1000 * 1306620);   // ~3.65 GB each
```

I can somehow allocate 4TB of memory in a machine that has 4GB of storage. 

![[heap_after_alloc_4TB.png]]

<center><i>Fig 8. Memory layout after 4TB allocated.</i></center>

> SO CLEARLY MALLOC DOES NOT REALLY ALLOCATE MEMORY? _(accidental caps lock — I'm not mad, I'm chill.)_


I am left with the following questions: 
- _Is the new area above `[heap]` a part of `[heap]`?_
- _Why can we only allocate ~4GB at a time? _
- _How can we allocate more data than we have space?_ 


The new area is a separate anonymous `mmap`, distinct from the brk arena. glibc's `malloc` has two backends: requests below `MMAP_THRESHOLD` (128 KB default) grow the brk heap and carry the `[heap]` tag; requests at or above it get a private anonymous `mmap` with blank pathname and inode `0`. The `mmap` region is placed near the shared libraries, which explains the address gap. When many such mappings are contiguous with identical permissions, the kernel merges them into one VMA, shown as a single large line.

The ~4 GB ceiling is integer overflow in the size argument rather than a limit in `malloc`. `4 GiB = 2³²` is where 32-bit arithmetic wraps. A size written as plain `int`, such as `4 * 1024 * 1024 * 1024`, wraps to `0`; `malloc` then returns a small chunk or `NULL`, and the maps are unchanged. Forcing 64-bit arithmetic removes the ceiling, which is why the loop above reserved terabytes — its `3UL` made each computation 64-bit. The fix is `(size_t)4 * 1024 * 1024 * 1024` or `4UL << 30`. `calloc(n, size)` checks the `n * size` multiplication for overflow and returns `NULL` instead of wrapping.

The reservation exceeds physical space because `malloc` reserves virtual address space, and physical RAM is committed only when a page is touched. This is demand paging. `mmap` records a VMA in the `task_struct`. Page-table entries are not pre-filled; for a 3.6 TB range that would itself cost gigabytes of page tables. The pages do not exist yet. When the CPU touches a page, a page fault occurs; the kernel checks the address against the VMA list, finds it legal (so no segfault), and resolves it. A read of an untouched anonymous page maps a shared zero page, read-only, which every untouched page across all processes can share, so no new RAM is consumed. A write makes the kernel take a free physical frame from RAM, zero it, and map it writable, which is where real memory is spent. A page faults once; after the PTE is installed, later accesses are ordinary memory accesses. The loop above stored pointers without writing into the memory, so almost no physical pages were used, and Linux overcommit permitted the reservation.


> Refs: [malloc(3)](https://man7.org/linux/man-pages/man3/malloc.3.html), [mallopt(3)](https://man7.org/linux/man-pages/man3/mallopt.3.html), [GNU libc: Malloc Tunable Parameters](https://www.gnu.org/software/libc/manual/html_node/Malloc-Tunable-Parameters.html), [proc_pid_maps(5)](https://man7.org/linux/man-pages/man5/proc_pid_maps.5.html), [mmap(2)](https://man7.org/linux/man-pages/man2/mmap.2.html), [overcommit accounting](https://docs.kernel.org/mm/overcommit-accounting.html), `overcommit_memory` in [proc(5)](https://man7.org/linux/man-pages/man5/proc.5.html)
---

## Part 5 — Linking

```c
// mainHello.c
#include "printHello.c"
```


![[include_printHello.png]]
<center><i>Fig 9. Memory layout after including "printHello.c".</i></center>


I expected `printHello` to appear as its own region like `libc.so`. It did not.

_Why not?_

`#include` is preprocessor copy-paste of source text, performed before compilation. `printHello`'s source was pasted into `mainHello.c`, compiled as one translation unit, and built into `a.out`. There is no separate object to map at runtime. `printf` differs: `#include <stdio.h>` pastes only its declaration. Its code comes from `libc.so.6`, resolved by the linker and loaded at runtime.

> Ref: [GCC CPP — Header Files](https://gcc.gnu.org/onlinedocs/cpp/Header-Files.html)

_Static vs dynamic linking?_

Static linking copies the library's compiled code into the executable. The file is larger and self-contained, with no `.so` mapping at runtime. Dynamic linking records a dependency ("needs `libc.so.6`"); the dynamic linker (`ld-linux`) loads and maps the library at runtime. `libc.so.6` appears as its own file-backed mapping because it is loaded at runtime. `gcc -static` removes the libc mapping and enlarges the binary.

_How do I dynamically link my own code?_

Build a shared library instead of including the source:

```bash
gcc -shared -fPIC printHello.c -o libprintHello.so
gcc mainHello.c -L. -lprintHello -Wl,-rpath,. -o a.out
```

![[dyn_link_printHello.png]]
<center><i>Fig 10. Memory layout after dynamic linking of "printHello.c".</i></center>

(Hello, World from 0xfffff7fb0020)

`printHello` now gets its own file-backed mapping beside `libc.so.6`.

_Why does `printf` need none of those flags?_

It needs the same things, it's just supplied automatically.

|What's needed|my library|libc / printf|
|---|---|---|
|`-fPIC -shared`|I build it|already built by the distro|
|declaration|I write a header|`<stdio.h>` provides it|
|link flag|`-lprintHello -L.`|`-lc` added automatically by gcc|
|runtime findability|`-rpath,.` (cwd not searched)|on the default path and in `ld.so.cache`|

> Ref: [ld.so(8)](https://man7.org/linux/man-pages/man8/ld.so.8.html), [GCC Link Options](https://gcc.gnu.org/onlinedocs/gcc/Link-Options.html)

---

## Part 6  Reading the physical address

To compare what two processes see, I needed the physical address behind a pointer. The kernel exposes the virtual-to-physical mapping through `/proc/pid/pagemap`, which holds one 64-bit value for every virtual page.

Reading it with `cat /proc/self/pagemap` hangs. The file is indexed over the whole virtual address space, one 8-byte entry per page. On aarch64 (48-bit addresses, 4 KB pages) that is `2³⁶` entries ≈ 512 GB, mostly zero. `cat` starts at address 0 (unmapped, all zeros) and tries to stream the entire range in order, so it never finishes. The documentation says as much: to use pagemap efficiently, consult `/proc/pid/maps` for the regions that are actually mapped and seek past the unmapped gaps. The file is read by seeking directly to the one entry for the address in question.

Addressing works in pages rather than single bytes. A page is the unit of transfer between disk and memory (4 KB here), so memory is tracked one page at a time. (I am not certain why it is built this way — locality, and fewer entries to track, seem like the reasons.)

A virtual address splits into a page number (high bits) and an offset within the page (low bits). The split is fixed by the page size and the address width, which I read from the kernel config:

```
$ grep -E 'CONFIG_ARM64_(VA_BITS|4K_PAGES)=' /boot/config-$(uname -r)
CONFIG_ARM64_4K_PAGES=y
CONFIG_ARM64_VA_BITS=48
```

The virtual address is 48 bits and the page offset is `log2(4096) = 12` bits, leaving a 36-bit virtual page number (VPN).

c

```c
page_number = v / page_size   ==  v >> 12     // VPN, index into pagemap
offset      = v % page_size   ==  v & 0xFFF   // VPO, unchanged in the physical address
```

Printing a pointer in binary shows the breakdown:

```
0000000000000000 101010101010101010101010101011000001 000000010000
                 └──────────── VPN (36) ─────────────┘ └─ VPO (12) ─┘
```

An early miscalculation: I first assumed a 20-bit page number, which gives only `2²⁰` pages × 4 KB ≈ 4 GB of addressable space, and I wondered whether it tied to the ~4 GB malloc ceiling. The kernel config corrected this. VA is 48 bits, so the page number is 36 bits, and the two numbers are unrelated.

The page number is the index into pagemap. Each entry is 8 bytes, so multiplying the page number by 8 gives the byte offset to seek to. `pread` reads exactly that 8-byte entry without touching the rest of the file.

Each entry is a 64-bit value. Bits 0–54 hold the physical frame number (PFN) when the page is present, bit 63 marks the page present, bit 62 marks it swapped. The physical address is the frame's base plus the offset carried over from the virtual address: `PFN * page_size + offset`. Only the page number is translated; the offset is identical on both sides.


```c
uint64_t virt_to_phys(void *vaddr) {
    long ps = sysconf(_SC_PAGESIZE);
    uint64_t v = (uint64_t)(uintptr_t)vaddr;

    int fd = open("/proc/self/pagemap", O_RDONLY);
    if (fd < 0) { perror("open"); return 0; }

    uint64_t entry;
    if (pread(fd, &entry, 8, (v / ps) * 8) != 8) { perror("pread"); close(fd); return 0; }
    close(fd);

    if (!(entry & (1ULL << 63))) { fprintf(stderr, "page not present\n"); return 0; }
    uint64_t pfn = entry & ((1ULL << 55) - 1);     // keep bits 0-54, drop the flags
    if (!pfn) { fprintf(stderr, "PFN=0 — run with sudo\n"); return 0; }

    return pfn * ps + (v % ps);
}
```

Two constraints I hit. The PFN is hidden from unprivileged users, so the program must run under `sudo` or the PFN reads back as `0`. The page must be present, meaning it has been touched and given a physical frame; an untouched page has no PFN and bit 63 is clear. One detail in the masking: `(1ULL << 55) - 1` builds a 55-bit mask to keep bits 0–54, and the `ULL` matters, since shifting a plain `int` by 55 is undefined.

> Ref: [kernel pagemap docs](https://docs.kernel.org/admin-guide/mm/pagemap.html)
---

## Part 7 — Is a shared library's global the same physical memory across processes?

I put a global in the shared library and printed its address:

c

```c
int someV = 10;
void printHello(void) { printf("Hello, World from %p\n", &someV); }
```

📷 _Image: `someV` in the library's r/w region_ — `Pasted_image_20260619174850.png`

The address lands in the private read/write region of `libprintHello.so`. With ASLR enabled, I ran the program in two processes, expecting the same physical address with different virtual addresses, since both map the same library file.

c

```c
void *getSomeV(void) { return &someV; }
```

![[1proc_someV.png]]
<center><i>Fig 11. someV writable global — VA and PA, one process.  </i></center>


![[2proc_someV.png]]
<center><i>Fig 12. someV writable global — two processes, PA differs.  </i></center>


The physical addresses differed. `someV` is a writable global in `.data`, which is mapped private copy-on-write, so each process gets its own physical copy of writable data. The pages shared physically across processes are the read-only ones:

```
.text / .rodata             → read-only, file-backed → shared physical page  (same PA)
.data / .bss / heap / stack → writable, private (CoW) → own copy             (different PA)
```

Marking `someV` as `const` moves it to `.rodata`, which gave the same PA in both processes.

![[2proc_samePA.png]]
<center><i>Fig 13. someV as const (.rodata) — two processes, PA matches.  </i></center>


A `const` cannot be written, so it could not show writable sharing. I then tried a `static` variable:

c

```c
void *getSomeV(void)
{
    static bool created = false;
    if (!created) { created = true; printf("I have been created\n"); }
    return &created;
}
```

![[static_diff_pa_h.png]]
<center><i>Fig 14. static variable — two processes, PA differs.</i></center>


The physical addresses still differed. `static` changes lifetime and visibility within one process and has no effect on physical sharing across processes; the variable lives in `.data`, one copy per process. Both processes printed "I have been created", confirming separate copies. Sharing writable memory needs something built for it.

---

## Part 10 — A shared slab between two processes

_First idea: process A writes its pointer to a file; process B reads it and uses it._

A pointer is a number valid only inside the address space that produced it. B's `0x7f…abcd` indexes B's page tables, so it points elsewhere or faults. A raw pointer cannot cross processes.

_Second idea: save the physical address; B converts physical to virtual._

Userspace has no physical-to-virtual reverse mapping; that operation is privileged because it would break isolation. The physical address is also unstable, since the kernel can swap or migrate the page.

_What crosses the boundary?_

A name. Both processes agree on a name, both `mmap` it `MAP_SHARED`, and the kernel maps the same physical pages into each, returning a valid pointer per process. This matches how `.text` sharing already works: both processes map `a.out`, so they share physical pages without exchanging an address.

```c
void *getSomeV(void)
{
    int created = 0;
    int fd = open("/tmp/slab", O_CREAT | O_EXCL | O_RDWR, 0600);  // create only if absent
    if (fd >= 0) {
        created = 1;                       // creator
    } else if (errno == EEXIST) {
        fd = open("/tmp/slab", O_RDWR);    // already exists -> attach
    } else {
        perror("open"); exit(1);
    }

    if (created) ftruncate(fd, 4096);      // only the creator sizes it (new file is 0 bytes)

    void *p = mmap(NULL, 4096, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
    return p;
}
```

Notes from wiring this up:

- `open` returns a small positive fd, or `-1` with `errno` set. It never returns `0` for a missing file.
- `O_CREAT | O_EXCL` is an atomic "create only if absent". The first process wins (creator); the second gets `EEXIST` (attacher). Identical code selects its role with no race.
- A fresh file is 0 bytes. Mapping zero bytes faults with `SIGBUS`, so the creator runs `ftruncate` first. The attacher skips `ftruncate` and initialization to avoid clobbering data.
- `mmap` makes a file's bytes addressable as memory, loaded lazily by page fault. With `MAP_SHARED`, both processes are backed by the same page-cache pages. `/tmp/slab` is file-backed, so the data survives a process exit.

> Ref: [open(2)](https://man7.org/linux/man-pages/man2/open.2.html), [ftruncate(2)](https://man7.org/linux/man-pages/man2/ftruncate.2.html), [mmap(2)](https://man7.org/linux/man-pages/man2/mmap.2.html). For RAM-only backing with no disk path, [shm_open(3)](https://man7.org/linux/man-pages/man3/shm_open.3.html) does the same under `/dev/shm`.

The demo:

```c
int main(void)
{
    char *p = (char *)getSomeV();
    volatile char t = p[0]; (void)t;       // read-touch -> page present, value preserved

    printf("\n[BEFORE] slab[0] = '%c'\n", p[0]);
    printf("VA:\t"); printAddressBinary(&p[0]);
    printf("PA:\t"); printAddressBinary((void *)virt_to_phys(&p[0]));

    printf("enter a character (or just Enter to only read): ");
    fflush(stdout);
    int c = getchar();
    if (c != '\n' && c != EOF)
        p[0] = (char)c;                    // write into the shared slab

    printf("\nslab[0] = '%c'\n", p[0]);
    printf("VA:\t"); printAddressBinary(&p[0]);
    printf("PA:\t"); printAddressBinary((void *)virt_to_phys(&p[0]));

    printf("running — press Ctrl-C to exit\n");
    pause();                               // stay alive so the other process can share
    return 0;
}
```


![[mmap_shared.png]]
<center><i>Fig 15. MAP_SHARED, two terminals.</i></center>


Result: both processes show different VA and the same PA (`0x4f5e6000`). Typing a character in one process changes what the other reads. The matching physical address confirms the sharing.

---

## Part 11 — Copy-on-write

Switch one flag: `MAP_SHARED` to `MAP_PRIVATE`.

```c
void *p = mmap(NULL, 4096, PROT_READ | PROT_WRITE, MAP_PRIVATE, fd, 0);
```

_What changes?_

Under `MAP_PRIVATE`, a read keeps the process on the shared page. A write triggers the copy-on-write split. In `[BEFORE]`, with only a read-touch, both processes share the same physical page and report the same PA. After one process writes, that process moves to a private frame, so its PA changes while the other's stays.

![[mmap_private.png]]
<center><i>Fig 16. MAP_PRIVATE, two terminals.</i></center>

On write (not read), the writing process's PA changes from `0x4f5e6000` to `0x80b3a000`. The other process's PA is unchanged, and it never sees the new value.

```
MAP_SHARED  : same page before and after a write   (writes are shared)
MAP_PRIVATE : same page before, private copy after  (writes diverge — copy-on-write)
```


---

## Summary

- `/proc` is a generated view of kernel structures.
- The maps groups memory by permission, not ELF section.
- `malloc` reserves virtual space; physical RAM is committed lazily on first write, one page at a time. A 3.7 TB reservation can succeed for this reason.
- A virtual address is a page number plus an offset. The page number is the index into `/proc/pid/pagemap`.
- Pointers and physical addresses are per-process and unstable. Sharing memory uses a name plus `MAP_SHARED`, with the kernel handling the physical mapping.
- Read-only pages are shared system-wide. Writable sharing is opt-in, and copy-on-write is the mechanism underneath.

---

### References

- Linux man pages: [proc(5)](https://man7.org/linux/man-pages/man5/proc.5.html) · [proc_pid_maps(5)](https://man7.org/linux/man-pages/man5/proc_pid_maps.5.html) · [malloc(3)](https://man7.org/linux/man-pages/man3/malloc.3.html) · [mallopt(3)](https://man7.org/linux/man-pages/man3/mallopt.3.html) · [mmap(2)](https://man7.org/linux/man-pages/man2/mmap.2.html) · [open(2)](https://man7.org/linux/man-pages/man2/open.2.html) · [ftruncate(2)](https://man7.org/linux/man-pages/man2/ftruncate.2.html) · [shm_open(3)](https://man7.org/linux/man-pages/man3/shm_open.3.html) · [ld.so(8)](https://man7.org/linux/man-pages/man8/ld.so.8.html) · [setarch(8)](https://man7.org/linux/man-pages/man8/setarch.8.html)
- Kernel docs: [pagemap](https://docs.kernel.org/admin-guide/mm/pagemap.html) · [overcommit accounting](https://docs.kernel.org/mm/overcommit-accounting.html) · [memory concepts](https://docs.kernel.org/admin-guide/mm/concepts.html)
- GCC: [CPP Header Files](https://gcc.gnu.org/onlinedocs/cpp/Header-Files.html) · [Link Options](https://gcc.gnu.org/onlinedocs/gcc/Link-Options.html)
- GNU libc: [Malloc Tunable Parameters](https://www.gnu.org/software/libc/manual/html_node/Malloc-Tunable-Parameters.html)
- Bryant & O'Hallaron, _Computer Systems: A Programmer's Perspective_ (memory-layout figure).