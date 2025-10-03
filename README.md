data sets are maintained in online mysql platform.
created and manintained database. use of joins filtration and projection done in last tasks. here making use of stored procedure and functions and output is stored as screen shot in seperate file
(1st procedure example- procedure will insert a new loan only if the book is not already on loan (i.e., ReturnDate IS NULL).Otherwise, it will output a message saying the book is already borrowed.)
(1st function example-function will take a LoanID and return:'Active' if not returned (ReturnDate IS NULL),'Returned' if ReturnDate exist,'Invalid Loan ID' if no such loan exists..)
(2nd procedure- This procedure deletes a member only if they have no active loans,If they still have books on loan (ReturnDate IS NULL), the deletion is blocked and a message is returned.)
(2nd function-This function takes a CategoryID and returns the number of books in that category,If no such category exists, it returns -1.)
