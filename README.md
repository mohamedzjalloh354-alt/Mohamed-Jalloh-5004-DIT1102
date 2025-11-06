# Mohamed-Jalloh-5004-DIT1102
Object Oriented Programming Assignment

##Operation.py
[operation.py](https://github.com/user-attachments/files/23377600/operation.py)
# Data structures
books = {}
members = []
genres = ("Fiction", "Non-Fiction", "Sci-Fi", "Biography", "Romance")

# ---------------- CORE FUNCTIONS ---------------- #

def add_book(isbn, title, author, genre, total_copies):
    """Add a new book if ISBN is unique and genre is valid."""
    if isbn in books:
        return "Book already exists."
    if genre not in genres:
        return "Invalid genre."
    books[isbn] = {
        "title": title,
        "author": author,
        "genre": genre,
        "total_copies": total_copies,
        "available_copies": total_copies
    }
    return "Book added successfully."

def add_member(member_id, name, email):
    """Add a member if ID is unique."""
    for member in members:
        if member["member_id"] == member_id:
            return "Member already exists."
    members.append({
        "member_id": member_id,
        "name": name,
        "email": email,
        "borrowed_books": []
    })
    return "Member added successfully."

def search_books(keyword):
    """Search by title or author."""
    result = []
    for isbn, details in books.items():
        if keyword.lower() in details["title"].lower() or keyword.lower() in details["author"].lower():
            result.append((isbn, details))
    return result

def update_book(isbn, title=None, author=None, genre=None, total_copies=None):
    """Update book details."""
    if isbn not in books:
        return "Book not found."
    if genre and genre not in genres:
        return "Invalid genre."
    if title: books[isbn]["title"] = title
    if author: books[isbn]["author"] = author
    if genre: books[isbn]["genre"] = genre
    if total_copies: books[isbn]["total_copies"] = total_copies
    return "Book updated successfully."

def delete_book(isbn):
    """Delete a book only if no borrowed copies exist."""
    if isbn not in books:
        return "Book not found."
    for member in members:
        if isbn in member["borrowed_books"]:
            return "Book currently borrowed; cannot delete."
    del books[isbn]
    return "Book deleted successfully."

def borrow_book(member_id, isbn):
    """Member can borrow up to 3 books."""
    for member in members:
        if member["member_id"] == member_id:
            if isbn not in books:
                return "Book not found."
            if books[isbn]["available_copies"] == 0:
                return "No copies available."
            if len(member["borrowed_books"]) >= 3:
                return "Borrow limit reached."
            member["borrowed_books"].append(isbn)
            books[isbn]["available_copies"] -= 1
            return "Book borrowed successfully."
    return "Member not found."

def return_book(member_id, isbn):
    """Return borrowed book."""
    for member in members:
        if member["member_id"] == member_id:
            if isbn in member["borrowed_books"]:
                member["borrowed_books"].remove(isbn)
                books[isbn]["available_copies"] += 1
                return "Book returned successfully."
            return "Book not borrowed by member."
    return "Member not found."

def delete_member(member_id):
    """Delete a member only if no borrowed books."""
    for member in members:
        if member["member_id"] == member_id:
            if member["borrowed_books"]:
                return "Cannot delete member with borrowed books."
            members.remove(member)
            return "Member deleted successfully."
    return "Member not found."



    ##Demo.py
    [demo.py](https://github.com/user-attachments/files/23377615/demo.py)
from operations import *

# Demonstrate system usage
print(add_book("978-001", "Python Basics", "John Doe", "Fiction", 5))
print(add_book("978-002", "Data Science 101", "Jane Smith", "Non-Fiction", 3))

print(add_member("M001", "Alice Brown", "alice@example.com"))
print(add_member("M002", "Bob White", "bob@example.com"))

print(search_books("Python"))

print(borrow_book("M001", "978-001"))
print(borrow_book("M001", "978-002"))
print(return_book("M001", "978-001"))

print(update_book("978-002", title="Data Science Advanced"))
print(delete_book("978-001"))
print(delete_member("M002"))


##Test.py
[test.py](https://github.com/user-attachments/files/23377657/test.py)
from operations import *

# Test adding book
assert add_book("978-003", "AI Revolution", "Tom Smart", "Sci-Fi", 2) == "Book added successfully."
assert add_book("978-003", "AI Revolution", "Tom Smart", "Sci-Fi", 2) == "Book already exists."

# Test adding member
assert add_member("M003", "Charlie", "charlie@example.com") == "Member added successfully."
assert add_member("M003", "Charlie", "charlie@example.com") == "Member already exists."

# Test borrowing
assert borrow_book("M003", "978-003") == "Book borrowed successfully."

# Test returning
assert return_book("M003", "978-003") == "Book returned successfully."

# Test searching
assert len(search_books("AI")) == 1

print("All tests passed successfully!")


##UML Diagram
![WhatsApp Image 2025-11-06 at 16 18 50_de7f4d48](https://github.com/user-attachments/assets/4caf1282-84ee-407d-9f71-1128c4c2a6b9)


##Rationale
[Introduction.docx](https://github.com/user-attachments/files/23377734/Introduction.docx)

