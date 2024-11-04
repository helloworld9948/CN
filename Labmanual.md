# Computer Networks Lab Manual

---

## Experiment 1: Data Link Layer Framing

### Character Stuffing and Bit Stuffing

*Description of the experiment:*

- **Character Stuffing**: This method involves inserting a special character (usually an escape character) before each instance of the special character in the data stream.


*Sample Code:*


```c
#include <stdio.h>
#include <string.h>

void characterStuffing(char* input, char* stuffed) {
    char flag = 'F';  // Example flag character
    char esc = 'E';   // Example escape character
    int j = 0;
    
    stuffed[j++] = flag;  // Start with the flag character
    
    for (int i = 0; i < strlen(input); i++) {
        if (input[i] == flag || input[i] == esc) {
            stuffed[j++] = esc;  // Add escape character before any flag or escape character
        }
        stuffed[j++] = input[i];
    }
    
    stuffed[j++] = flag;  // End with the flag character
    stuffed[j] = '\0';    // Null-terminate the string
}

int main() {
    char input[] = "ABCDEF";
    char stuffed[50];
    
    characterStuffing(input, stuffed);
    
    printf("Original Data: %s\n", input);
    printf("Stuffed Data: %s\n", stuffed);
    
    return 0;
}
```
- **Bit Stuffing**: This involves inserting a bit into the data stream to break up sequences of bits that might be interpreted as control information.

*Sample Code:*



```c
#include <stdio.h>
#include <string.h>

void bitStuffing(char* input, char* stuffed) {
    int count = 0, j = 0;
    
    for (int i = 0; i < strlen(input); i++) {
        if (input[i] == '1') {
            count++;
        } else {
            count = 0;
        }
        
        stuffed[j++] = input[i];
        
        if (count == 5) {  // After five consecutive 1s, insert a 0
            stuffed[j++] = '0';
            count = 0;
        }
    }
    
    stuffed[j] = '\0';  // Null-terminate the string
}

int main() {
    char input[] = "1111101111101111110";
    char stuffed[50];
    
    bitStuffing(input, stuffed);
    
    printf("Original Data: %s\n", input);
    printf("Stuffed Data: %s\n", stuffed);
    
    return 0;
}
```

---

## Experiment 2: Cyclic Redundancy Check (CRC)

### CRC Code Computation

*Program to compute CRC codes for the following polynomials:*

- CRC-12
- CRC-16
- CRC-CCIP

*Sample Code:*

```c
#include <stdio.h>

// Function prototypes and sample code to compute CRC...

int main() {
    // Sample implementation and test cases
    return 0;
}
```

---

## Experiment 3: Sliding Window Protocol

### Flow Control and Loss Recovery

*Program implementing sliding window protocol for flow control and Go-Back-N mechanism for loss recovery.*

*Sample Code:*

```c
#include <stdio.h>

int main() {
    int windowSize, numFrames, frames[50];
    printf("Enter window size: ");
    scanf("%d", &windowSize);
    printf("\nEnter number of frames to transmit: ");
    scanf("%d", &numFrames);
    printf("\nEnter %d frames: ", numFrames);

    for(int i = 1; i <= numFrames; i++)
        scanf("%d", &frames[i]);

    printf("\nWith sliding window protocol, frames will be sent as follows:\n\n");

    for(int i = 1; i <= numFrames; i++) {
        if(i % windowSize == 0) {
            printf("%d\n", frames[i]);
            printf("Acknowledgement received for above frames\n\n");
        } else {
            printf("%d ", frames[i]);
        }
    }

    if(numFrames % windowSize != 0)
        printf("\nAcknowledgement received for above frames\n");

    return 0;
}
```

---

## Experiment 4: Dijkstra's Algorithm

### Shortest Path Computation

*Program to implement Dijkstra’s algorithm for finding the shortest path in a network.*

*Refactored Code:*

```c
#include <stdio.h>
#define INFINITY 9999
#define MAX 10

void dijkstra(int G[MAX][MAX], int n, int startnode);

int main() {
    int G[MAX][MAX], n, startnode;
    printf("Enter number of vertices: ");
    scanf("%d", &n);
    printf("\nEnter the adjacency matrix:\n");

    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            scanf("%d", &G[i][j]);
        }
    }

    printf("\nEnter the starting node: ");
    scanf("%d", &startnode);
    dijkstra(G, n, startnode);

    return 0;
}

void dijkstra(int G[MAX][MAX], int n, int startnode) {
    int cost[MAX][MAX], distance[MAX], pred[MAX], visited[MAX];
    int count, mindistance, nextnode;

    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            if(G[i][j] == 0) {
                cost[i][j] = INFINITY;
            } else {
                cost[i][j] = G[i][j];
            }
        }
    }

    for(int i = 0; i < n; i++) {
        distance[i] = cost[startnode][i];
        pred[i] = startnode;
        visited[i] = 0;
    }

    distance[startnode] = 0;
    visited[startnode] = 1;
    count = 1;

    while(count < n - 1) {
        mindistance = INFINITY;

        for(int i = 0; i < n; i++) {
            if(distance[i] < mindistance && !visited[i]) {
                mindistance = distance[i];
                nextnode = i;
            }
        }

        visited[nextnode] = 1;

        for(int i = 0; i < n; i++) {
            if(!visited[i]) {
                if(mindistance + cost[nextnode][i] < distance[i]) {
                    distance[i] = mindistance + cost[nextnode][i];
                    pred[i] = nextnode;
                }
            }
        }
        count++;
    }

    for(int i = 0; i < n; i++) {
        if(i != startnode) {
            printf("\nDistance to node %d = %d", i, distance[i]);
            printf("\nPath = %d", i);

            int j = i;
            do {
                j = pred[j];
                printf(" <- %d", j);
            } while(j != startnode);
        }
    }
}
```

---

## Experiment 5: Distance Vector Routing

### Routing Tables Calculation

*Description and implementation of the distance vector routing algorithm to obtain routing tables at each node.*

---

## Experiment 6: Data Encryption and Decryption

### Encryption and Decryption Techniques

*Explanation of basic encryption and decryption methods with sample code.*

---
