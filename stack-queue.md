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