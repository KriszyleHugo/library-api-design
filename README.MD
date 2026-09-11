Reflection Questions
1. What was the most challenging part of designing this API?
The most challenging part was deciding the correct endpoints and how the resources should be connected. I needed to make sure that members, books, loans, and categories had clear relationships and that each endpoint had a proper purpose.

2. Which endpoint was the hardest to design and why?

The hardest endpoint was POST /api/loans because borrowing a book involves more than simply creating a record. The system needs to check if the member and book exist and if the book has available copies. 

3. How did you decide which operations should be nested resources vs. top-level resources?

I used top-level resources for the main resources such as members, books, loans, and categories because they can exist independently. I used nested resources when the operation is directly related to another resource. 

4. If you had to add a "Reviews" feature (users can review books), how would you design those endpoints?

I would create reviews as a separate resource because reviews contain their own information such as the review ID, member ID, book ID, rating, comment, and date.

The endpoints could be:

GET    /api/reviews
GET    /api/reviews/:id
POST   /api/reviews
PUT    /api/reviews/:id
PATCH  /api/reviews/:id
DELETE /api/reviews/:id


5. What is one thing you would do differently if you started over?

If I started over, I would plan the resources and relationships first before creating the endpoints. This would make the API design more organized and would help avoid changing endpoints later.