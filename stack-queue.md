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

# [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/description/)

## Approach ->
These types of questions use a concept called monotonic deque. This is because monotonic deque allows for efficient tracking of the maximum element within the sliding window. The deque (double-ended queue) maintains elements in a way that preserves their monotonicity, ensuring that the front of the deque always holds the maximum element in the current window.

This question can be solved using the following 4 steps:

Step1: When a new element comes make room for it by adjusting your window and popping the old element from the front of deque.

Step 2: We know from the concept of monotonic stack that the greatest number in the stack should always be at the front of deque, so after adjusting the window size we must maintain this propery of monotonic deque. To do this we compare the new element from the back of the deque and if the new element is greater then we keep popping from the back. That way if there were no elements greater than the new element then the queue will get empty and if there was a greater element in the queue already then it will still be on the front of the deque maintaining the deque's propery.

Step 3: Now we add the new element to the back of the deque. If that element is the greatest it would also be the front of the queue and if somone else is greatest then that will be the front of the queue. Hence the propery of monotonic deque that the greatest element should be at the front in maintained.

Step 4: Add to your answer the front of the deque.

Read the code to understand the approach. TC->O(N)

## Code ->
```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> ans;
        deque<int> q;

        for(int i = 0; i < nums.size(); i++){
            // Step 1: Make room for the new element by adjusting the window
            while(!q.empty() && q.front() <= (i - k))
                q.pop_front();
            
            // Step 2: Maintain monotonic deque property by popping from the back
            while(!q.empty() && nums[i] > nums[q.back()])
                q.pop_back();

            // Step 3: Add the new element to the back of the deque
            q.push_back(i);
            
            // Step 4: Add the front of the deque to the answer
            if(i >= k - 1) 
                ans.push_back(nums[q.front()]);
        }

        return ans;
    }
};
```

# [901. Online Stock Span](https://leetcode.com/problems/online-stock-span/description/)

## Approach ->
Using the monotonic stack again. Initialize a stack to store pairs of prices and spans. When we spot a price that is greater than the previous price that is on top of the stack, we add the span of that price in our current price and keep doing it till we not find a smaller price. Then we push the new price with its calculated span in the stack and return its stack. In other words here are the steps we are going to perform..

1. Initialize a stack: Start with an empty stack.

2. Process each price: As we go through each stock price:
- Check if it's greater than the previous price on the stack.
- If yes, add the span of that price to our current price.
- Keep doing this until we find a smaller or equal price.
- Then, push the new price along with its calculated span onto the stack.

3. Repeat for all prices: Go through all the prices, following the same steps.

4. Return the stack: The stack now holds pairs of prices and their corresponding spans.

## Code ->
```cpp
class StockSpanner {
public:
    stack<pair<int, int>> st;  // Monotonic stack to store pairs of prices and spans.

    StockSpanner() {
        // Constructor: Initializes the object of the class.
    }
    
    int next(int price) {
        int span = 1;  // Currently span of price is 1 i.e. itself
        
        // Iterate through the stack to find consecutive days where the stock price is less than or equal to the current price.
        while (!st.empty() && st.top().first <= price) {
            span += st.top().second;  // Add the span of the current price on top of the stack to the current span.
            st.pop();  // Pop the pair from the stack as we've accounted for its span.
        }
        
        // Push the current price and its calculated span onto the stack for future references.
        st.push({price, span});
        
        return span;  // Return the calculated span for the current day.
    }
};
```

# [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/description/)

## Approach ->
Using the monotonic stack

## Codes->
1. SC-> O(2n)
```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        stack<pair<int, int>> st;  
        vector<int> ans(temperatures.size());  

        // Traverse the temperatures array from right to left.
        for(int i = temperatures.size() - 1; i >= 0; i--){
            int days_to_wait = 1;  // Initialize the number of days to wait.

            // Main step: Iterate through the stack to find consecutive days with temperatures less than the current day.
            while(st.size() && temperatures[i] >= st.top().first){
                days_to_wait += st.top().second;  // Add the days to wait for the current temperature.
                st.pop();  // Pop the pair from the stack as we've accounted for its days.
            }

            // If the stack is empty, it means there is no future day with a warmer temperature.
            if(st.empty()) 
                days_to_wait = 0;

            ans[i] = days_to_wait;

            // Push the current temperature and its calculated days to wait onto the stack for future references.
            st.push({temperatures[i], days_to_wait});
        }

        return ans; 
    }
};

2. SC-> O(n)
```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        vector<int> ans(temperatures.size(), 0);  // Result vector to store the number of days to wait for a warmer temperature.
        stack<int> st;  // Monotonic stack to store indices of temperatures.

        // Traverse the temperatures array from right to left.
        for(int i = temperatures.size() - 1; i >= 0; i--){
            // Main Step: Iterate through the stack to find indices of temperatures less than or equal to the current day's temperature.
            while(st.size() && temperatures[st.top()] <= temperatures[i])
                st.pop();

            // If the stack is not empty, calculate the number of days to wait for a warmer temperature.
            if(!st.empty()) 
                ans[i] = st.top() - i;

            // Push the current index onto the stack.
            st.push(i);
        }

        return ans; 
    }
};
```
# [844. Backspace String Compare](https://leetcode.com/problems/backspace-string-compare/description/)

## Approach ->
Create 2 separate stacks s1 and s2. If you encounter a "#" pop the element from stack. Else push all the elements in the stack.

## Code ->
```cpp
class Solution {
public:
    bool backspaceCompare(string s, string t) {
        stack<char> s1;
        stack<char> s2;

        for(int i=0; i<s.size(); i++){
            if(s[i]=='#'){
                if(s1.size()) s1.pop();
            }
            else s1.push(s[i]);
        }
        for(int i=0; i<t.size(); i++){
            if(t[i]=='#'){
                if(s2.size()) s2.pop();
            }
            else s2.push(t[i]);
        }

        return s1==s2;
    }
};
```

# [The Celebrity Problem](https://www.geeksforgeeks.org/problems/the-celebrity-problem/1?itm_source=geeksforgeeks&itm_medium=article&itm_campaign=bottom_sticky_on_article)

## Approach ->
1. TC->O(N*N) -> Iterate through each row and check if all elements in that row are 0 i.e. the row's person doesn't know anyone. Then check all the columns for that row if all it's column other than the diagonal column (for example M[1][1] or M[2][2]) are 1. If yes then that confirms that the row we are currently on is our celebrity. Else keep checking for all the other rows.
2. TC->O(N) -> Take a stack and insert all from 0 to n-1 in the stack i.e. all possible people who can be a celebrity. Now pop two people (a and b) from the stack and check if a knows b. If a knows b then a cannot be the celebrity hence push b in the stack and vice versa. Then when we have just one element left in the stack then that element/person is our potential celeb but we're not sure yet. So we that person/row if that is the real celeb by the method above. If it is the celeb then we return its index else we return -1. 

## Code ->
```cpp
class Solution 
{
public:
    // Function to find if there is a celebrity in the party or not.
    int celebrity(vector<vector<int>>& M, int n) 
    {
        stack<int> st;  // Stack to store potential celebrity candidates.
        
        // Initializing the stack with all potential celebrity candidates.
        for(int i = 0; i < M.size(); i++)
            st.push(i);
        
        // Stack-based elimination process to find a potential celebrity.
        while(st.size() > 1)
        {
            int a = st.top();
            st.pop();
            
            int b = st.top();
            st.pop();
            
            // If a knows b, eliminate a; else, eliminate b.
            if(M[a][b] == 1) 
                st.push(b);
            else 
                st.push(a);
        }
        
        int potential_celeb = st.top();  // Potential celebrity candidate.
        
        // Final check to verify if the potential celebrity does not know anyone.
        for(int i = 0; i < M[0].size(); i++)
        {
            if(M[potential_celeb][i] == 1) 
                return -1;
        }
        
        int cnt = 0;
        
        // Final check to count the number of people who know the potential celebrity.
        for(int i = 0; i < M.size(); i++)
        {
            if(M[i][potential_celeb] == 1) 
                cnt++;
        }
        
        // If the potential celebrity is known by everyone except themselves, return their index; else, return -1.
        if(cnt == (M.size() - 1)) 
            return potential_celeb;
        else 
            return -1;
    }
};
```

# [1047. Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/description/)

## Approach ->
Just use stack, simple q.

## Code ->
```cpp
class Solution {
public:
    string removeDuplicates(string s) {
        stack<char> st;

        for(int i=s.size()-1; i>=0; i--){
            if(st.empty() || st.top() != s[i]) st.push(s[i]);
            else st.pop();
        }
        string ans = "";
        while(st.size()){
            ans+=st.top();
            st.pop();
        }
        return ans;
    }
};
```