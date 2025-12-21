#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Vehicle {
    int number;
    char type[10];
    int hours;
};

void parkVehicle() {
    struct Vehicle v;
    FILE *fp = fopen("parking.dat", "ab");

    if (fp == NULL) {
        printf("File error\n");
        return;
    }

    printf("Enter vehicle number: ");
    scanf("%d", &v.number);

    printf("Enter vehicle type (Bike/Car/Truck): ");
    scanf("%s", v.type);

    printf("Enter parking hours: ");
    scanf("%d", &v.hours);

    fwrite(&v, sizeof(v), 1, fp);
    fclose(fp);

    printf("Vehicle parked successfully.\n");
}

void displayVehicles() {
    struct Vehicle v;
    FILE *fp = fopen("parking.dat", "rb");

    if (fp == NULL) {
        printf("No vehicles parked yet.\n");
        return;
    }

    printf("\nVehicle No\tType\tHours\n");
    printf("--------------------------------\n");

    while (fread(&v, sizeof(v), 1, fp)) {
        printf("%d\t\t%s\t%d\n", v.number, v.type, v.hours);
    }

    fclose(fp);
}

void calculateCharge() {
    struct Vehicle v;
    int num, charge = 0;
    FILE *fp = fopen("parking.dat", "rb");

    if (fp == NULL) {
        printf("No records found.\n");
        return;
    }

    printf("Enter vehicle number: ");
    scanf("%d", &num);

    while (fread(&v, sizeof(v), 1, fp)) {
        if (v.number == num) {
            if (strcmp(v.type, "Bike") == 0)
                charge = v.hours * 10;
            else if (strcmp(v.type, "Car") == 0)
                charge = v.hours * 20;
            else if (strcmp(v.type, "Truck") == 0)
                charge = v.hours * 30;

            printf("Total parking charge = Rs.%d\n", charge);
            fclose(fp);
            return;
        }
    }

    printf("Vehicle not found.\n");
    fclose(fp);
}

int main() {
    int choice;

    while (1) {
        printf("\n--- VEHICLE PARKING SYSTEM ---\n");
        printf("1. Park Vehicle\n");
        printf("2. Display Parked Vehicles\n");
        printf("3. Calculate Parking Charge\n");
        printf("4. Exit\n");
        printf("Enter choice: ");

        scanf("%d", &choice);

        switch (choice) {
            case 1: parkVehicle(); break;
            case 2: displayVehicles(); break;
            case 3: calculateCharge(); break;
            case 4: exit(0);
            default: printf("Invalid choice\n");
        }
    }
}
