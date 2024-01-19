[876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/description/)
---
Approaches ->
1. Find the size of the ll and return the size/2 node.
2. Fast-Slow pointer
---
Code ->
```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        //ListNode *temp = head;
//         int size=0;
//         while(temp){
//             temp = temp->next;
//             size++;
//         }
        
//         size = size/2 + 1;
//         cout << size;
        
//         temp = head;
//         while(--size)
//             temp = temp->next;
//         return temp;
        
        ListNode *slow=head, *fast=head;
        while(fast && fast->next ){
            slow=slow->next;
            fast=fast->next->next;
        }
        return slow;
    }      
};
```
[206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/description/)
## Iterative Approach Code (TC->O(N) SC->O(1)) ->
```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* p = NULL, *c = head, *n = NULL;
        while(c){
            n = c->next; // don't forget to assign the value of next here instead at the end to avoid runtime error
            c->next = p;
            p=c;
            c=n;
        }
        return p;
    }
};
```
## [Recursive Approach Code (TC->O(N) SC->O(1)) ->](https://takeuforward.org/data-structure/reverse-a-linked-list/)
```cpp
Node* reverseLinkedList(Node* head) {
    // Base case:
    // If the linked list is empty or has only one node,
    // return the head as it is already reversed.
    if (head == NULL || head->next == NULL) {
        return head;
    }
    
    // Recursive step:
    // Reverse the linked list starting 
    // from the second node (head->next).
    Node* newHead = reverseLinkedList(head->next);
    
    // Save a reference to the node following
    // the current 'head' node.
    Node* front = head->next;
    
    // Make the 'front' node point to the current
    // 'head' node in the reversed order.
    front->next = head;
    
    // Break the link from the current 'head' node
    // to the 'front' node to avoid cycles.
    head->next = NULL;
    
    // Return the 'newHead,' which is the new
    // head of the reversed linked list.
    return newHead;
}
```
# [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/description/)
## Approaches ->
1. Maintain a hashmap. Traverse the ll and find the node in the map. Insert the node in the map.
2. Take a slow and fast pointer. If cycle exist, fast and slow pointer will meet each other at some point.
---
## Code ->
2. 
```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode *fast = head, *slow = head;
        while(fast && fast->next)
        {
            fast = fast->next->next;
            slow = slow->next;
            if(fast == slow)
                return true;
        }
        return false;
    }
};
```

# [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/description/)

## Approaches ->
1. We can store nodes in a hash table so that, if a loop exists, the head will encounter the same node again.
2. Two pointer approach ->
- Initially take two pointers, fast and slow. The fast pointer takes two steps ahead while the slow pointer will take a single step ahead for each iteration.
- We know that if a cycle exists, fast and slow pointers will collide.
- If the cycle does not exist, the fast pointer will move to NULL
- Else, when both slow and fast pointer collides, it detects a cycle exists.
- Take another pointer, say entry. Point to the very first of the linked list.
- Move the slow and the entry pointer ahead by single steps until they collide. 
- Once they collide we get the starting node of the linked list.

Why does this work though?
 In the presence of a cycle, these pointers will eventually meet within the cycle. When they meet, the distance covered by the fast pointer is a multiple of the cycle length. To find the starting point of the cycle, another pointer is initiated at the beginning of the linked list. By moving this new pointer and the slow pointer one step at a time, they meet at the starting point of the cycle, effectively nullifying the extra distance covered by the fast pointer within the cycle. This method leverages the relative speeds of the pointers and ensures that the meeting point of the slow pointer and the entry pointer is indeed the starting point of the cycle.

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *slow = head;
        ListNode *fast = head;
        
        
        if(head==NULL || head->next==NULL) return NULL;
        while(fast!=NULL && fast->next!=NULL){
            slow = slow->next;
            fast = fast->next->next;
            
            if(slow == fast){
                break;
            }
                
        }
        
        if(slow!=fast) return NULL; // important condition just incase the loop doesn't exist
        
        fast = head;
        while(fast!=slow){
            slow=slow->next;
            fast=fast->next;
        }
        
        return fast;
    }
};
```

# [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/description/)

## Approaches ->
1. Use extra space and create an array of ll element and compare in the array.
2. Split the linkedlist into two linked lists from the middle and compare the two linkedlists. (Don't forget to reverse the second linkedlist)

## Code ->
```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        // Finding the middle of the linked list
        ListNode *slow = head, *fast = head, *temp = head;
        while(fast && fast->next){
            slow = slow->next;
            fast = fast->next->next;
        }

        // reverse the linked list from slow pointer
        ListNode *p = NULL, *c = slow, *n = NULL;
        while(c){
            n = c->next;
            c->next = p;
            p = c;
            c = n;
        }

        // check both linked lists
        while(p && temp){
            if(p->val!=temp->val) return false;
            p = p->next;
            temp = temp->next;
        }
        return true;
    }
};
```