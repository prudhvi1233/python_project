# python_project

import json
import os

DATA_FILE = "library.json"

# Load existing data from JSON file
def load_data():
    if os.path.exists(DATA_FILE):
        with open(DATA_FILE, "r") as f:
            return json.load(f)
    return []

# Save data to JSON file
def save_data(books):
    with open(DATA_FILE, "w") as f:
        json.dump(books, f, indent=4)

# Add a new book
def add_book(books):
    title = input("Enter book title: ")
    author = input("Enter author: ")
    year = input("Enter year: ")
    books.append({"title": title, "author": author, "year": year, "status": "available"})
    save_data(books)
    print("Book added successfully!")

# View all books
def view_books(books):
    if not books:
        print("\nNo books in the library.\n")
        return
    print("\n--- Book List ---")
    for i, book in enumerate(books, start=1):
        print(f"{i}. {book['title']} by {book['author']} ({book['year']}) - {book['status']}")
    print("-----------------\n")

# Search for a book
def search_book(books):
    keyword = input("Enter title or author to search: ").lower()
    results = [book for book in books if keyword in book['title'].lower() or keyword in book['author'].lower()]
    if results:
        print("\n--- Search Results ---")
        for book in results:
            print(f"{book['title']} by {book['author']} ({book['year']}) - {book['status']}")
        print("-----------------------\n")
    else:
        print("No matching books found.\n")

# Issue or return a book
def update_status(books):
    if not books:
        print("No books available to issue/return.")
        return
    view_books(books)
    try:
        choice = int(input("Enter book number to issue/return: ")) - 1
        if 0 <= choice < len(books):
            books[choice]['status'] = "issued" if books[choice]['status'] == "available" else "available"
            save_data(books)
            print("Book status updated!\n")
        else:
            print("Invalid choice.\n")
    except ValueError:
        print("Invalid input.\n")

# Delete a book
def delete_book(books):
    if not books:
        print("No books to delete.")
        return
    view_books(books)
    try:
        choice = int(input("Enter book number to delete: ")) - 1
        if 0 <= choice < len(books):
            removed = books.pop(choice)
            save_data(books)
            print(f"Book '{removed['title']}' deleted successfully!\n")
        else:
            print("Invalid choice.\n")
    except ValueError:
        print("Invalid input.\n")

# Main menu
def main():
    books = load_data()
    while True:
        print("\n--- Library Book Tracker ---")
        print("1. Add Book")
        print("2. View Books")
        print("3. Search Book")
        print("4. Issue/Return Book")
        print("5. Delete Book")
        print("6. Exit")
        choice = input("Enter your choice: ")
        
        if choice == '1':
            add_book(books)
        elif choice == '2':
            view_books(books)
        elif choice == '3':
            search_book(books)
        elif choice == '4':
            update_status(books)
        elif choice == '5':
            delete_book(books)
        elif choice == '6':
            print("Goodbye!")
            break
        else:
            print("Invalid choice. Try again.")

if __name__ == "__main__":
    main()

