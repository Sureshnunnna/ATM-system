# ATM-system
Description:

The project implements an Automated Teller Machine (ATM) system using Object-Oriented Programming (OOP) in C++. It simulates real-life ATM functionalities, including account information management, cash deposit/withdrawal, cheque requests, and transaction records.
Features
Account Management: View balance, update user information.
Transaction Options:
Deposit cash.
Withdraw cash.
Request cheques.
Transaction history.
Database Management: Transaction and account data are securely stored in files for persistence.
Implementation Details
Classes Used:
Account: Stores account details like name, balance, and account number.
ATM: Handles transaction options like deposit, withdrawal, and cheque requests.
Database: Manages file operations for storing and retrieving data.
Concepts Used:
Encapsulation: Data hiding using private members.
Inheritance (optional): For advanced account types like savings or current accounts.
File Handling: To store and retrieve user transaction data.


#include <iostream>
#include <fstream>
#include <string>
#include <iomanip>
#include <limits>

using namespace std;

class Account {
private:
    string accountHolder;
    int accountNumber;
    double balance;

public:
    Account(string name = "Unknown", int accNum = 0, double bal = 0.0)
        : accountHolder(name), accountNumber(accNum), balance(bal) {}

    void displayInfo() {
        cout << "\nAccount Holder: " << accountHolder
             << "\nAccount Number: " << accountNumber
             << "\nBalance: $" << fixed << setprecision(2) << balance << endl;
    }

    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            cout << "Deposited: $" << amount << "\nNew Balance: $" << fixed << setprecision(2) << balance << endl;
        } else {
            cout << "Invalid deposit amount!" << endl;
        }
    }

    void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            cout << "Withdrawn: $" << amount << "\nNew Balance: $" << fixed << setprecision(2) << balance << endl;
        } else {
            cout << "Insufficient balance or invalid amount!" << endl;
        }
    }

    void saveToFile() {
        ofstream file("account_data.txt", ios::app);
        if (file.is_open()) {
            file << accountHolder << "," << accountNumber << "," << balance << endl;
            file.close();
            cout << "Account data saved successfully.\n";
        } else {
            cout << "Error opening file for saving data.\n";
        }
    }

    static void loadAccounts() {
        ifstream file("account_data.txt");
        if (file.is_open()) {
            string line;
            cout << "Account Data:\n";
            while (getline(file, line)) {
                cout << line << endl;
            }
            file.close();
        } else {
            cout << "Error opening file for loading data.\n";
        }
    }
};

int main() {
    cout << "Welcome to the ATM System\n";

    // Create a sample account
    Account user("John Doe", 12345, 500.00);

    int choice;
    do {
        cout << "\nMenu:\n"
             << "1. View Account Info\n"
             << "2. Deposit Money\n"
             << "3. Withdraw Money\n"
             << "4. Save Account Data\n"
             << "5. Load All Accounts\n"
             << "0. Exit\n"
             << "Enter your choice: ";
        
        while (!(cin >> choice)) { // Input validation for choice
            cin.clear(); // Clear the error flag
            cin.ignore(numeric_limits<streamsize>::max(), '\n'); // Ignore invalid input
            cout << "Invalid input! Please enter a number: ";
        }

        switch (choice) {
            case 1:
                user.displayInfo();
                break;
            case 2: {
                double depositAmount;
                cout << "Enter deposit amount: ";
                cin >> depositAmount;

                while (cin.fail() || depositAmount < 0) { // Input validation for deposit
                    cin.clear();
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');
                    cout << "Invalid input! Please enter a positive number for deposit: ";
                    cin >> depositAmount;
                }
                user.deposit(depositAmount);
                break;
            }
            case 3: {
                double withdrawAmount;
                cout << "Enter withdrawal amount: ";
                cin >> withdrawAmount;

                while (cin.fail() || withdrawAmount < 0) { // Input validation for withdrawal
                    cin.clear();
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');
                    cout << "Invalid input! Please enter a positive number for withdrawal: ";
                    cin >> withdrawAmount;
                }
                user.withdraw(withdrawAmount);
                break;
            }
            case 4:
                user.saveToFile();
                break;
            case 5:
                Account::loadAccounts();
                break;
            case 0:
                cout << "Exiting...\n";
                break;
            default:
                cout << "Invalid choice! Please try again.\n";
        }

        cin.ignore(numeric_limits<streamsize>::max(), '\n'); // Clear the input buffer

    } while (choice != 0);

    return 0;
}
![image](https://github.com/user-attachments/assets/6e4012c2-97e8-493b-a9c1-bf7337124aec)
![image](https://github.com/user-attachments/assets/4fc78577-8215-4382-b23f-5d39dd454b13)

