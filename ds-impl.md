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

# Doubly LL impl.
## Code ->
```cpp
#include <bits/stdc++.h>
using namespace std;

// Node class to represent each element in the doubly linked list
class Node {
public:
    int data;   // Data stored in the node
    Node* next; // Pointer to the next node in the list
    Node* prev; // Pointer to the previous node in the list

    // Default constructor
    Node() {
        data = -1;
        next = NULL;
        prev = NULL;
    }

    // Constructor to initialize node with a specific value
    Node(int data) {
        this->data = data;
        next = NULL;
        prev = NULL;
    }
};

// DoublyLinkedList class to manage the linked list operations
class DoublyLinkedList {
private:
    Node* head; // Pointer to the first node in the list

public:
    // Constructor to initialize the linked list
    DoublyLinkedList() {
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
        newNode->prev = temp;          // Set the previous pointer of the new node
    }

    // Method to display all the nodes in the list from head to tail
    void displayForward() {
        Node* temp = head;
        while (temp != NULL) {         // Traverse the list
            cout << temp->data << " "; // Print the data of each node
            temp = temp->next;
        }
        cout << endl;                  // Newline for cleaner output
    }

    // Method to display all the nodes in the list from tail to head
    void displayBackward() {
        Node* temp = head;
        if (temp == NULL) return;      // If the list is empty

        // Traverse to the last node
        while (temp->next != NULL) {
            temp = temp->next;
        }

        // Traverse backward from the last node
        while (temp != NULL) {
            cout << temp->data << " ";
            temp = temp->prev;
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

        // If the node to be deleted is the head
        if (pos == 1) {
            head = head->next;
            if (head != NULL) {
                head->prev = NULL;
            }
            delete temp;
            return;
        }

        for (int i = 1; temp != NULL && i < pos; ++i) {
            temp = temp->next;
        }

        // If position is more than the number of nodes
        if (temp == NULL) {
            return;
        }

        // Re-link the previous node and next node
        if (temp->prev != NULL) {
            temp->prev->next = temp->next;
        }
        if (temp->next != NULL) {
            temp->next->prev = temp->prev;
        }

        delete temp; // Delete the node
    }
};

int main() {
    DoublyLinkedList lst;
    lst.insert(5);    // Insert node with value 5
    lst.insert(6);    // Insert node with value 6
    lst.insert(8);    // Insert node with value 8

    lst.displayForward();  // Display the list forward: Output should be 5 6 8
    lst.displayBackward(); // Display the list backward: Output should be 8 6 5

    lst.deleteNode(2);     // Delete the node at position 2
    lst.displayForward();  // Display the list forward: Output should be 5 8

    lst.deleteNode(1);     // Delete the node at position 1
    lst.displayForward();  // Display the list forward: Output should be 8

    lst.deleteNode(3);     // Attempt to delete a node at an out-of-range position
    lst.displayForward();  // Display the list forward: Output should be 8

    return 0;
}
```

# Stack impl.
## Code ->
```cpp
#include <iostream>
using namespace std;

class Stack {
private:
    int* arr;
    int top;
    int capacity;

public:
    // Constructor to initialize stack
    Stack(int size) {
        arr = new int[size];
        capacity = size;
        top = -1;
    }

    // Destructor to free memory allocated for the stack
    ~Stack() {
        delete[] arr;
    }

    // Function to add an element to the stack
    void push(int x) {
        if (isFull()) {
            cout << "Overflow: Stack is full" << endl;
            return;
        }
        arr[++top] = x;
    }

    // Function to remove the top element from the stack
    int pop() {
        if (isEmpty()) {
            cout << "Underflow: Stack is empty" << endl;
            return -1;
        }
        return arr[top--];
    }

    // Function to return the top element of the stack
    int peek() {
        if (isEmpty()) {
            cout << "Stack is empty" << endl;
            return -1;
        }
        return arr[top];
    }

    // Function to check if the stack is empty
    bool isEmpty() {
        return top == -1;
    }

    // Function to check if the stack is full
    bool isFull() {
        return top == capacity - 1;
    }

    // Function to return the size of the stack
    int size() {
        return top + 1;
    }
};

int main() {
    Stack stack(5);

    stack.push(1);
    stack.push(2);
    stack.push(3);

    cout << "Top element is " << stack.peek() << endl;
    cout << "Stack size is " << stack.size() << endl;

    stack.pop();
    stack.pop();
    stack.pop();

    stack.pop(); // Trying to pop from an empty stack

    return 0;
}
```

# Stack impl using array
## Code ->
```cpp
#include <iostream>
using namespace std;

class Stack {
private:
    int* arr;
    int top;
    int capacity;

public:
    // Constructor to initialize stack
    Stack(int size) {
        arr = new int[size];
        capacity = size;
        top = -1;
    }

    // Destructor to free memory allocated for the stack
    ~Stack() {
        delete[] arr;
    }

    // Function to add an element to the stack
    void push(int x) {
        if (isFull()) {
            cout << "Overflow: Stack is full" << endl;
            return;
        }
        arr[++top] = x;
    }

    // Function to remove the top element from the stack
    int pop() {
        if (isEmpty()) {
            cout << "Underflow: Stack is empty" << endl;
            return -1;
        }
        return arr[top--];
    }

    // Function to return the top element of the stack
    int peek() {
        if (isEmpty()) {
            cout << "Stack is empty" << endl;
            return -1;
        }
        return arr[top];
    }

    // Function to check if the stack is empty
    bool isEmpty() {
        return top == -1;
    }

    // Function to check if the stack is full
    bool isFull() {
        return top == capacity - 1;
    }

    // Function to return the size of the stack
    int size() {
        return top + 1;
    }
};

int main() {
    Stack stack(5);

    stack.push(1);
    stack.push(2);
    stack.push(3);

    cout << "Top element is " << stack.peek() << endl;
    cout << "Stack size is " << stack.size() << endl;

    stack.pop();
    stack.pop();
    stack.pop();

    stack.pop(); // Trying to pop from an empty stack

    return 0;
}
```

# Stack impl using LL
## Code ->
```cpp
#include <iostream>
using namespace std;

// Node class to represent each element in the stack
class Node {
public:
    int data;
    Node* next;

    // Constructor to initialize node with a specific value
    Node(int data) {
        this->data = data;
        next = NULL;
    }
};

// Stack class to manage stack operations
class Stack {
private:
    Node* top; // Pointer to the top node of the stack

public:
    // Constructor to initialize stack
    Stack() {
        top = NULL;
    }

    // Function to add an element to the stack
    void push(int x) {
        Node* newNode = new Node(x);
        newNode->next = top;
        top = newNode;
    }

    // Function to remove the top element from the stack
    int pop() {
        if (isEmpty()) {
            cout << "Underflow: Stack is empty" << endl;
            return -1;
        }
        Node* temp = top;
        top = top->next;
        int popped = temp->data;
        delete temp;
        return popped;
    }

    // Function to return the top element of the stack
    int peek() {
        if (isEmpty()) {
            cout << "Stack is empty" << endl;
            return -1;
        }
        return top->data;
    }

    // Function to check if the stack is empty
    bool isEmpty() {
        return top == NULL;
    }

    // Function to return the size of the stack
    int size() {
        int count = 0;
        Node* temp = top;
        while (temp != NULL) {
            count++;
            temp = temp->next;
        }
        return count;
    }
};

int main() {
    Stack stack;

    stack.push(1);
    stack.push(2);
    stack.push(3);

    cout << "Top element is " << stack.peek() << endl;
    cout << "Stack size is " << stack.size() << endl;

    stack.pop();
    stack.pop();
    stack.pop();

    stack.pop(); // Trying to pop from an empty stack

    return 0;
}
```

# Queue impl using array
## Code ->
```cpp
#include <iostream>
using namespace std;

class Queue {
private:
    int* arr;
    int front;
    int rear;
    int capacity;
    int count;

public:
    // Constructor to initialize queue
    Queue(int size) {
        arr = new int[size];
        capacity = size;
        front = 0;
        rear = -1;
        count = 0;
    }

    // Destructor to free memory allocated for the queue
    ~Queue() {
        delete[] arr;
    }

    // Function to add an element to the queue
    void enqueue(int x) {
        if (isFull()) {
            cout << "Overflow: Queue is full" << endl;
            return;
        }
        rear = (rear + 1) % capacity;
        arr[rear] = x;
        count++;
    }

    // Function to remove the front element from the queue
    int dequeue() {
        if (isEmpty()) {
            cout << "Underflow: Queue is empty" << endl;
            return -1;
        }
        int x = arr[front];
        front = (front + 1) % capacity;
        count--;
        return x;
    }

    // Function to return the front element of the queue
    int peek() {
        if (isEmpty()) {
            cout << "Queue is empty" << endl;
            return -1;
        }
        return arr[front];
    }

    // Function to check if the queue is empty
    bool isEmpty() {
        return (count == 0);
    }

    // Function to check if the queue is full
    bool isFull() {
        return (count == capacity);
    }

    // Function to return the size of the queue
    int size() {
        return count;
    }
};

int main() {
    Queue q(5);

    q.enqueue(1);
    q.enqueue(2);
    q.enqueue(3);

    cout << "Front element is " << q.peek() << endl;
    cout << "Queue size is " << q.size() << endl;

    q.dequeue();
    q.dequeue();
    q.dequeue();

    q.dequeue(); // Trying to dequeue from an empty queue

    return 0;
}

```

# Binary tree impl
## Code ->

```cpp
#include <iostream>
using namespace std;

// Node class to represent each node in the binary tree
class Node {
public:
    int data;
    Node* left;
    Node* right;

    // Constructor to initialize node with specific value
    Node(int data) {
        this->data = data;
        left = right = nullptr;
    }
};

// BinaryTree class to manage binary tree operations
class BinaryTree {
private:
    Node* root;

    // Helper function for inorder traversal
    void inorderTraversal(Node* node) {
        if (node == nullptr) return;
        inorderTraversal(node->left);
        cout << node->data << " ";
        inorderTraversal(node->right);
    }

    // Helper function for preorder traversal
    void preorderTraversal(Node* node) {
        if (node == nullptr) return;
        cout << node->data << " ";
        preorderTraversal(node->left);
        preorderTraversal(node->right);
    }

    // Helper function for postorder traversal
    void postorderTraversal(Node* node) {
        if (node == nullptr) return;
        postorderTraversal(node->left);
        postorderTraversal(node->right);
        cout << node->data << " ";
    }

public:
    // Constructor to initialize binary tree with null root
    BinaryTree() {
        root = nullptr;
    }

    // Function to insert a new node in the binary tree
    void insert(int data) {
        Node* newNode = new Node(data);
        if (root == nullptr) {
            root = newNode;
            return;
        }

        Node* current = root;
        Node* parent = nullptr;

        while (current != nullptr) {
            parent = current;
            if (data < current->data) {
                current = current->left;
            } else {
                current = current->right;
            }
        }

        if (data < parent->data) {
            parent->left = newNode;
        } else {
            parent->right = newNode;
        }
    }

    // Function for inorder traversal
    void inorder() {
        inorderTraversal(root);
        cout << endl;
    }

    // Function for preorder traversal
    void preorder() {
        preorderTraversal(root);
        cout << endl;
    }

    // Function for postorder traversal
    void postorder() {
        postorderTraversal(root);
        cout << endl;
    }
};

int main() {
    BinaryTree bt;

    bt.insert(10);
    bt.insert(6);
    bt.insert(15);
    bt.insert(3);
    bt.insert(8);
    bt.insert(12);
    bt.insert(17);

    cout << "Inorder traversal: ";
    bt.inorder();

    cout << "Preorder traversal: ";
    bt.preorder();

    cout << "Postorder traversal: ";
    bt.postorder();

    return 0;
}
```