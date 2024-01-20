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

# [328. Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/description/)

## Approaches ->
1. Brute Force: Separate odd and even index nodes into an array using two iterations. Replace the data in the linked list by traversing it again and using the array.
Time complexity: O(n), Space complexity: O(n).
2. Instead of using an external array, rearrange the linked list in-place by changing the links. Traverse the linked list using two pointers, odd and even, starting at the head and head.next. Iterate through the linked list, rearranging the links to connect odd and even nodes alternately. Connect the last odd node to the head of even nodes.
Time complexity: O(n), Space complexity: O(1)

## Code ->
```cpp
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if(head==NULL || head->next==NULL || head->next->next==NULL) return head;
        ListNode* odd = head, *even = head->next, *toPoint = head->next;

        // connecting all odd nodes and even nodes seperately
        while(odd->next && even->next){
            odd->next = odd->next->next;
            odd = odd->next;
            even->next = even->next->next;
            even = even->next;
        }

        // connecting the end of odd linked list with start of even linked list
        odd->next = toPoint;
        return head;
    }
};
```
# [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)
## Approaches -> 
1. We can traverse through the Linked List while maintaining a count of nodes, let’s say in the variable count, and then traversing for the 2nd time for (n – count) nodes to get to the nth node of the list. TC-> O(2n)
2. Unlike the above approach, we don’t have to maintain the count value, we can find the nth node just by one traversal by using two pointer approach. So what we will do is take a fast pointer and slow pointer. Then start traversing until the fast pointer reaches the nth node. Now start traversing by one step for both the pointers fast and slow until the fast pointers reach the end. Now your slow pointer will for sure be at the n-1th position from the last. TC-> O(n).
In this approach keep in mind the two things -> first that write the code in such a way that slow pointer is at n-1th position and not nth position because you need to delete nth position. And second that keep in mind the edge case of what if the value of n is equal to the size of the linked list, in that case you have to delete the head node but you don't know if you have to delete the head node or some other node because you are only traversing once. To tackle this see in code how I make a start node and pointed it to the head node and started the slow and fast pointer from there. Dry run for the edge case and if possible please solve this question instead of looking at the approach and moving forward to the next question while revising.

## Code ->
```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode * start = new ListNode();
        start -> next = head;
        ListNode* fast = start;
        ListNode* slow = start;     

        for(int i = 1; i <= n; ++i)
            fast = fast->next;
    
        while(fast->next != NULL)
        {
            fast = fast->next;
            slow = slow->next;
        }
        
        slow->next = slow->next->next;
        
        return start->next;
    }
};
```

# [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/description/)
## Approach -> 
Keep track of the carry using a variable and simulate digits-by-digits sum starting from the head of the list, which contains the least significant digit.

---
## Code ->
```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        // Initialize variables to keep track of carry, sum, and the value to add to the new node
        int carry = 0, sum = 0, toAdd = 0;

        // Create a dummy node to serve as the head of the result linked list
        ListNode* head = new ListNode(-1);
        
        // Create a pointer to traverse the result linked list
        ListNode* mover = head;

        // Iterate through the input linked lists until both are exhausted
        while (l1 || l2) {
            // Reset sum for each iteration
            sum = 0;

            // Add values from the current nodes of l1 and l2, if available
            if (l1) sum += l1->val;
            if (l2) sum += l2->val;

            // Add the carry from the previous iteration
            sum += carry;

            // Calculate the value to be added to the new node and update the carry
            toAdd = sum % 10;
            carry = sum / 10;

            // Create a new node with the calculated value and append it to the result linked list
            ListNode* temp = new ListNode(toAdd);
            mover->next = temp;
            mover = mover->next;

            // Move to the next nodes in the input linked lists, if available
            if (l1) l1 = l1->next;
            if (l2) l2 = l2->next;
        }

        // If there's a carry remaining after the last iteration, create a new node for it
        if (carry) {
            ListNode* temp = new ListNode(carry);
            mover->next = temp;
            mover = mover->next;
        }

        // Set the next pointer of the last node to NULL to terminate the result linked list
        mover->next = nullptr;

        // Return the actual head of the result linked list (excluding the dummy node)
        return head->next;
    }
};
```

# [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/description/)

## Approaches ->
1. Brute-Force: Keep any one of the list to check its node present in the other list. Here, we are choosing the second list for this task.
Iterate through the other list. Here, it is the first one. 
Check if the both nodes are the same. If yes, we got our first intersection node.
If not, continue iteration.
If we did not find an intersection node and completed the entire iteration of the second list, then there is no intersection between the provided lists. Hence, return null.
2. Hashing
3. Better Approach: Find the length of both lists.
Find the positive difference between these lengths.
Move the dummy pointer of the larger list by the difference achieved. This makes our search length reduced to a smaller list length.
Move both pointers, each pointing two lists, ahead simultaneously till they collide.
4. Best Approach: The difference of length method requires various steps to work on it. Using the same concept of difference of length, a different approach can be implemented. The process is as follows:-

Take two dummy nodes for each list. Point each to the head of the lists.
Iterate over them. If anyone becomes null, point them to the head of the opposite lists and continue iterating until they collide.

## Code ->
```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *p1 = headA;
        ListNode *p2 = headB;

        while(p1 || p2){
            if(p1==NULL) p1 = headB;
            if(p2==NULL) p2 = headA;
            if(p1==p2) return p1;
            // make sure to check those conditions at first else the sol. would be wrong.
            p1 = p1->next;
            p2 = p2->next;
        }
        return p1;
    }
};
```