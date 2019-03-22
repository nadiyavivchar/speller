// Implements a dictionary's functionality

#include <ctype.h>
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#include "dictionary.h"

// Represents number of buckets in a hash table
#define N 26

// Represents a node in a hash table
typedef struct node
{
    char word[LENGTH + 1];
    struct node *next;
}
node;

node *hashtable[NUM_BUCKETS];

//defining some local variables
int wordcount = 0;
node *head = NULL;
int hashtable_size;


// returning the index of the hash function as per Doug's Walkthrough
unsigned int hash_function(const char *str)
{
    int index = 0;

    // go through the whole word of the word to be hashed
    for (int i = 0; str[i] != '\0'; i++)
    {
        //add up the index per char of the word
        index += toupper(str[i]);
    }

    // modulo the index with the hashtable_size to ensure accuracy
    // and limit the index within the size
    return index % NUM_BUCKETS;
}

// Hashes word to a number between 0 and 25, inclusive, based on its first letter -> turn letter into lowercase
unsigned int hash(const char *word)
{
    return tolower(word[0]) - 'a';
}

// Loads dictionary into memory, returning true if successful else false
bool load(const char *dictionary)
{
    // Initialize hash table
    for (int i = 0; i < N; i++)
    {
        hashtable[i] = NULL;
    }

    // Open dictionary
    FILE *file = fopen(dictionary, "r");
    if (file == NULL)
    {
        unload();
        return false;
    }

    char word[LENGTH + 1];

    // Insert words into hash table (look and scan for words in dictionary until the end of file)
    while (fscanf(file, "%s", word) != EOF)
    {
        node *new_node = malloc(sizeof(node));

        // initialize the memories allocated to NULL while it wasn't used
        memset(new_node, 0, sizeof(node));

        if (new_node == NULL)
        {
            unload();
            return false;
        }

        //increase the wordcount if the word was located
        wordcount++;

        // copy the word found to word pointer of the node
        strcpy(new_node->word, word);

        // hash the word copied so it can be indexed on the hash table
        int hashed = hash(word);

        //check if it's in the list
        if (hashtable[hashed] == NULL)
        {
            hashtable[hashed] = new_node; //if not - set a new node
            head = new_node; //say the new node is head now  (add it to the list of arrays)
        }
        else
        {
            new_node->next = hashtable[hashed]; //if it's empty, rewrite it and add the content
            hashtable[hashed] = new_node;
            head = new_node;
        }

        //github thank you! https://github.com/atclarkson/CS50/blob/master/pset5/speller/dictionary.c
        if (feof(file))
        {
            free(new_node);
        }
    }

    // Close dictionary
    fclose(file);
    // Indicate success
    return true;
}

//Returns number of words in dictionary if loaded else 0 if not yet loaded
unsigned int size(void)
{
    return wordcount;
}

// Returns true if word is in dictionary else false
bool check(const char *word)
{
    // got from https://github.com/atclarkson/CS50/blob/master/pset5/speller/dictionary.c
    //create copy of word
    int length = 0;
    for (int i = 0; word[i] != '\0'; i++)
    {
        length++;
    }
    char word_copy[length + 1];

    // convert word to lowercase
    for (int i = 0; i < length; i++)
    {
        word_copy[i] = tolower(word[i]);
    }

    // add null terminator to end of char array
    word_copy[length] = '\0';

    // Hash the word
    int hashed = hash(word_copy);

    // assign pointer node to the first node of the bucket
    node *pointer = hashtable[hashed];

    // check until the end of the linked list
    while (pointer != NULL)
    {
        if (strcmp(pointer->word, word_copy) == 0)
        {
            // word is in dictionary
            return true;

        }
        else
        {
            // check next node
            pointer = pointer->next;
        }
    }
    return false;
}


// Unloads dictionary from memory, returning true if successful else false
bool unload(void)
{
    for (int i = 0; i < hashtable_size; i++)
    {
        if (hashtable[i] != NULL)
        {
            node *cursor = hashtable[i];
            while (cursor != NULL)
            {
                node *temp = cursor;
                cursor = cursor->next;
                free(temp);

            }
        }
    }
    return true;
}

