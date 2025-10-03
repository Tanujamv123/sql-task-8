
CREATE TABLE Category (
    CategoryID INT AUTO_INCREMENT PRIMARY KEY,
    CategoryName VARCHAR(100) NOT NULL
);
CREATE TABLE Author (
    AuthorID INT AUTO_INCREMENT PRIMARY KEY,
    AuthorName VARCHAR(150) NOT NULL
);
CREATE TABLE Book (
    BookID INT AUTO_INCREMENT PRIMARY KEY,
    Title VARCHAR(200) NOT NULL,
    ISBN VARCHAR(20) UNIQUE,
    CategoryID INT,
    FOREIGN KEY (CategoryID) REFERENCES Category(CategoryID)
);
CREATE TABLE Member (
    MemberID INT AUTO_INCREMENT PRIMARY KEY,
    FirstName VARCHAR(100),
    LastName VARCHAR(100),
    Email VARCHAR(150) UNIQUE,
    Phone VARCHAR(20)
);
CREATE TABLE Loan1 (
    LoanID INT AUTO_INCREMENT PRIMARY KEY,
    BookID INT,
    MemberID INT,
    LoanDate DATE NOT NULL,
    ReturnDate DATE,
    FOREIGN KEY (BookID) REFERENCES Book(BookID),
    FOREIGN KEY (MemberID) REFERENCES Member(MemberID)
);
CREATE TABLE BookAuthor (
    BookID INT,
    AuthorID INT,
    PRIMARY KEY (BookID, AuthorID),
    FOREIGN KEY (BookID) REFERENCES Book(BookID),
    FOREIGN KEY (AuthorID) REFERENCES Author(AuthorID)
);
INSERT INTO Category (CategoryName) VALUES ('Fiction'),('Science'),('History');

INSERT INTO Author (AuthorName) VALUES 
('J.K. Rowling'), 
('Stephen Hawking'), 
('Yuval Noah Harari');

INSERT INTO Book (Title, ISBN, CategoryID) VALUES
('Harry Potter and the Philosopher''s Stone', '9780747532699', 1),
('Sapiens: A Brief History of Humankind', NULL, 3);

INSERT INTO BookAuthor (BookID, AuthorID) VALUES
(1, 1),
(2, 2);

INSERT INTO Member (FirstName, LastName, Email, Phone) VALUES
('Alice', 'Johnson', 'alice@example.com', NULL),
('Bob', 'Smith', 'bob@example.com', '8765432109');

INSERT INTO Loan1 (BookID, MemberID, LoanDate, ReturnDate) VALUES
(1, 1, '2025-09-01', NULL),
(2, 2, '2025-09-05', '2025-09-15');

SELECT * FROM Book;

UPDATE Book
SET CategoryID = 2
WHERE Title = 'Sapiens: A Brief History of Humankind';

UPDATE Member
SET Phone = '9876543210'
WHERE MemberID = 1 AND Phone IS NULL;

UPDATE Loan1
SET ReturnDate = '2025-09-20'
WHERE LoanID = 1 AND ReturnDate IS NULL;

DELETE FROM Loan1 WHERE LoanID = 2;

DELETE FROM BookAuthor WHERE BookID = 2;
DELETE FROM Book WHERE BookID = 2;

DELETE FROM Member
WHERE MemberID = 2
AND MemberID NOT IN (SELECT MemberID FROM Loan1 WHERE ReturnDate IS NULL);

select * from Member where MemberID=2;

DELIMITER $$

CREATE PROCEDURE AddLoan(
    IN p_BookID INT,
    IN p_MemberID INT,
    IN p_LoanDate DATE
)
BEGIN
    DECLARE v_Count INT;

    -- Check if the book is already on loan (ReturnDate still NULL)
    SELECT COUNT(*) INTO v_Count
    FROM Loan1
    WHERE BookID = p_BookID AND ReturnDate IS NULL;

    IF v_Count > 0 THEN
        SELECT CONCAT('Book ID ', p_BookID, ' is already on loan.') AS Message;
    ELSE
        INSERT INTO Loan1 (BookID, MemberID, LoanDate, ReturnDate)
        VALUES (p_BookID, p_MemberID, p_LoanDate, NULL);

        SELECT CONCAT('Loan created successfully for Book ID ', p_BookID, ' to Member ID ', p_MemberID) AS Message;
    END IF;
END $$

DELIMITER ;

CALL AddLoan(1, 1, '2025-10-03');

DELIMITER $$

CREATE FUNCTION GetLoanStatus(p_LoanID INT)
RETURNS VARCHAR(20)
DETERMINISTIC
BEGIN
    DECLARE v_Status VARCHAR(20);

    IF (SELECT COUNT(*) FROM Loan1 WHERE LoanID = p_LoanID) = 0 THEN
        SET v_Status = 'Invalid Loan ID';
    ELSEIF (SELECT ReturnDate FROM Loan1 WHERE LoanID = p_LoanID) IS NULL THEN
        SET v_Status = 'Active';
    ELSE
        SET v_Status = 'Returned';
    END IF;

    RETURN v_Status;
END $$

DELIMITER ;

SELECT GetLoanStatus(1);
SELECT GetLoanStatus(999);

DELIMITER $$

CREATE PROCEDURE DeleteMemberSafely(
    IN p_MemberID INT
)
BEGIN
    DECLARE v_Count INT;

    -- Count active loans for this member
    SELECT COUNT(*) INTO v_Count
    FROM Loan1
    WHERE MemberID = p_MemberID AND ReturnDate IS NULL;

    IF v_Count > 0 THEN
        SELECT CONCAT('Cannot delete Member ID ', p_MemberID, 
                      ' – they still have active loans.') AS Message;
    ELSE
        DELETE FROM Member WHERE MemberID = p_MemberID;
        SELECT CONCAT('Member ID ', p_MemberID, ' deleted successfully.') AS Message;
    END IF;
END $$

DELIMITER ;

CALL DeleteMemberSafely(2);

DELIMITER $$

CREATE FUNCTION GetCategoryBookCount(p_CategoryID INT)
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE v_Count INT;

    -- Check if category exists
    IF (SELECT COUNT(*) FROM Category WHERE CategoryID = p_CategoryID) = 0 THEN
        SET v_Count = -1; -- Invalid category
    ELSE
        SELECT COUNT(*) INTO v_Count
        FROM Book
        WHERE CategoryID = p_CategoryID;
    END IF;

    RETURN v_Count;
END $$

DELIMITER ;

SELECT GetCategoryBookCount(1);  
SELECT GetCategoryBookCount(999);


