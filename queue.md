## Double Ended Queue Operation
```c
#include <stdio.h>
#include <stdlib.h>

// Structure of a node
struct Node {
    int data;
    struct Node* next;
    struct Node* prev;
};

// Structure of a deque
struct Deque {
    struct Node* front;
    struct Node* rear;
};

// Function to create a new node
struct Node* createNode(int data) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    if (newNode == NULL) {
        printf("Memory allocation failed\n");
        exit(1);
    }
    newNode->data = data;
    newNode->next = NULL;
    newNode->prev = NULL;
    return newNode;
}

// Function to initialize a deque
struct Deque* initializeDeque() {
    struct Deque* deque = (struct Deque*)malloc(sizeof(struct Deque));
    if (deque == NULL) {
        printf("Memory allocation failed\n");
        exit(1);
    }
    deque->front = NULL;
    deque->rear = NULL;
    return deque;
}

// Function to insert an element at the front of the deque
void insertFront(struct Deque* deque, int data) {
    struct Node* newNode = createNode(data);
    if (deque->front == NULL) {
        deque->front = newNode;
        deque->rear = newNode;
    } else {
        newNode->next = deque->front;
        deque->front->prev = newNode;
        deque->front = newNode;
    }
}

// Function to insert an element at the rear of the deque
void insertRear(struct Deque* deque, int data) {
    struct Node* newNode = createNode(data);
    if (deque->rear == NULL) {
        deque->front = newNode;
        deque->rear = newNode;
    } else {
        newNode->prev = deque->rear;
        deque->rear->next = newNode;
        deque->rear = newNode;
    }
}

// Function to delete an element from the front of the deque
void deleteFront(struct Deque* deque) {
    if (deque->front == NULL) {
        printf("Deque is empty\n");
        return;
    }
    struct Node* temp = deque->front;
    deque->front = deque->front->next;
    if (deque->front == NULL) {
        deque->rear = NULL;
    } else {
        deque->front->prev = NULL;
    }
    free(temp);
}

// Function to delete an element from the rear of the deque
void deleteRear(struct Deque* deque) {
    if (deque->rear == NULL) {
        printf("Deque is empty\n");
        return;
    }
    struct Node* temp = deque->rear;
    deque->rear = deque->rear->prev;
    if (deque->rear == NULL) {
        deque->front = NULL;
    } else {
        deque->rear->next = NULL;
    }
    free(temp);
}

// Function to display the elements of the deque
void displayDeque(struct Deque* deque) {
    if (deque->front == NULL) {
        printf("Deque is empty\n");
        return;
    }
    struct Node* current = deque->front;
    while (current != NULL) {
        printf("%d ", current->data);
        current = current->next;
    }
    printf("\n");
}

// Main function
int main() {
    struct Deque* deque = initializeDeque();

    // Inserting elements at front and rear
    insertFront(deque, 10);
    insertFront(deque, 20);
    insertRear(deque, 30);
    insertRear(deque, 40);

    // Displaying the deque
    printf("Deque: ");
    displayDeque(deque);

    // Deleting elements from front and rear
    deleteFront(deque);
    deleteRear(deque);

    // Displaying the deque after deletion
    printf("Deque after deletion: ");
    displayDeque(deque);

    // Freeing allocated memory
    free(deque);

    return 0;
}
```
