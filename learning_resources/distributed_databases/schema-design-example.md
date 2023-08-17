# ER Diagram for Social Media Website

## Entities and their attributes:

### User
- **UserID** (Primary Key)
- Username
- Email
- Password
- DateOfBirth
- ProfilePicture

### Post
- **PostID** (Primary Key)
- UserID (Foreign Key)
- Content
- PostDate

### Comment
- **CommentID** (Primary Key)
- PostID (Foreign Key)
- UserID (Foreign Key)
- CommentContent
- CommentDate

### Friendship
- **FriendshipID** (Primary Key)
- User1ID (Foreign Key)
- User2ID (Foreign Key)
- FriendshipDate

## Relationships:

- **User** can create many **Posts**: One-to-Many between User and Post.
- **User** can write many **Comments**: One-to-Many between User and Comment.
- **Post** can have many **Comments**: One-to-Many between Post and Comment.
- Two **Users** can have a **Friendship**: Many-to-Many between User and itself, represented via the Friendship entity.

## Normalization:

Normalization is the process of organizing data to reduce redundancy and ensure data integrity. The primary aim is to divide large tables into smaller ones and link them using relationships.

### 1NF (First Normal Form):

- Each table has a primary key: unique data that identifies every record.
- Each column contains atomic (indivisible) values.
- Each column contains values of the same kind.
- Each column has a unique name.
- The order in which data is stored doesn't matter.

**Our social media ER is in 1NF because**:
- Each entity has a primary key (UserID, PostID, CommentID, FriendshipID).
- All attributes have atomic values.

### 2NF (Second Normal Form):

- It's in 1NF.
- All non-key attributes are fully functional dependent on the primary key.

**Assuming that Username and Email can be unique for each user, our User table is in 2NF**. The other tables are also in 2NF because non-key attributes are dependent on their primary keys.

### 3NF (Third Normal Form):

- It's in 2NF.
- All attributes are functionally dependent only on the primary key.

**Our design is in 3NF**. For example, in the Post entity, Content and PostDate are dependent on PostID but not on the UserID.

### BCNF (Boyce-Codd Normal Form):

- It's in 3NF.
- For any dependency A -> B, A should be a super key.

**Our design meets this because**, for example, in the Friendship entity, the combination of User1ID and User2ID can be seen as a composite super key for FriendshipDate.

### 4NF (Fourth Normal Form):

- It's in BCNF.
- There are no multi-valued dependencies.

**Our design adheres to 4NF**. We don't have attributes in any table that would create multi-valued dependencies.

### 5NF (Fifth Normal Form):

- Concerned with cases where we might be storing redundant info in a table due to combinations of multiple types of information.
