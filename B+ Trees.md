## Operations on B+ Trees

```c
#include <stdio.h>
#include <stdlib.h>

#define M 2  // Define the minimum degree (number of keys) for a node (excluding the root)

// Structure for a B+ Tree node
struct bPlusTreeNode {
  int *keys;  // Array of keys
  int numKeys;  // Number of keys in the node
  struct bPlusTreeNode *children[M + 1];  // Array of child pointers (one more than keys for leaf nodes)
  int isLeaf;  // Flag to indicate if the node is a leaf
};

// Function prototypes
struct bPlusTreeNode* createNode(int isLeaf);
struct bPlusTreeNode* search(struct bPlusTreeNode *root, int key);
void insertNode(struct bPlusTreeNode *root, int key);

int main() {
  struct bPlusTreeNode *root = createNode(1); // Create a root leaf node

  // Sample insertions (replace with actual insertion calls)
  insertNode(root, 10);
  insertNode(root, 20);
  insertNode(root, 30);

  // Sample search (replace with actual search calls)
  struct bPlusTreeNode *node = search(root, 20);
  if (node != NULL) {
    printf("Key 20 found\n");
  } else {
    printf("Key 20 not found\n");
  }

  return 0;
}

// Create a new B+ Tree node
struct bPlusTreeNode* createNode(int isLeaf) {
  struct bPlusTreeNode *newNode = (struct bPlusTreeNode*)malloc(sizeof(struct bPlusTreeNode));
  newNode->keys = (int*)malloc(sizeof(int) * (2 * M - 1)); // Allocate space for maximum keys
  newNode->children = (struct bPlusTreeNode**)malloc(sizeof(struct bPlusTreeNode*) * (M + 1)); // Allocate space for maximum children (one more for leaf nodes)
  newNode->numKeys = 0;
  newNode->isLeaf = isLeaf;
  return newNode;
}

// Search for a key in the B+ Tree (recursive)
struct bPlusTreeNode* search(struct bPlusTreeNode *root, int key) {
  if (root == NULL)
    return NULL;

  // Traverse internal nodes
  if (!root->isLeaf) {
    for (int i = 0; i < root->numKeys; i++) {
      if (key < root->keys[i])
        return search(root->children[i], key);
    }
    return search(root->children[root->numKeys], key); // Search in the last child
  }

  // Search in leaf node
  for (int i = 0; i < root->numKeys; i++) {
    if (root->keys[i] == key)
      return root;
  }
  return NULL;
}

// Insert a key into the B+ Tree (recursive) - Simplified version (no splitting)
void insertNode(struct bPlusTreeNode *root, int key) {
  if (root->isLeaf) {
    // Handle insertion in a leaf node (assuming enough space for simplicity)
    int i = root->numKeys - 1;
    while (i >= 0 && key < root->keys[i]) {
      root->keys[i + 1] = root->keys[i];
      i--;
    }
    root->keys[i + 1] = key;
    root->numKeys++;
  } else {
    // Handle insertion in an internal node (find appropriate child)
    int i = 0;
    while (i < root->numKeys && key > root->keys[i])
      i++;
    insertNode(root->children[i], key);
  }
}
```
