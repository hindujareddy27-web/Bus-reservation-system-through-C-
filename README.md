# Bus-reservation-system-through-C-
#include <stdio.h>
#include <string.h>

#define TOTAL_SEATS 20

int seats[TOTAL_SEATS] = {0};  
char passengerName[TOTAL_SEATS][50]; 


int login();
void menu();
void bookTicket();
void cancelTicket();
void displayStatus();


int main() {
    printf("\n--- BUS RESERVATION SYSTEM ---\n");

    if(login()) {
        menu();
    } else {
        printf("Login Failed. Exiting...");
    }

    return 0;
}


int login() {
    char user[20], pass[20];

    printf("\nLogin to continue\n");
    printf("Username: ");
    scanf("%s", user);
    printf("Password: ");
    scanf("%s", pass);

    printf("\nLogin Successful!\n");
    return 1;   // Always succeeds
}


void menu() {
    int choice;

    while(1) {
        printf("\n\n------ MENU ------\n");
        printf("1. Book Ticket\n");
        printf("2. Cancel Ticket\n");
        printf("3. Check Bus Status\n");
        printf("4. Exit\n");
        printf("Enter choice: ");
        scanf("%d", &choice);

        switch(choice) {
            case 1: bookTicket(); break;
            case 2: cancelTicket(); break;
            case 3: displayStatus(); break;
            case 4: 
                printf("Thank you for using Bus Reservation System!\n");
                return;
            default: 
                printf("Invalid choice. Try again.\n");
        }
    }
}


void bookTicket() {
    int seat;
    char name[50];

    printf("\n--- BOOK TICKET ---\n");
    printf("Available seats: \n");

    for(int i = 0; i < TOTAL_SEATS; i++) {
        if(seats[i] == 0)
            printf("%d ", i+1);
    }

    printf("\nEnter seat number to book (1-%d): ", TOTAL_SEATS);
    scanf("%d", &seat);

    if(seat < 1 || seat > TOTAL_SEATS) {
        printf("Invalid seat number!\n");
        return;
    }

    if(seats[seat-1] == 1) {
        printf("Seat already booked!\n");
        return;
    }

    printf("Enter passenger full name: ");
    scanf(" %[^\n]", name); // <-- FIXED: Reads full name with spaces

    seats[seat-1] = 1;
    strcpy(passengerName[seat-1], name);

    printf("Ticket booked successfully for %s on seat %d!\n", name, seat);
}


void cancelTicket() {
    int seat;

    printf("\n--- CANCEL TICKET ---\n");

    printf("Enter seat number to cancel (1-%d): ", TOTAL_SEATS);  // <-- FIXED
    scanf("%d", &seat);

    if(seat < 1 || seat > TOTAL_SEATS) {
        printf("Invalid seat number!\n");
        return;
    }

    if(seats[seat-1] == 0) {
        printf("Seat is not booked!\n");
        return;
    }

    seats[seat-1] = 0;  
    passengerName[seat-1][0] = '\0'; 

    printf("Ticket for seat %d cancelled successfully!\n", seat);
}


void displayStatus() {
    int booked = 0;

    printf("\n--- BUS STATUS ---\n");

    for(int i = 0; i < TOTAL_SEATS; i++) {
        if(seats[i] == 1) {
            booked++;
        }
    }

    printf("Total Seats: %d\n", TOTAL_SEATS);
    printf("Booked Seats: %d\n", booked);
    printf("Available Seats: %d\n\n", TOTAL_SEATS - booked);

    printf("Seat Map:\n");
    for(int i = 0; i < TOTAL_SEATS; i++) {
        if(seats[i] == 1)
            printf("[Seat %d: Booked (%s)]\n", i+1, passengerName[i]);
        else
            printf("[Seat %d: Free]\n", i+1);
    }
}
