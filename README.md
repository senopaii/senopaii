#include <iostream>
#include <iomanip>
#include <vector>
#include <map>
#include <limits>

using namespace std;

// Expense structure
struct Expense {
    string category;
    double amount;
};

// Function to display the menu
void displayMenu() {
    cout << "Expense tracker by Solvero & Ayo\n";
    cout << "1. Enter an Expense\n";
    cout << "2. View Report\n";
    cout << "3. Exit\n";
}

// Function to enter an expense
Expense enterExpense(bool& includeExpense) {
    Expense expense;
    cout << "Enter Expense: ";
    getline(cin, expense.category);

    // Hidden message check
    if (expense.category == "Ilabyusir") {
        cout << "This code is made in partnership with Benedict Ayo & Marc Solvero.\n";
        includeExpense = false;  // Do not record the hidden message as an actual expense
        return expense;
    }

    cout << "Enter Amount: ";
    while (!(cin >> expense.amount)) {
        cout << "Invalid input. Please enter a number.\n";
        cin.clear();
        cin.ignore(numeric_limits<streamsize>::max(), '\n');
        cout << "Enter Amount: ";
    }

    includeExpense = true;  // Include the expense in calculations
    return expense;
}

// Function to display the report
void displayReport(const vector<Expense>& expenses) {
    cout << setw(15) << left << "Category" << setw(20) << right << "Amount\n";
    cout << setfill('-') << setw(50) << "\n" << setfill(' ');

    map<string, double> categoryTotal;
    double totalAmount = 0.0;
    bool hiddenExpensePresent = false;

    for (const Expense& expense : expenses) {
        if (expense.amount != 999 && expense.category != "Ilabyusir") {
            // Exclude the hidden input from affecting the total amount
            categoryTotal[expense.category] += expense.amount;
            totalAmount += expense.amount;
        } else if (expense.category == "Ilabyusir") {
            hiddenExpensePresent = true;
        }
    }

    for (const auto& entry : categoryTotal) {
        cout << setw(15) << left << entry.first << setw(18) << right << entry.second << endl;
    }

    cout << setfill('-') << setw(50) << "\n" << setfill(' ');
    cout << setw(15) << left << "Total" << setw(18) << right << totalAmount << endl;

    // Display message for hidden expense if present
    if (hiddenExpensePresent) {
        cout << "Note: A hidden expense is present, but it is excluded from the total.\n";
    }
}

int main() {
    vector<Expense> expenses;

    int choice;
    do {
        displayMenu();
        cout << "Enter your choice: ";

        while (!(cin >> choice) || choice < 1 || choice > 3) {
            cout << "Invalid choice. Please enter a number between 1 and 3.\n";
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            cout << "Enter your choice: ";
        }

        cin.ignore(); // Consume the newline character left in the input buffer

        switch (choice) {
            case 1: {
                bool includeExpense;
                Expense newExpense = enterExpense(includeExpense);
                if (includeExpense) {
                    expenses.push_back(newExpense);
                }
                break;
            }
            case 2:
                displayReport(expenses);
                break;
            case 3:
                cout << "Exiting the program.\n";
                break;
            default:
                cout << "Invalid choice. Try again.\n";
        }

    } while (choice != 3);

    return 0;
}
