# Student-Help-desk-system-
#include <stdio.h>
#include <string.h>

#define MAX_TICKETS 100
#define STR_LEN 100

// Define the Ticket structure
typedef struct {
    int id;
    char studentName[STR_LEN];
    char issue[STR_LEN];
    char status[20]; // Open, In Progress, Resolved
} Ticket;

Ticket helpDesk[MAX_TICKETS];
int ticketCount = 0;

// Function to create a new ticket
void createTicket() {
    if (ticketCount >= MAX_TICKETS) {
        printf("\n[Error] System full. Cannot accept more tickets.\n");
        return;
    }

    Ticket t;
    t.id = ticketCount + 101; // Starting IDs from 101
    
    printf("\nEnter Student Name: ");
    getchar(); // Clear buffer
    fgets(t.studentName, STR_LEN, stdin);
    t.studentName[strcspn(t.studentName, "\n")] = 0; // Remove newline

    printf("Describe the Issue: ");
    fgets(t.issue, STR_LEN, stdin);
    t.issue[strcspn(t.issue, "\n")] = 0;

    strcpy(t.status, "Open");

    helpDesk[ticketCount] = t;
    ticketCount++;

    printf("\n[Success] Ticket #%d created for %s\n", t.id, t.studentName);
}

// Function to display all tickets
void displayTickets() {
    if (ticketCount == 0) {
        printf("\nNo tickets found in the system.\n");
        return;
    }

    printf("\n--- Current Student Help Desk Tickets ---\n");
    printf("%-5s | %-20s | %-30s | %-10s\n", "ID", "Student", "Issue", "Status");
    printf("----------------------------------------------------------------------\n");

    for (int i = 0; i < ticketCount; i++) {
        printf("%-5d | %-20s | %-30s | %-10s\n", 
               helpDesk[i].id, helpDesk[i].studentName, helpDesk[i].issue, helpDesk[i].status);
    }
}

// Function to update ticket status
void updateStatus() {
    int id, found = 0;
    printf("\nEnter Ticket ID to update: ");
    scanf("%d", &id);

    for (int i = 0; i < ticketCount; i++) {
        if (helpDesk[i].id == id) {
            printf("Current Status: %s. Enter new status (1-InProgress, 2-Resolved): ", helpDesk[i].status);
            int choice;
            scanf("%d", &choice);
            if (choice == 1) strcpy(helpDesk[i].status, "InProgress");
            else if (choice == 2) strcpy(helpDesk[i].status, "Resolved");
            
            printf("\n[Success] Ticket #%d updated.\n", id);
            found = 1;
            break;
        }
    }
    if (!found) printf("\n[Error] Ticket ID %d not found.\n", id);
}

int main() {
    int choice;

    while (1) {
        printf("\n=== STUDENT HELP DESK SYSTEM ===");
        printf("\n1. Raise a New Ticket");
        printf("\n2. View All Tickets");
        printf("\n3. Update Ticket Status");
        printf("\n4. Exit");
        printf("\nSelect an option: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: createTicket(); break;
            case 2: displayTickets(); break;
            case 3: updateStatus(); break;
            case 4: return 0;
            default: printf("\nInvalid choice. Try again.\n");
        }
    }
    return 0;
}
