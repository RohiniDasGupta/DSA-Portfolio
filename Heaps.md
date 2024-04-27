## Heaps Operations

```c
#include <stdio.h>

#define MAX_SIZE 10  // Define the maximum size of the heap

// Function prototypes
void swap(int *a, int *b);
void minHeapify(int arr[], int n, int i);
void insertHeap(int arr[], int *heapSize, int newValue);
void printHeap(int arr[], int n);

int main() {
  int arr[MAX_SIZE];
  int heapSize = 0;

  insertHeap(arr, &heapSize, 10);
  insertHeap(arr, &heapSize, 20);
  insertHeap(arr, &heapSize, 5);
  insertHeap(arr, &heapSize, 30);

  printf("Heap elements: ");
  printHeap(arr, heapSize);

  return 0;
}

// Swap two integer values
void swap(int *a, int *b) {
  int temp = *a;
  *a = *b;
  *b = temp;
}

// Heapify operation (maintains min-heap property)
void minHeapify(int arr[], int n, int i) {
  // Find left and right child indices
  int left = leftChild(i);
  int right = rightChild(i);
  int smallest = i;

  // Find the smallest element among root, left child, and right child
  if (left < n && arr[left] < arr[smallest])
    smallest = left;
  if (right < n && arr[right] < arr[smallest])
    smallest = right;

  // If the root is not the smallest, swap it with the smallest child and recursively heapify down
  if (smallest != i) {
    swap(&arr[i], &arr[smallest]);
    minHeapify(arr, n, smallest);
  }
}

// Function to get the left child index (assuming 0-based indexing)
int leftChild(int i) {
  return 2 * i + 1;
}

// Function to get the right child index (assuming 0-based indexing)
int rightChild(int i) {
  return 2 * i + 2;
}

// Insert a new element into the heap (maintains min-heap property)
void insertHeap(int arr[], int *heapSize, int newValue) {
  if (*heapSize == MAX_SIZE) {
    printf("Heap overflow\n");
    return;
  }

  arr[*heapSize] = newValue;
  (*heapSize)++;

  // Heapify bottom-up to maintain min-heap property
  int i = *heapSize - 1;
  while (i > 0 && arr[parent(i)] > arr[i]) {
    swap(&arr[i], &arr[parent(i)]);
    i = parent(i);
  }
}

// Function to get the parent index (assuming 0-based indexing)
int parent(int i) {
  return (i - 1) / 2;
}

// Print the elements of the heap
void printHeap(int arr[], int n) {
  for (int i = 0; i < n; ++i) {
    printf("%d ", arr[i]);
  }
  printf("\n");
}
```
