# Computer Networks Lab Manual

---

## Experiment 1: Data Link Layer Framing Methods

### **Implement data link layer framing methods such as character stuffing and bit stuffing.**

- **Character Stuffing**: Involves inserting a special character before each instance of the special character in the data stream.

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

- **Bit Stuffing**: Involves inserting a bit to break up sequences of bits that might be interpreted as control information.

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

### **Write a program to compute CRC codes for the polynomials: CRC-12, CRC-16, and CRC-CCIP.**

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

### **Develop a simple data link layer that performs flow control using the sliding window protocol and loss recovery using the Go-Back-N mechanism.**

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

### **Implement Dijkstra’s algorithm to compute the shortest path through a network.**

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

## Experiment 5: Broadcast Tree for a Subnet

### **Take an example subnet of hosts and obtain a broadcast tree for the subnet.**

- A broadcast tree can be derived using a Minimum Spanning Tree (MST) algorithm such as Prim's Algorithm.

```c
#include <stdio.h>

#define MAX_NODES 50
#define INF 99

int n;
int mincost = 0;
int edge[MAX_NODES][MAX_NODES];
int parent[MAX_NODES];
int t[MAX_NODES][2];

int find(int l);
void sunion(int l, int m);

int main() {
    int i, j, u, v, min, p, q;

    printf("\nEnter the number of nodes: ");
    scanf("%d", &n);

    for (i = 0; i < n; i++) {
        printf("%c\t", 65 + i);
        parent[i] = -1;
    }

    printf("\n");

    for (i = 0; i < n; i++) {
        printf("%c: ", 65 + i);
        for (j = 0; j < n; j++) {
            scanf("%d", &edge[i][j]);
        }
    }

    for (i = 0; i < n; i++) {
        min = INF;
        for (j = 0; j < n; j++) {
            if (edge[i][j] != INF && edge[i][j] < min) {
                min = edge[i][j];
                u = i;
                v = j;
            }
        }
        p = find(u);
        q = find(v);
        if (p != q) {
            t[i][0] = u;
            t[i][1] = v;
            mincost += edge[u][v];
            sunion(p, q);
        } else {
            t[i][0] = -1;
            t[i][1] = -1;
        }
    }

    printf("Minimum cost is %d\nMinimum spanning tree is:\n", mincost);
    for (i = 0; i < n; i++) {
        if (t[i][0] != -1 && t[i][1] != -1) {
            printf("%c %c %d\n", 65 + t[i][0], 65 + t[i][1], edge[t[i][0]][t[i][1]]);
        }
    }

    return 0;
}

void sunion(int l, int m) {
    parent[l] = m;
}

int find(int l) {
    if (parent[l] > 0) {
        l = parent[l];
    }
    return l;
}
```


---

## Experiment 6: Distance Vector Routing Algorithm

### **Implement the distance vector routing algorithm to obtain routing tables at each node.**

*Sample Code:*

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

## Experiment 7: Data Encryption and Decryption

### **Implement data encryption and decryption.**

#### Encryption and Decryption using Caesar Cipher

- **Encryption**

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

- **Decryption**

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

**"XOR Cipher Implementation for Data Encryption and Decryption"**

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void encrypt(const char *plain, const char *key, char *cipher) {
    int lp = strlen(key);
    for (int i = 0; plain[i] != '\0'; i++) {
        cipher[i] = plain[i] ^ lp; // XOR operation with key length
    }
    cipher[strlen(plain)] = '\0'; // Null-terminate the cipher
}

void decrypt(const char *cipher, int keyLength, char *plain) {
    for (int i = 0; cipher[i] != '\0'; i++) {
        plain[i] = cipher[i] ^ keyLength; // XOR operation with key length
    }
    plain[strlen(cipher)] = '\0'; // Null-terminate the plain text
}

int main() {
    char cipher[50], plain[50], key[50];

    while (1) {
        int choice;
        printf("\n----- MENU -----\n");
        printf("1: Data Encryption\n");
        printf("2: Data Decryption\n");
        printf("3: Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: {
                printf("Data Encryption\n");
                printf("Enter the plain text: ");
                scanf("%s", plain);
                printf("Enter the encryption key: ");
                scanf("%s", key);
                encrypt(plain, key, cipher);
                printf("The encrypted text is: %s\n", cipher);
                break;
            }
            case 2: {
                printf("Data Decryption\n");
                int keyLength = strlen(key);
                decrypt(cipher, keyLength, plain);
                printf("Decrypted text is: %s\n", plain);
                break;
            }
            case 3:
                exit(0);
            default:
                printf("Invalid choice. Please try again.\n");
        }
    }

    return 0;
}
```
