#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Define a structure for a bank customer
typedef struct Customer {
    int accountNumber;
    char name[100];
    float balance;
    struct Customer* next;
} Customer;
void createAccount(Customer** head, int accountNumber, char name[], float initialDeposit);
void deposit(Customer* head, int accountNumber, float amount);
void withdraw(Customer* head, int accountNumber, float amount);
void checkBalance(Customer* head, int accountNumber);
Customer* findCustomer(Customer* head, int accountNumber);
void displayCustomers(Customer* head);

// Create a new account
void createAccount(Customer** head, int accountNumber, char name[], float initialDeposit) {
    Customer* newCustomer = (Customer*)malloc(sizeof(Customer));
    newCustomer->accountNumber = accountNumber;
    strcpy(newCustomer->name, name);
    newCustomer->balance = initialDeposit;
    newCustomer->next = *head;
    *head = newCustomer;
    printf("Account created successfully!\n");
}

// Deposit money into an account
void deposit(Customer* head, int accountNumber, float amount) {
    Customer* customer = findCustomer(head, accountNumber);
    if (customer) {
        customer->balance += amount;
        printf("Deposit successful! New balance: %.2f\n", customer->balance);
    } else {
        printf("Account not found.\n");
    }
}

// Withdraw money from an account
void withdraw(Customer* head, int accountNumber, float amount) {
    Customer* customer = findCustomer(head, accountNumber);
    if (customer) {
        if (customer->balance >= amount) {
            customer->balance -= amount;
            printf("Withdrawal successful! New balance: %.2f\n", customer->balance);
        } else {
            printf("Insufficient balance.\n");
        }
    } else {
        printf("Account not found.\n");
    }
}

// Check the balance of an account
void checkBalance(Customer* head, int accountNumber) {
    Customer* customer = findCustomer(head, accountNumber);
    if (customer) {
        printf("Account Balance for %s: %.2f\n", customer->name, customer->balance);
    } else {
        printf("Account not found.\n");
    }
}

// Find a customer by account number
Customer* findCustomer(Customer* head, int accountNumber) {
    Customer* current = head;
    while (current != NULL) {
        if (current->accountNumber == accountNumber) {
            return current;
        }
        current = current->next;
    }
    return NULL;
}

// Display all customers
void displayCustomers(Customer* head) {
    Customer* current = head;
    while (current != NULL) {
        printf("Account Number: %d, Name: %s, Balance: %.2f\n", current->accountNumber, current->name, current->balance);
        current = current->next;
    }
}
int main() {
    Customer* head = NULL;
    int choice, accountNumber;
    char name[100];
    float amount;

    while (1) {
        printf("\nBank Management System\n");
        printf("1. Create Account\n");
        printf("2. Deposit Money\n");
        printf("3. Withdraw Money\n");
        printf("4. Check Balance\n");
        printf("5. Display All Accounts\n");
        printf("6. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter Account Number: ");
                scanf("%d", &accountNumber);
                printf("Enter Name: ");
                scanf("%s", name);
                printf("Enter Initial Deposit: ");
                scanf("%f", &amount);
                createAccount(&head, accountNumber, name, amount);
                break;
            case 2:
                printf("Enter Account Number: ");
                scanf("%d", &accountNumber);
                printf("Enter Amount to Deposit: ");
                scanf("%f", &amount);
                deposit(head, accountNumber, amount);
                break;
            case 3:
                printf("Enter Account Number: ");
                scanf("%d", &accountNumber);
                printf("Enter Amount to Withdraw: ");
                scanf("%f", &amount);
                withdraw(head, accountNumber, amount);
                break;
            case 4:
                printf("Enter Account Number: ");
                scanf("%d", &accountNumber);
                checkBalance(head, accountNumber);
                break;
            case 5:
                displayCustomers(head);
                break;
            case 6:
                exit(0);
            default:
                printf("Invalid choice!\n");
        }
    }
    return 0;
}


