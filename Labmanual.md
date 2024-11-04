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

## Experiment 5: Obtain a Broadcast Tree for a Subnet

A broadcast tree is a spanning tree that connects all nodes in a network, ensuring that broadcast messages can be transmitted efficiently to all nodes. One common approach is to use a Minimum Spanning Tree (MST) algorithm like Prim's or Kruskal's to derive this tree.

#### Example using Prim’s Algorithm

```c
#include <stdio.h>
#include <limits.h>

#define V 5  // Number of vertices in the graph

int minKey(int key[], int mstSet[]) {
    int min = INT_MAX, min_index;
    for (int v = 0; v < V; v++)
        if (mstSet[v] == 0 && key[v] < min)
            min = key[v], min_index = v;
    return min_index;
}

void printMST(int parent[], int n, int graph[V][V]) {
    printf("Edge \tWeight\n");
    for (int i = 1; i < V; i++)
        printf("%d - %d \t%d \n", parent[i], i, graph[i][parent[i]]);
}

void primMST(int graph[V][V]) {
    int parent[V];
    int key[V];
    int mstSet[V];

    for (int i = 0; i < V; i++)
        key[i] = INT_MAX, mstSet[i] = 0;

    key[0] = 0;
    parent[0] = -1;

    for (int count = 0; count < V - 1; count++) {
        int u = minKey(key, mstSet);
        mstSet[u] = 1;

        for (int v = 0; v < V; v++)
            if (graph[u][v] && mstSet[v] == 0 && graph[u][v] < key[v])
                parent[v] = u, key[v] = graph[u][v];
    }

    printMST(parent, V, graph);
}

int main() {
    int graph[V][V] = {{0, 2, 0, 6, 0},
                       {2, 0, 3, 8, 5},
                       {0, 3, 0, 0, 7},
                       {6, 8, 0, 0, 9},
                       {0, 5, 7, 9, 0}};

    primMST(graph);

    return 0;
}
```

### Experiment 6. Implement the Distance Vector Routing Algorithm

The distance vector routing algorithm involves each node maintaining a table of the minimum distance to every other node and updating it based on information from its neighbors.

#### Example of Distance Vector Routing

```c
#include <stdio.h>

#define INFINITY 999
#define MAX_NODES 10

void distanceVector(int cost[MAX_NODES][MAX_NODES], int n) {
    int distance[MAX_NODES][MAX_NODES], via[MAX_NODES][MAX_NODES];
    int i, j, k;

    for (i = 0; i < n; i++)
        for (j = 0; j < n; j++) {
            distance[i][j] = cost[i][j];
            via[i][j] = j;
        }

    for (k = 0; k < n; k++)
        for (i = 0; i < n; i++)
            for (j = 0; j < n; j++)
                if (distance[i][k] + distance[k][j] < distance[i][j]) {
                    distance[i][j] = distance[i][k] + distance[k][j];
                    via[i][j] = k;
                }

    for (i = 0; i < n; i++) {
        printf("\nRouting table for node %d:\n", i);
        printf("To\tVia\tCost\n");
        for (j = 0; j < n; j++) {
            if (i != j)
                printf("%d\t%d\t%d\n", j, via[i][j], distance[i][j]);
        }
    }
}

int main() {
    int cost[MAX_NODES][MAX_NODES], n;
    printf("Enter the number of nodes: ");
    scanf("%d", &n);

    printf("Enter the cost matrix (use 999 for infinity):\n");
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            scanf("%d", &cost[i][j]);

    distanceVector(cost, n);

    return 0;
}
```


---

## Experiment 6: Data Encryption and Decryption

### Encryption and Decryption Technique Caesar Cipher Example
#### Encryption

```c
#include <stdio.h>
#include <string.h>

// Function to encrypt the plaintext using a basic Caesar Cipher
void encrypt(char *plaintext, int shift) {
    char ciphertext[100];
    strcpy(ciphertext, plaintext);

    for (int i = 0; i < strlen(ciphertext); i++) {
        if (ciphertext[i] >= 'a' && ciphertext[i] <= 'z') {
            ciphertext[i] = ((ciphertext[i] - 'a' + shift) % 26) + 'a';
        } else if (ciphertext[i] >= 'A' && ciphertext[i] <= 'Z') {
            ciphertext[i] = ((ciphertext[i] - 'A' + shift) % 26) + 'A';
        }
    }

    printf("Encrypted Text: %s\n", ciphertext);
}

int main() {
    char plaintext[100];
    int shift;

    printf("Enter the plaintext: ");
    fgets(plaintext, sizeof(plaintext), stdin);
    plaintext[strcspn(plaintext, "\n")] = 0; // Remove newline character

    printf("Enter the shift value: ");
    scanf("%d", &shift);

    encrypt(plaintext, shift);

    return 0;
}
```

#### Decryption

```c
#include <stdio.h>
#include <string.h>

// Function to decrypt the ciphertext using a basic Caesar Cipher
void decrypt(char *ciphertext, int shift) {
    char plaintext[100];
    strcpy(plaintext, ciphertext);

    for (int i = 0; i < strlen(plaintext); i++) {
        if (plaintext[i] >= 'a' && plaintext[i] <= 'z') {
            plaintext[i] = ((plaintext[i] - 'a' - shift + 26) % 26) + 'a';
        } else if (plaintext[i] >= 'A' && plaintext[i] <= 'Z') {
            plaintext[i] = ((plaintext[i] - 'A' - shift + 26) % 26) + 'A';
        }
    }

    printf("Decrypted Text: %s\n", plaintext);
}

int main() {
    char ciphertext[100];
    int shift;

    printf("Enter the ciphertext: ");
    fgets(ciphertext, sizeof(ciphertext), stdin);
    ciphertext[strcspn(ciphertext, "\n")] = 0; // Remove newline character

    printf("Enter the shift value: ");
    scanf("%d", &shift);

    decrypt(ciphertext, shift);

    return 0;
}
```
---
