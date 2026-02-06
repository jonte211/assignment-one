# assignment-one
assignment 1
#include <iostream>
#include <vector>
#include <string>

using namespace std;

// Book Class
class Book {
private:
    string title;
    string author;
    string ISBN;
    bool isAvailable;
public:
    Book(string t, string a, string i) {
        title = t;
        author = a;
        ISBN = i;
        isAvailable = true;
    }

    string getTitle() { return title; }
    string getAuthor() { return author; }
    string getISBN() { return ISBN; }
    bool available() { return isAvailable; }

    void borrowBook() { isAvailable = false; }
    void returnBook() { isAvailable = true; }

    void displayBook() {
        cout << "Title: " << title
             << ", Author: " << author
             << ", ISBN: " << ISBN
             << ", Available: " << (isAvailable ? "Yes" : "No") << endl;
    }
};

// User Class
class User {
private:
    string name;
    string userID;
    vector<Book*> borrowedBooks;
public:
    User(string n, string id) {
        name = n;
        userID = id;
    }

    void borrowBook(Book &book) {
        if (book.available()) {
            borrowedBooks.push_back(&book);
            book.borrowBook();
            cout << name << " borrowed " << book.getTitle() << endl;
        } else {
            cout << "Book is not available.\n";
        }
    }

    void returnBook(Book &book) {
        for (int i = 0; i < borrowedBooks.size(); i++) {
            if (borrowedBooks[i]->getISBN() == book.getISBN()) {
                borrowedBooks.erase(borrowedBooks.begin() + i);
                book.returnBook();
                cout << name << " returned " << book.getTitle() << endl;
                return;
            }
        }
        cout << name << " did not borrow this book.\n";
    }

    void viewBorrowedBooks() {
        cout << name << "'s Borrowed Books:\n";
        for (auto b : borrowedBooks) {
            cout << "- " << b->getTitle() << endl;
        }
    }
};

// Library Class
class Library {
private:
    vector<Book> books;
public:
    void addBook(Book book) {
        books.push_back(book);
        cout << "Book added: " << book.getTitle() << endl;
    }

    void removeBook(string ISBN) {
        for (int i = 0; i < books.size(); i++) {
            if (books[i].getISBN() == ISBN) {
                cout << "Book removed: " << books[i].getTitle() << endl;
                books.erase(books.begin() + i);
                return;
            }
        }
        cout << "Book not found.\n";
    }

    void searchBookByTitle(string title) {
        cout << "Search results for title '" << title << "':\n";
        for (auto &b : books) {
            if (b.getTitle() == title) b.displayBook();
        }
    }

    void listAvailableBooks() {
        cout << "Available books:\n";
        for (auto &b : books) {
            if (b.available()) b.displayBook();
        }
    }
};

// Main Program
int main() {
    Library lib;

    // Adding books
    lib.addBook(Book("C++ Basics", "Bjarne Stroustrup", "001"));
    lib.addBook(Book("Data Structures", "Mark Allen", "002"));
    lib.addBook(Book("Algorithms", "Robert Sedgewick", "003"));

    // Users
    User user1("Alice", "U001");
    User user2("Bob", "U002");

    // Borrowing books
    user1.borrowBook(lib.books[0]);
    user2.borrowBook(lib.books[0]); // Already borrowed

    // Returning books
    user1.returnBook(lib.books[0]);
    user2.borrowBook(lib.books[0]); // Now can borrow

    // Search and list books
    lib.searchBookByTitle("Data Structures");
    lib.listAvailableBooks();

    return 0;
}
#include "LibraryManagementSystem.cpp"

void runTests() {
    Library lib;
    Book b1("Test Book 1", "Author 1", "T001");
    lib.addBook(b1);

    User testUser("TestUser", "U100");
    testUser.borrowBook(b1); // should borrow successfully
    testUser.returnBook(b1); // should return successfully
    testUser.borrowBook(b1); // borrow again
}

int main() {
    runTests();
    cout << "All tests ran successfully.\n";
    return 0;
}
g++ LibraryManagementSystem.cpp -o library
g++ LibraryTest.cpp -o testLibrary
./library    # Run main program
./testLibrary   # Run tests
