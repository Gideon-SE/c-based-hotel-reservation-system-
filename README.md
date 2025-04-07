#include <stdio.h>
#include <string.h>

#define MAX_ROOMS 10

// Structure to hold room information
typedef struct {
    int roomNumber;
    int isAvailable;
    char guestName[50];
} Room;

Room hotelRooms[MAX_ROOMS];

// Function to initialize the hotel rooms
void initializeRooms() {
    for (int i = 0; i < MAX_ROOMS; i++) {
        hotelRooms[i].roomNumber = i + 1;
        hotelRooms[i].isAvailable = 1;  // All rooms are available initially
        strcpy(hotelRooms[i].guestName, "");
    }
}

// Function to check room availability
void checkAvailability() {
    printf("Room Number | Availability\n");
    for (int i = 0; i < MAX_ROOMS; i++) {
        printf("%d\t\t%s\n", hotelRooms[i].roomNumber, hotelRooms[i].isAvailable ? "Available" : "Occupied");
    }
}

// Function to reserve a room
void reserveRoom() {
    int roomNumber;
    char guestName[50];

    printf("Enter room number to reserve: ");
    scanf("%d", &roomNumber);
    getchar();  // To consume newline left by scanf

    if (roomNumber < 1 || roomNumber > MAX_ROOMS) {
        printf("Invalid room number.\n");
        return;
    }

    if (!hotelRooms[roomNumber - 1].isAvailable) {
        printf("Sorry, Room %d is already reserved.\n", roomNumber);
        return;
    }

    printf("Enter guest name: ");
    fgets(guestName, 50, stdin);
    guestName[strcspn(guestName, "\n")] = 0;  // Remove newline character

    hotelRooms[roomNumber - 1].isAvailable = 0;
    strcpy(hotelRooms[roomNumber - 1].guestName, guestName);

    printf("Room %d successfully reserved for %s.\n", roomNumber, guestName);
}

// Function to cancel a reservation
void cancelReservation() {
    int roomNumber;

    printf("Enter room number to cancel reservation: ");
    scanf("%d", &roomNumber);

    if (roomNumber < 1 || roomNumber > MAX_ROOMS) {
        printf("Invalid room number.\n");
        return;
    }

    if (hotelRooms[roomNumber - 1].isAvailable) {
        printf("No reservation found for Room %d.\n", roomNumber);
        return;
    }

    hotelRooms[roomNumber - 1].isAvailable = 1;
    strcpy(hotelRooms[roomNumber - 1].guestName, "");

    printf("Reservation for Room %d has been cancelled.\n", roomNumber);
}

int main() {
    int choice;

    initializeRooms();

    while (1) {
        printf("\nHotel Reservation System\n");
        printf("1. Check Room Availability\n");
        printf("2. Reserve a Room\n");
        printf("3. Cancel a Reservation\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                checkAvailability();
                break;
            case 2:
                reserveRoom();
                break;
            case 3:
                cancelReservation();
                break;
            case 4:
                printf("Exiting the system.\n");
                return 0;
            default:
                printf("Invalid choice. Please try again.\n");
        }
    }
}
