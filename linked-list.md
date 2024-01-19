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
