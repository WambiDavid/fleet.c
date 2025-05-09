#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    char name[50];
    char license[20];
} Driver;

typedef struct {
    int vehicleID;
    Driver driver;
    int deliveries[7];
    double averageDeliveries;
    char performance[15];
} Vehicle;

void inputVehicleData(Vehicle *v);
double calculateAverageDeliveries(int deliveries[ ]);
void assignPerformance(Vehicle *v);
void displayVehicleProfile(Vehicle v);

int main() {
    int n;
    printf("Enter number of vehicles (max 5): ");
    scanf("%d", &n);

    if (n < 1 || n > 5) {
        printf("Invalid number of vehicles\n");
        return 1;
    }

    Vehicle * num =(Vehicle*) malloc(n * sizeof(Vehicle));
    if (num == NULL) {
        printf("Memory allocation failed\n");
        return 1;
    }

    for (int i = 0; i < n; i++) {
        printf("Vehicle %d:\n", i + 1);
        inputVehicleData(&num[i]);
        num[i].averageDeliveries = calculateAverageDeliveries(num[i].deliveries);
        assignPerformance(&num[i]);
    }

    printf("\n");
    for (int i = 0; i < n; i++) {
        displayVehicleProfile(num[i]);
    }

    free(num);
    return 0;
}

void inputVehicleData(Vehicle *v) {
    printf("Enter Vehicle ID: ");
    scanf("%d", &v->vehicleID);
    getchar();

    printf("Enter Driver Name: ");
    fgets(v->driver.name, sizeof(v->driver.name), stdin);
    v->driver.name[strcspn(v->driver.name, "\n")] = '\0';

    printf("Enter License Number: ");
  
      fgets(v->driver.license, sizeof(v->driver.license), stdin);
 
       v->driver.license[strcspn(v->driver.license, "\n")] = '\0';

    printf("Enter deliveries for 7 days:\n");
    for (int i = 0; i < 7; i++) {
        printf("Day %d: ", i + 1);
        scanf("%d", &v->deliveries[i]);
    }
}

double calculateAverageDeliveries(int deliveries[]) {
    int total = 0;
    for (int i = 0; i < 7; i++) {
        total += deliveries[i];
    }
    return (double)total / 7;
}

void assignPerformance(Vehicle *v) {
    double avg = v->averageDeliveries;

    if (avg >= 5.0)
        strcpy(v->performance, "Excellent");
    else if (avg >= 3.0)
        strcpy(v->performance, "Good");
    else if (avg >= 1.0)
        strcpy(v->performance, "Average");
    else
        strcpy(v->performance, "Poor");
}

void displayVehicleProfile(Vehicle v) {
   
     printf("Vehicle ID: %d\n", v.vehicleID);
    printf("Driver: %s\n", v.driver.name);
    printf("License: %s\n", v.driver.license);
    printf("Deliveries: ");
    for (int i = 0; i < 7; i++) {
        printf("[%d]%s", v.deliveries[i], i < 6 ? ", " : "\n");
    }
    printf("Average Deliveries: [%.2f]\n", v.averageDeliveries);
    printf("Performance: [%s]\n", v.performance);
}
