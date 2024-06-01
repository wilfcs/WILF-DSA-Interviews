# Singly LinkedList Impl.
## Code ->
```cpp
#include <bits/stdc++.h>
using namespace std;

// Node class to represent each element in the linked list
class Node {
public:
    int data;   // Data stored in the node
    Node* next; // Pointer to the next node in the list

    // Default constructor
    Node() {
        data = -1;
        next = NULL;
    }

    // Constructor to initialize node with a specific value
    Node(int data) {
        this->data = data;
        next = NULL;
    }
};

// LinkedList class to manage the linked list operations
class LinkedList {
private:
    Node* head; // Pointer to the first node in the list

public:
    // Constructor to initialize the linked list
    LinkedList() {
        head = NULL;
    }

    // Method to insert a new node at the end of the list
    void insert(int val) {
        Node* newNode = new Node(val); // Create a new node with the given value
        if (head == NULL) {            // If the list is empty
            head = newNode;            // Set the new node as the head
            return;
        }

        Node* temp = head;
        while (temp->next != NULL) {   // Traverse to the end of the list
            temp = temp->next;
        }

        temp->next = newNode;          // Append the new node at the end
    }

    // Method to display all the nodes in the list
    void display() {
        Node* temp = head;
        while (temp != NULL) {         // Traverse the list
            cout << temp->data << " "; // Print the data of each node
            temp = temp->next;
        }
        cout << endl;                  // Newline for cleaner output
    }

    // Method to delete a node at a specific position
    void deleteNode(int pos) {
        if (head == NULL || pos <= 0) {
            // Invalid position or empty list
            return;
        }

        Node* temp = head;

        if (pos == 1) {                // If the position is the head
            head = head->next;         // Move the head to the next node
            delete temp;               // Delete the old head
            return;
        }

        for (int i = 1; temp != NULL && i < pos - 1; ++i) {
            temp = temp->next;         // Traverse to the node before the position
        }

        // If position is more than the number of nodes
        if (temp == NULL || temp->next == NULL) {
            return;
        }

        Node* toDel = temp->next;      // Node to be deleted
        temp->next = temp->next->next; // Bypass the node to be deleted
        delete toDel;                  // Delete the node
    }
};

int main() {
    LinkedList lst;
    lst.insert(5);    // Insert node with value 5
    lst.insert(6);    // Insert node with value 6
    lst.insert(8);    // Insert node with value 8

    lst.display();    // Display the list: Output should be 5 6 8

    lst.deleteNode(2); // Delete the node at position 2
    lst.display();     // Display the list: Output should be 5 8

    lst.deleteNode(1); // Delete the node at position 1
    lst.display();     // Display the list: Output should be 8

    lst.deleteNode(3); // Attempt to delete a node at an out-of-range position
    lst.display();     // Display the list: Output should be 8

    return 0;
}
```