# University-Management-System
It is a small code .where i make a demo university Management system . It  is a small Project.
<br>
Author: Pranto Paul
#include <stdio.h>
int max(int a, int b) {
    return (a > b) ? a : b;
}

void knapsack(int numDepartments, int availableSeats[], int numCandidates, int scores[], int departmentScores[][100]) {
    int dp[numDepartments + 1][numCandidates + 1];

    for (int i = 0; i <= numDepartments; i++) {
        for (int j = 0; j <= numCandidates; j++) {
            if (i == 0 || j == 0)
                dp[i][j] = 0;
            else if (availableSeats[i - 1] <= j)++
                dp[i][j] = max(departmentScores[i - 1][availableSeats[i - 1] - 1] + dp[i - 1][j - availableSeats[i - 1]], dp[i - 1][j]);
            else
                dp[i][j] = dp[i - 1][j];
        }
    }

    int selectedStudents[numDepartments]; 
    for (int i = 0; i < numDepartments; i++)
        selectedStudents[i] = 0;

    int i = numDepartments, j = numCandidates;
    while (i > 0 && j > 0) {
        if (dp[i][j] != dp[i - 1][j]) {
            selectedStudents[i - 1] = 1;
            j -= availableSeats[i - 1];
        }
        i--;
    }

    
   printf("Selected students for each department:\n");
for (int i = 0; i < numDepartments; i++) {
    printf("Department %d: ", i + 1);
    if (selectedStudents[i]) {
        for (int j = 0; j < availableSeats[i]; j++) {
            printf("%d ", departmentScores[i][j]);
        }
    } else {
        printf("None selected");
    }
    printf("\n");
}

}

int main() {
    int numDepartments, numCandidates;
    printf("Enter the number of departments: ");
    scanf("%d", &numDepartments);

    int availableSeats[numDepartments];
    for (int i = 0; i < numDepartments; i++) {
        printf("Enter the number of available seats for Department %d: ", i + 1);
        scanf("%d", &availableSeats[i]);
    }

    printf("Enter the total number of candidates: ");
    scanf("%d", &numCandidates);

    int scores[numCandidates];
    printf("Enter the scores of the candidates:\n");
    for (int i = 0; i < numCandidates; i++) {
        printf("Candidate %d: ", i + 1);
        scanf("%d", &scores[i]);
    }

    int departmentScores[numDepartments][100];
    printf("Enter the scores of candidates for each department:\n");
    for (int i = 0; i < numDepartments; i++) {
        printf("Department %d:\n", i + 1);
        for (int j = 0; j < availableSeats[i]; j++) {
            printf("Candidate %d: ", j + 1);
            scanf("%d", &departmentScores[i][j]);
        }
    }

    knapsack(numDepartments, availableSeats, numCandidates, scores, departmentScores);

    return 0;
}
