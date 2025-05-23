# Library-Management-system
# The main goal of this project is to create a simple and efficient Library Management System using the C programming language. The idea is to help manage common library tasks like adding new books, keeping track of issued books, returning books, and maintaining records in an organized way. 
#include <stdio.h>
#include <string.h>

#define MAX_BOOKS 100

typedef struct {
    int id;
    char title[50];
    char author[50];
    int issued;
} Book;

Book books[MAX_BOOKS];
int bookCount = 0;

void addBook() {
    if (bookCount == MAX_BOOKS) {
        printf("Oops! Library is full. Can't add more books.\n");
        return;
    }
    printf("Please enter the Book ID: ");
    scanf("%d", &books[bookCount].id);
    getchar();

    printf("Enter the Book Title: ");
    fgets(books[bookCount].title, sizeof(books[bookCount].title), stdin);
    books[bookCount].title[strcspn(books[bookCount].title, "\n")] = 0;

    printf("Enter Author Name: ");
    fgets(books[bookCount].author, sizeof(books[bookCount].author), stdin);
    books[bookCount].author[strcspn(books[bookCount].author, "\n")] = 0;

    books[bookCount].issued = 0;
    bookCount++;

    printf("Book added! Thanks for updating the library.\n");
}

void showBooks() {
    if (bookCount == 0) {
        printf("No books found. The library is empty right now.\n");
        return;
    }
    printf("\nHere are the books in the library:\n");
    printf("ID\tTitle\t\tAuthor\t\tStatus\n");
    for (int i = 0; i < bookCount; i++) {
        printf("%d\t%-15s\t%-15s\t%s\n",
            books[i].id,
            books[i].title,
            books[i].author,
            books[i].issued ? "Issued" : "Available");
    }
}

int findBookById(int id) {
    for (int i = 0; i < bookCount; i++) {
        if (books[i].id == id) return i;
    }
    return -1;
}

void issueBook() {
    int id;
    printf("Enter the Book ID you want to issue: ");
    scanf("%d", &id);

    int index = findBookById(id);
    if (index == -1) {
        printf("Sorry, we don't have a book with ID %d.\n", id);
    } else if (books[index].issued) {
        printf("This book is already issued.\n");
    } else {
        books[index].issued = 1;
        printf("Book issued: %s\n", books[index].title);
    }
}

void returnBook() {
    int id;
    printf("Enter the Book ID you want to return: ");
    scanf("%d", &id);

    int index = findBookById(id);
    if (index == -1) {
        printf("Book with ID %d not found.\n", id);
    } else if (!books[index].issued) {
        printf("This book was not issued.\n");
    } else {
        books[index].issued = 0;
        printf("Book returned: %s\n", books[index].title);
    }
}

void mainMenu() {
    int choice;
    while (1) {
        printf("\n--- Library Menu ---\n");
        printf("1. Add Book\n");
        printf("2. Show Books\n");
        printf("3. Issue Book\n");
        printf("4. Return Book\n");
        printf("5. Exit\n");
        printf("Enter your choice: ");
        
        if (scanf("%d", &choice) != 1) {
            while(getchar() != '\n'); // clear invalid input
            printf("Invalid input, please enter a number.\n");
            continue;
        }

        switch (choice) {
            case 1: addBook(); break;
            case 2: showBooks(); break;
            case 3: issueBook(); break;
            case 4: returnBook(); break;
            case 5: printf("Goodbye!\n"); return;
            default: printf("Please enter a valid option (1-5).\n");
        }
    }
}

int main() {
    mainMenu();
    return 0;
}
