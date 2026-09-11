```markdown
# Library Management System API Design

## 1. Overview
The Library Management System API is a RESTful API designed to manage library members, books, loans, and categories.
The API allows users to perform CRUD (Create, Read, Update, Delete) operations on library resources. It also supports relationships between members, books, loans, and categories.

## 2. Base URL
Production: https://api.library.com/v1
Development: http://localhost:3000/api

## 3. Resources
The Library Management System contains the following resources:

Members
Books
Loans
Categories

Resource:  members
Properties:
- id (integer, auto-generated)
- first_name (string, required)
- last_name (string, required)
- email (string, unique, required)
- phone (string, optional)
- created_at (timestamp)
- updated_at (timestamp)

Resource: loans
Properties:
- id (integer, auto-generated) 
- member_id (integer, foreign key) 
- book_id (integer, foreign key) 
- borrow_date (timestamp) 
- due_date (timestamp, required) 
- return_date (timestamp, nullable) 
- status (string, active/returned/overdue) 
- created_at (timestamp) 
- updated_at (timestamp)
Resource: categories 
Properties: 
- id (integer, auto-generated) 
- name (string, unique, required) 
- description (string, optional) 
- created_at (timestamp) 
- updated_at (timestamp)



## 4. Endpoints
GET /api/members/

Description: Get one member by ID.

Success Response — 200 OK

{
  "id": 12,
  "first_name": "Kriszyle",
  "last_name": "Hugo",
  "email": "kriszyle.hugo@yahoo.com",
  "phone": "09632839451",
  "created_at": "2026-11-10T18:40:00Z",
  "updated_at": "2026-11-10T18:40:00Z"
}

Error Response — 404 Not Found

{
  "error": {
    "code": "MEMBER_NOT_FOUND",
    "message": "Member with ID 12 does not exist"
  }
}

Status Codes:
200 — Member found
400 — Invalid ID parameter
404 — Member does not exist
500 — Database query failed


GET /api/books/
Description: Get one book by ID.
Success Response — 200 OK

{
  "id": 5,
  "title": "The Pragmatic Programmer",
  "author": "Andrew Hunt",
  "isbn": "9780135957059",
  "category_id": 3,
  "published_year": 2019,
  "available_copies": 3
}

Error Response — 404 Not Found

{
  "error": {
    "code": "BOOK_NOT_FOUND",
    "message": "Book with ID 5 does not exist"
  }
}

Status Codes:
200 — Book found
400 — Invalid ID parameter
404 — Book does not exist
500 — Database query failed

POST /api/loans
Description: Create a new loan or borrow a book.
Request Body:
{
  "member_id": 12,
  "book_id": 5
}
Success Response — 201 Created
{
  "id": 88,
  "member_id": 12,
  "book_id": 5,
  "borrow_date": "2026-11-10T18:40:00Z",
  "due_date": "2026-11-24T18:40:00Z",
  "return_date": null,
  "status": "active",
  "created_at": "2026-11-10T18:40:00Z",
  "updated_at": "2026-11-10T18:40:00Z"
}
Error Response — 409 Conflict
{
  "error": {
    "code": "BOOK_NOT_AVAILABLE",
    "message": "This book has no available copies for borrowing"
  }
}
Status Codes:
201 — Loan created successfully
400 — Invalid input parameters
404 — Member or book does not exist
409 — Book has no available copies
500 — Database transaction failed


DELETE /api/categories/
Description: Delete a category.
Success Response — 204 No Content

Empty body

Status Codes:
204 — Category deleted successfully
404 — Category not found
409 — Category still contains books
500 — Database deletion failed




## 5. Authentication
Authorization: Bearer <JWT_TOKEN>

## 6. Error Handling
Error Handling

The API will use a standard error format:

{
  "error": {
    "code": "ERROR_CODE",
    "message": "Description of the error"
  }
}

## 7. Special Scenarios
1. Borrowing an Unavailable Book

Status: 409 Conflict

Code: BOOK_NOT_AVAILABLE
Message: This book has no available copies for borrowing
2. Returning an Already-Returned Loan

Status: 400 Bad Request

Code: LOAN_ALREADY_CLOSED
Message: This loan has already been marked as returned
3. Deleting a Book With Active Loans

Status: 409 Conflict

Code: ACTIVE_LOANS_EXIST
Message: Cannot delete book while active borrowing records exist
4. Duplicate Member Email

Status: 409 Conflict

Code: DUPLICATE_EMAIL
Message: A member profile with this email address already exists

## 8. Future Enhancements
The following features may be added in future versions:

JWT authentication and authorization
Role-based access control
Book reservation
Email notifications for overdue books
Advanced book searching
Library reports and statistics
Library management dashboard