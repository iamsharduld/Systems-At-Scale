# Online Bookstore Database Schema Design

Database schema refers to the structure or blueprint of how data is organized and how relationships between data are handled in the database. Proper schema design ensures the efficiency, flexibility, and scalability of the database.

## Entities & Attributes

### 1. Books

- **BookID** (Primary Key)
- **Title**
- **AuthorID** (Foreign Key)
- **Price**
- **Genre**
- **ISBN**
- **Publication Date**

### 2. Authors

- **AuthorID** (Primary Key)
- **Name**
- **Birthdate**
- **Nationality**

### 3. Customers

- **CustomerID** (Primary Key)
- **Name**
- **Email**
- **Address**

### 4. Orders

- **OrderID** (Primary Key)
- **CustomerID** (Foreign Key)
- **Date**
- **Total Amount**

## Relationships

- **Books to Authors**: Many books can have one author. This is a *Many-to-One* relationship.
- **Customers to Orders**: One customer can have many orders. This is a *One-to-Many* relationship.

## Design Considerations

1. **Normalization**: Reduces redundancy and ensures data integrity. The separation of Books and Authors using `AuthorID` is an example.

2. **Scalability**: Design should accommodate future requirements, such as books with multiple authors.

3. **Flexibility**: Avoid overly specific attributes. For instance, using a generic "Address" field instead of multiple specific fields.

4. **Indexes**: Useful for fields that are frequently queried to speed up search operations.

5. **Data Types**: Ensure appropriate types are chosen. E.g., ISBN might be a `VARCHAR` due to hyphens.

6. **Constraints**: Rules that help maintain data integrity, like ensuring unique email addresses for customers.
