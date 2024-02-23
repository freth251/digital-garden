#notes
- Design a schema for storing images and user interactions.
	- What do I want to store: 
		- I want to store images + their metadata
		- User Info + Interaction:
			- User Id + email + password
			- images they clicked on
			- images liked/disliked
			- images clicked
			- geography

This was the data I was looking for: 
**Implicit Feedback:**  
    - Clicks on images.(√)
    - Time spent viewing an image.(√)
    - Image views, scrolls, and zooms.
    - Mouse movements that may suggest interest. (X)
    - 
**Explicit Feedback:**
    - Ratings given to images.
    - Likes/dislikes. (√)
    - Comments or tags provided by users.
    - Saved or favorited images.

- Initialize a database (e.g., PostgreSQL, MongoDB).

```sql
CREATE TABLE Users (
    UserID INT PRIMARY KEY AUTO_INCREMENT,
    Email VARCHAR(255) UNIQUE NOT NULL,
    PasswordHash VARCHAR(255) NOT NULL,
    Geography VARCHAR(255)
);

```

```sql
CREATE TABLE Images (
    ImageID INT PRIMARY KEY AUTO_INCREMENT,
    FileName VARCHAR(255) NOT NULL,
    FilePath VARCHAR(255) NOT NULL,
    AltText VARCHAR(255),
    UploadDate DATETIME,
    -- Add additional metadata fields as necessary
);

```

```sql
CREATE TABLE UserInteractions (
    InteractionID INT PRIMARY KEY AUTO_INCREMENT,
    UserID INT,
    ImageID INT,
    InteractionType ENUM('click', 'like', 'dislike') NOT NULL,
    InteractionTime DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (UserID) REFERENCES Users(UserID),
    FOREIGN KEY (ImageID) REFERENCES Images(ImageID)
);

```

### Notes:

- **Password Storage**: Passwords should be securely hashed using a strong cryptographic hashing algorithm, like bcrypt, along with a salt to prevent rainbow table attacks.
- **Geography Data**: Depending on the granularity of geographic data, it could be broken down into separate fields like Country, State, City, etc.
- **Normalization**: The schema is designed to be normalized, which avoids redundancy and improves data integrity.
- **Indexing**: You should index columns that are frequently queried together, such as `UserID` and `ImageID` in the `UserInteractions` table, to speed up query performance.
- **Security**: Always consider security best practices, like using parameterized queries or prepared statements, to prevent SQL injection.
- **Scalability**: For scalability, particularly with the `UserInteractions` table which can grow quickly, consider partitioning strategies based on `InteractionTime` or `UserID`.

Depending on your application's specific needs or if you're using a NoSQL database like MongoDB, the schema could look quite different, possibly leveraging embedded documents for performance. Always design your schema based on the queries you expect to run and the database system you choose.