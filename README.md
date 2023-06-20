#include <stdio.h>

void calculateAverageTimes(int processes[][3], int n, float *avgWaitingTime, float *avgTurnaroundTime) {

    int burstTimes[n];

    int priorities[n];

    int waitingTimes[n];

    int turnaroundTimes[n];

    int i, j;

    // Initialize waiting times and turnaround times

    for (i = 0; i < n; i++) {

        waitingTimes[i] = 0;

        turnaroundTimes[i] = 0;

    }

    // Copy burst times and priorities into separate arrays

    for (i = 0; i < n; i++) {

        burstTimes[i] = processes[i][1];

        priorities[i] = processes[i][2];

    }

    // Sort the processes based on priority (in ascending order) using bubble sort

    for (i = 0; i < n - 1; i++) {

        for (j = 0; j < n - i - 1; j++) {

            if (priorities[j] > priorities[j + 1]) {

                // Swap burst times

                int tempBurstTime = burstTimes[j];

                burstTimes[j] = burstTimes[j + 1];

                burstTimes[j + 1] = tempBurstTime;

                // Swap priorities

                int tempPriority = priorities[j];

                priorities[j] = priorities[j + 1];

                priorities[j + 1] = tempPriority;

            }

        }

    }

    // Calculate waiting time for each process

    for (i = 1; i < n; i++) {

        waitingTimes[i] = burstTimes[i - 1] + waitingTimes[i - 1];

    }

    // Calculate turnaround time for each process

    for (i = 0; i < n; i++) {

        turnaroundTimes[i] = burstTimes[i] + waitingTimes[i];

    }

    // Calculate average waiting time and average turnaround time

    float totalWaitingTime = 0;

    float totalTurnaroundTime = 0;

    for (i = 0; i < n; i++) {

        totalWaitingTime += waitingTimes[i];

        totalTurnaroundTime += turnaroundTimes[i];

    }

    *avgWaitingTime = totalWaitingTime / n;

    *avgTurnaroundTime = totalTurnaroundTime / n;

}

int main() {

    int processes[][3] = {

        {1, 30, 2},

        {2, 5, 1},

        {3, 12, 3}

    };

    int n = sizeof(processes) / sizeof(processes[0]);

    float avgWaitingTime, avgTurnaroundTime;

    calculateAverageTimes(processes, n, &avgWaitingTime, &avgTurnaroundTime);

    printf("Average Waiting Time: %.2f\n", avgWaitingTime);

    printf("Average Turnaround Time: %.2f\n", avgTurnaroundTime);

    return 0;

}
