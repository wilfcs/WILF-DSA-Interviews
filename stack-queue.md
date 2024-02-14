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

# [496. Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/description/)

## [Approach](https://takeuforward.org/data-structure/next-greater-element-using-stack/)

## Code ->
```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        // Map to store the next greater element for each element in nums2
        map<int, int> mp;
        
        // Stack to keep track of elements in nums2
        stack<int> st;
        
        // Iterate through nums2 in reverse order
        for(int i = nums2.size() - 1; i >= 0; i--){
            // If the stack is empty, set the next greater element to -1
            if(st.empty()){
                mp[nums2[i]] = -1;
            }
            // If the current element is smaller than the top of the stack
            else if(st.top() > nums2[i]){
                // Set the next greater element to the top of the stack
                mp[nums2[i]] = st.top();
            }
            // If the current element is greater or equal to the top of the stack
            else{
                // Pop elements from the stack until a greater element is found or the stack is empty
                while(!st.empty()){
                    if(st.top() < nums2[i]) st.pop();
                    else break;
                }
                // If the stack is empty, set the next greater element to -1, else set it to the top of the stack
                if(st.empty()) mp[nums2[i]] = -1;
                else mp[nums2[i]] = st.top();
            }
            // Push the current element onto the stack
            st.push(nums2[i]);
        }

        // Vector to store the results for nums1
        vector<int> ans;
        
        // Iterate through nums1 and push the corresponding next greater element from the map
        for(int i = 0; i < nums1.size(); i++){
            ans.push_back(mp[nums1[i]]);
        }
        
        return ans;
    }
};
```

# [503. Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/description/)

## Approach ->
This question is very similar to the previous question but the only difference is that we are given a circular array. The code takes advantage of the circular nature of the array by initially pushing all elements onto the stack.
This ensures that when the second iteration starts, the stack already contains all possible candidates for the next greater element.
The rest of the logic remains the same as in the previous explanation, with comparisons and stack operations considering the circular nature of the array.
The final result vector contains the next greater element for each element in the circular array.

## Code ->
```cpp
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        vector<int> ans;  // Result vector to store the next greater elements
        stack<int> st;    // Stack to keep track of elements

        // Iterate through the array and push all elements onto the stack at the beginning
        for (int i = nums.size() - 1; i >= 0; i--)
            st.push(nums[i]);

        // Iterate through the array again, considering its circular nature
        for (int i = nums.size() - 1; i >= 0; i--) {
            if (st.empty()) {
                // If the stack is empty, push -1 to indicate no greater element
                ans.push_back(-1);
            } else if (st.top() > nums[i]) {
                // If the top of the stack is greater than the current element, push it onto the result vector
                ans.push_back(st.top());
            } else {
                // Pop elements from the stack until a greater element is found or the stack is empty
                while (!st.empty()) {
                    if (st.top() <= nums[i]) st.pop();
                    else break;
                }
                // If the stack is empty, push -1, else push the top element of the stack
                if (st.empty()) ans.push_back(-1);
                else ans.push_back(st.top());
            }
            // Push the current element onto the stack
            st.push(nums[i]);
        }

        // Reverse the result vector to maintain the original order of elements
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```
# [Nearest Smaller Element](https://www.interviewbit.com/problems/nearest-smaller-element/)

## Approach ->
If you have solved the above two questions, then this should be easy peasy.

## Code ->
```cpp
vector<int> Solution::prevSmaller(vector<int> &A) {
    vector<int> ans;
    stack<int> st;
    for(int i=0; i<A.size(); i++){
        if(st.empty()) ans.push_back(-1);
        else if(st.top()<A[i]) ans.push_back(st.top());
        else{
            while(st.size()){
                if(st.top()<A[i]) break;
                else st.pop();
            }
            if(st.size()) ans.push_back(st.top());
            else ans.push_back(-1);
        }
        st.push(A[i]);
    }
    
    return ans;
}
```

# [735. Asteroid Collision](https://leetcode.com/problems/asteroid-collision/description/)

## Approach ->
Use a stack to simulate the collisions of asteroids while traversing the array.
If the current asteroid is positive (moving right) or the stack is empty, push it onto the stack.
If the current asteroid is negative (moving left), handle the collision logic:
Pop asteroids from the stack until a larger or equal-sized positive asteroid is encountered or the stack is empty.
If there's an equal-sized positive asteroid on top, explode both; otherwise, push the current negative asteroid.
Finally, construct the result vector from the remaining elements in the stack, reversing it to maintain order.

You could have used vector itself to replicate stacks

Solve this please because thinking about the logic here is easier and the coding part is a bit tricky and you might miss some edge cases.

## Code ->
```cpp
class Solution {
public:
    vector<int> asteroidCollision(vector<int>& ast) {
        stack<int> st; // Stack to track asteroids during collisions
        for(int i = 0; i < ast.size(); i++) {
            // If asteroid is positive (moving right) or stack is empty, push onto stack
            if(ast[i] > 0 || st.empty()) {
                st.push(ast[i]);
            } 
            else {
                // Asteroid is negative (moving left)
                // Collisions happen when moving left asteroid is bigger than the top of the stack
                // Very important conditions, if you miss any then there will be edge cases
                while(!st.empty() && st.top() > 0 && st.top() < abs(ast[i])) {
                    st.pop(); // Explode the smaller asteroid
                }
                // Explode both asteroids if they are equal in size
                if(!st.empty() && st.top() == abs(ast[i]))
                    st.pop();
                // Push the moving left asteroid onto the stack 
                // if stack is empty or there are only left moving aesteroids left in stack
                else if(st.empty() || st.top() < 0)
                    st.push(ast[i]); 
            }
        }

        vector<int> ans;
        while(!st.empty()) {
            ans.push_back(st.top());
            st.pop();
        }
        reverse(ans.begin(), ans.end()); // Reverse the order of the stack to get the final result
        return ans;
    }
};
```

# [402. Remove K Digits](https://leetcode.com/problems/remove-k-digits/description/)

## Approach ->
The code aims to find the smallest possible integer after removing k digits from the given non-negative integer num. It utilizes a stack to maintain a decreasing order of digits while iterating through the input. The algorithm considers comparisons with the stack's top, handles removals, and takes care of leading zeroes. The final result is obtained by constructing a string from the stack and reversing it. Edge cases, such as when the length of num is less than or equal to k, are handled appropriately.
Solve this question please. You weren't able to solve the edge cases at all.

## Code ->
```cpp
class Solution {
public:
    string removeKdigits(string num, int k) {
        // number of operation greater than length we return an empty string
        if(num.length() <= k)   
            return "0";
        
        // k is 0 , no need of removing /  preforming any operation
        if(k == 0)
            return num;
        
        string res = "";// result string
        stack <char> s; // char stack
        
        s.push(num[0]); // pushing first character into stack
        
        for(int i = 1; i<num.length(); ++i)
        {
            while(k > 0 && !s.empty() && num[i] < s.top())
            {
                // if k greater than 0 and our stack is not empty and the upcoming digit,
                // is less than the current top than we will pop the stack top
                --k;
                s.pop();
            }
            
            s.push(num[i]);
            
            // popping preceding zeroes
            if(s.size() == 1 && num[i] == '0')
                s.pop();
        }
        
        while(k && !s.empty())
        {
            // for cases like "456" where every num[i] > num.top()
            --k;
            s.pop();
        }
        
        while(!s.empty())
        {
            res.push_back(s.top()); // pushing stack top to string
            s.pop(); // pop the top element
        }
        
        reverse(res.begin(),res.end()); // reverse the string 
        
        if(res.length() == 0)
            return "0";
        
        return res;
        
        
    }
};
```

# [907. Sum of Subarray Minimums](https://leetcode.com/problems/sum-of-subarray-minimums/description/)

## Approach ->
 The intuition behind the solution is to leverage the concept of the Next Smaller Element (NSE) on both the left and right sides for each element in the array. By finding the indices of the next smaller element to the left (NSL) and right (NSR), the algorithm efficiently determines the number of subarrays in which the current element is the minimum. Multiplying the counts of elements on the left and right provides the total ways each element contributes to the sum of minimums across all subarrays. The overall approach optimally handles the problem of finding the sum of minimums for all contiguous subarrays in the given array.
 [VIDEO SOLUTION](https://www.youtube.com/watch?v=HRQB7-D2bi0&t=1988s)

## Code ->
```cpp
class Solution {
public:

    // Function to find the Next Smaller to Left (NSL) for each element in the array
    vector<int> findNSL(vector<int>& arr, int n){
        vector<int> result(n);
        stack <int> st;

        for(int i=0; i<n; i++){
            if(st.empty()){
                result[i] = -1;
            }
            else{
                // Pop elements from the stack until a smaller element is found or the stack is empty
                // note how we used >= here 
                while(!st.empty() && arr[st.top()] >= arr[i]) 
                    st.pop();
                
                // If stack is empty, set NSL to -1, else set NSL to the top element of the stack
                result[i] = st.empty() ? -1 : st.top();
            }
            // Push the current index onto the stack
            st.push(i);
        }
        return result;
    }

    // Function to find the Next Smaller to Right (NSR) for each element in the array
    vector<int> findNSR(vector<int>& arr, int n){
        vector<int> result(n);
        stack <int> st;

        for(int i=n-1; i>=0; i--){
            if(st.empty()){
                result[i] = n;
            }
            else{
                // Pop elements from the stack until a smaller element is found or the stack is empty
                // note how we used > here
                while(!st.empty() && arr[st.top()] > arr[i]) 
                    st.pop();
                
                // If stack is empty, set NSR to n, else set NSR to the top element of the stack
                result[i] = st.empty() ? n : st.top();
            }
            // Push the current index onto the stack
            st.push(i);
        }
        return result;
    }

    // Main function to calculate the sum of minimums for all subarrays
    int sumSubarrayMins(vector<int>& arr) {
        int n = arr.size();

        // Find the Next Smaller to Left (NSL) and Next Smaller to Right (NSR) arrays
        vector<int> NSL = findNSL(arr, n);
        vector<int> NSR = findNSR(arr, n);

        long long sum = 0;
        int MOD = 1e9+7;

        // Iterate through each element in the array
        for(int i=0; i<arr.size(); i++){
            // Calculate the number of elements to the left and right of the current element
            long long ls = i - NSL[i]; // how many elements on left of i
            long long rs = NSR[i] - i; // how many elements on right of i

            // Calculate the total number of subarrays whose minimum is arr[i]
            long long totalWays = ls * rs;

            // Calculate the sum for the current element and add it to the overall sum
            long long totalSum = arr[i] * totalWays;
            sum = (sum + totalSum) % MOD;
        }

        return sum;
    }
};
```

# [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/description/)

## Approach ->
we consider every bar to be smaller and try to find how many rectangles it can cover. The algorithm considers each bar in the histogram as a potential height for a rectangle and aims to determine the maximum width it can cover. The process involves finding the next smaller bar on the left (NSL) and the next smaller bar on the right (NSR) for each bar. The area of the rectangle is then calculated by multiplying the height with the difference between the NSR and NSL indices. The maximum area across all bars is tracked, resulting in the area of the largest rectangle in the histogram.

```cpp
class Solution {
public:

    vector<int> findNSL(vector<int>& heights){
        stack<int> st;
        vector<int> res;

        for(int i=0; i<heights.size(); i++){
            while(st.size() && heights[st.top()]>=heights[i]) st.pop();

            if(st.empty()) res.push_back(-1);
            else res.push_back(st.top());

            st.push(i);
        }
        return res;
    }
    vector<int> findNSR(vector<int>& heights){
        stack<int> st;
        vector<int> res;

        for(int i=heights.size()-1; i>=0; i--){
            while(st.size() && heights[st.top()]>=heights[i]) st.pop();

            if(st.empty()) res.push_back(heights.size());
            else res.push_back(st.top());

            st.push(i);
        }
        reverse(res.begin(), res.end()); // don't forget to reverse in the case of NSR
        return res;
    }

    int largestRectangleArea(vector<int>& heights) {
        // Obtain Next Smaller to Left (NSL) and Next Smaller to Right (NSR) arrays
        vector<int> NSL = findNSL(heights);
        vector<int> NSR = findNSR(heights);

        int ans = 0;

        // Iterate through each bar in the histogram
        for(int i = 0; i < heights.size(); i++) {
            // Calculate the potential area of the rectangle formed by the current bar
            int currentArea = heights[i] * (NSR[i] - NSL[i] - 1);

            // Update the maximum area if the current area is larger
            ans = max(ans, currentArea);
        }

        // Return the area of the largest rectangle in the histogram
        return ans;
    }
};
```

# [85. Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/description/)

## Approach ->
So basically this question is just an extension of the Largest Rectangle in Histogram i.e. the question above. We will simply traverse the 2d array and for each row try to make the blocks (rectangles) considering the numbers above. Then pass each row to our same old function of previous question to find the largest rectangle.

## Code ->
```cpp
class Solution {
public:
    vector<int> findNSL(vector<int>& heights){
        stack<int> st;
        vector<int> res;

        for(int i=0; i<heights.size(); i++){
            while(st.size() && heights[st.top()]>=heights[i]) st.pop();

            if(st.empty()) res.push_back(-1);
            else res.push_back(st.top());

            st.push(i);
        }
        return res;
    }
    vector<int> findNSR(vector<int>& heights){
        stack<int> st;
        vector<int> res;

        for(int i=heights.size()-1; i>=0; i--){
            while(st.size() && heights[st.top()]>=heights[i]) st.pop();

            if(st.empty()) res.push_back(heights.size());
            else res.push_back(st.top());

            st.push(i);
        }
        reverse(res.begin(), res.end()); // don't forget to reverse in the case of NSR
        return res;
    }
    int largestRectangleArea(vector<int>& heights) {
        // we consider every bar to be smaller and try to find how many rectangles it can cover
        vector<int> NSL = findNSL(heights);
        vector<int> NSR = findNSR(heights);

        int ans; 

        for(int i=0; i<heights.size(); i++){
            int res = heights[i] * (NSR[i] - NSL[i] - 1);
            ans = max(ans, res);
        }

        return ans;
    }
    // MAIN function (rest everything on top is same as above question)
    int maximalRectangle(vector<vector<char>>& matrix) {
        int ans = 0;
        // Convert the char matrix to an integer binary matrix for ease of calculation
        vector<vector<int>> vec(matrix.size(), vector<int>(matrix[0].size()));
        for(int i=0; i<matrix.size(); i++){
            for(int j=0; j<matrix[0].size(); j++){
                vec[i][j]=((matrix[i][j]-'0'));
            }
        }
        // Traverse each row and calculate the maximal rectangle area
        for(int i=0; i<vec.size(); i++){
            for(int j=0; j<vec[0].size(); j++){
                // Update the height for each row based on the elements above
                if(i && vec[i][j]==1){
                    vec[i][j] += vec[i-1][j];
                }
            }
            int res = largestRectangleArea(vec[i]);
            ans = max(res, ans);
        }

        return ans;
    }
};
```