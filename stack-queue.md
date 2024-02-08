# [225. Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/description/)

## Approaches ->
1. Using 2 queues
2. Using 1 queue

## Codes ->
1. 
```cpp
class MyStack {
public:
    // using 2 queue
    // just push the new element in q2 (most recent element in front)
    // then keep pushing all elements of q1 in q2 (all past elements in back) and keep popping q1
    // that way the q2 has the most recent elements in front 
    // swap the values of q1 and q2 to update q1 and make q2 empty again
    queue<int> q1;
    queue<int> q2;
    
    MyStack() {
        
    }
    
    void push(int x) {
        // Push operation
        // Add the new element to the auxiliary queue (q2)
        q2.push(x);

        // Move all elements from the primary queue (q1) to the auxiliary queue (q2)
        // This ensures the new element is at the front (most recent) of the stack
        while(q1.size()){
            q2.push(q1.front());
            q1.pop();
        }

        // Swap q1 and q2
        swap(q1, q2);
    }
    
    // rest all the operations are simple and normal.

    int pop() {
        int res = q1.front();
        q1.pop();
        return res;
    }
    
    int top() {
        return q1.front();
    }
    
    bool empty() {
        return q1.empty();
    }
};
```
2. 
```cpp
class MyStack {
public:

    //using 1 queue
    //just push front element of queue in back
    //and then pop that element
    //then it will follow stack order.
    
    queue<int> q1;
    MyStack() {
        
    }
    
    void push(int x) {
        // Add the new element to the back of the queue
        q1.push(x);

        // Move the front elements to the back, reordering the queue to simulate stack behavior
        for(int i=0;i<q1.size()-1;i++){
            q1.push(q1.front());
            q1.pop();
        }
    }
    
    int pop() {
        if(q1.empty())return -1;
        int x = q1.front();
        q1.pop();
        return x;
        
    }
    
    int top() {
        if(q1.empty())return -1;
        return q1.front();
    }
    
    bool empty() {
        return q1.empty();
    }
};
```

# [232. Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/description/)

## Approach ->
You can dry run and understand very easily. We take two stacks s1 and s2. Move all elements from s1 to s2 to reverse the order, then push new element to s2. Now move the elements from s2 to s1 without swapping it because swapping means putting the elements as it is. But we pop from s2 and push in s1 to maintain the correct order.

## Code ->
```cpp
class MyQueue {
public:
    stack<int> s1;  // Main stack for enqueueing elements
    stack<int> s2;  // Auxiliary stack for dequeueing elements
    
    MyQueue() {
        // Constructor: Initialize the queues using two stacks
    }
    
    void push(int x) {
        // Enqueue operation
        // Move all elements from s1 to s2 to reverse the order
        while (s1.size()) {
            s2.push(s1.top());
            s1.pop();
        }
        
        // Push the new element onto s2, which will be at the bottom of the reversed order
        s2.push(x);
        
        // Move the elements back from s2 to s1 to maintain the correct order
        while (s2.size()) {
            s1.push(s2.top());
            s2.pop();
        }
    }
    
    int pop() {
        // Dequeue operation
        // Retrieve and remove the front element from s1
        int elem = s1.top();
        s1.pop();
        return elem;
    }
    
    int peek() {
        // Peek operation
        // Retrieve the front element from s1 without removing it
        return s1.top();
    }
    
    bool empty() {
        // Check if the queue is empty
        return s1.empty();
    }
};
```

# [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/description/)

## Approach-> 
Use a stack. If we have opening brackets then push it into the stack. If we have a closing bracket then check if the stack is empty. If empty return false because there will be no closing bracket if there is no opening bracket (TO AVOID RUNTIME). Then check if we have an opening bracket in st.top() and closing in s[i] then pop the top of st. else return false;

## Code ->
```cpp
class Solution {
public:
    bool isValid(string s) {
        stack <char> st;
        for(int i=0; i<s.size(); i++){
            if(s[i] == '(' || s[i] == '{' || s[i] == '[')
                st.push(s[i]);
            else
            {
                if(st.empty()) return false;
            
                if(st.top()=='(' && s[i]==')' || st.top()=='{' && s[i]=='}' || st.top()=='[' && s[i]==']')
                    st.pop();
                else
                    return false;
            }
        }
        
        if(st.empty())
            return true;
        else 
            return false;
    }
};
```

# [155. Min Stack](https://leetcode.com/problems/min-stack/description/)

## Approach -> 
ATQ you have to do every operation in O(1) TC. You can easily do push, pop, top operations in O(1) TC but the only problem is getMin() operation. In order to store the minimum element we can make a stack of pair int. The first position contains the val and the second contains the minimum val till that pertucular element. That way you can always have the minimum element and even if you pop the top, the minimum will be popped and the last minimums will be in action keeping the minimums updated.

## Code ->
```cpp
class MinStack {
public:
    stack <pair<int, int>> s;
    MinStack() {
        
    }
    
    void push(int val) {
        if(s.empty()) s.push({val, val});
        else{
            int mini = min(val, s.top().second);
            s.push({val, mini});
        }
    }
    
    void pop() {
        s.pop();
    }
    
    int top() {
        return s.top().first;
    }
    
    int getMin() {
        return s.top().second;
    }
};
```