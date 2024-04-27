##Hashing Programs

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_SIZE 10  // Define the size of the hash table

// Structure to store a key-value pair
struct Entry {
  char *key;
  int value;
};

// Structure for the hash table
struct HashTable {
  struct Entry *table[MAX_SIZE];
  int size;
};

// Function prototypes
unsigned long hash_function(const char *key);
struct HashTable* createHashTable();
void insertHashTable(struct HashTable *hashTable, const char *key, int value);
int searchHashTable(struct HashTable *hashTable, const char *key);

int main() {
  struct HashTable *hashTable = createHashTable();

  insertHashTable(hashTable, "apple", 10);
  insertHashTable(hashTable, "banana", 20);
  insertHashTable(hashTable, "cherry", 30);

  int value = searchHashTable(hashTable, "apple");
  if (value != -1) {
    printf("Value for 'apple': %d\n", value);
  } else {
    printf("Key 'apple' not found\n");
  }

  value = searchHashTable(hashTable, "mango");
  if (value != -1) {
    printf("Value for 'mango': %d\n", value);
  } else {
    printf("Key 'mango' not found\n");
  }

  return 0;
}

// Hash function (simple division method)
unsigned long hash_function(const char *key) {
  unsigned long hash_value = 0;
  const char *c;

  // Iterate through characters and add ASCII values
  for (c = key; *c != '\0'; ++c) {
    hash_value += *c;
  }

  // Apply division to get the hash index within the table size
  return hash_value % MAX_SIZE;
}

// Create a new hash table
struct HashTable* createHashTable() {
  struct HashTable *hashTable = (struct HashTable*)malloc(sizeof(struct HashTable));
  memset(hashTable->table, 0, sizeof(hashTable->table)); // Initialize all entries to NULL
  hashTable->size = 0;
  return hashTable;
}

// Insert a key-value pair into the hash table
void insertHashTable(struct HashTable *hashTable, const char *key, int value) {
  // Get the hash index for the key
  unsigned long hash_index = hash_function(key);

  // Probe for an empty slot (linear probing)
  while (hashTable->table[hash_index] != NULL) {
    hash_index++;
    hash_index %= MAX_SIZE; // Wrap around if reaching the end of the table
  }

  // Allocate memory for a new entry
  struct Entry *newEntry = (struct Entry*)malloc(sizeof(struct Entry));
  newEntry->key = strdup(key); // Duplicate the key string
  newEntry->value = value;

  // Insert the key-value pair into the table
  hashTable->table[hash_index] = newEntry;
  hashTable->size++;
}

// Search for a key in the hash table
int searchHashTable(struct HashTable *hashTable, const char *key) {
  unsigned long hash_index = hash_function(key);

  // Probe for the key (linear probing)
  while (hashTable->table[hash_index] != NULL) {
    if (strcmp(hashTable->table[hash_index]->key, key) == 0) {
      return hashTable->table[hash_index]->value; // Key found, return value
    }

    hash_index++;
    hash_index %= MAX_SIZE; // Wrap around if reaching the end of the table
  }

  // Key not found
  return -1;
}
```
