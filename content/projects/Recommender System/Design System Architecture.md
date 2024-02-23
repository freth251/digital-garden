#notes
- Sketch out the system architecture.
- Decide on the tech stack (programming language, frameworks, database).

## 1. Requirements:

Functional Requirements: What the systems is supposed to do 
- User Profile: 
	- Login and Authentication
- Display a tile of pictures based on the recsys candidates
- Like/Dislike Images
- Load a new set of images when user scrolls down
- Generate new images based on user preferences

Non-functional Requirements: 
- Latency has to be less than 200ms (excluding network latency from users to our servers)
- Throughput: 1000 queries per second 

Constraints: 
- Cost: 10% of expected revenue (min $100)

## 2. Key Components

- Data stores: 
	- Database to store user information
	- Feature store
- Clients:
	- Web apps

