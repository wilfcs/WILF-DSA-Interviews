Q. What is a heap?

Ans-> A heap is a specialized tree-based data structure that satisfies the heap order property. It is a complete binary tree, meaning that all levels are completely filled except possibly for the last level, which is filled from left to right. The heap order property ensures that for every node 'x' in the heap, the key of 'x' is either less than or equal to (in a max heap) or greater than or equal to (in a min heap) the keys of its children. This property allows efficient retrieval and removal of the maximum (or minimum) element from the heap, making heaps useful for priority queues and heap-sort algorithms.


# Insertion in heap
## Approach->
In the context of heaps, we conceptualize them as binary trees, akin to Binary Search Trees (BST). However, we implement heaps using an array or vector for efficient storage and retrieval. To insert an element into a max heap, the new element is placed at the last leaf node from the left. Subsequently, the algorithm checks whether the node's parent is smaller; if so, it swaps the node with its parent. The formulas for finding children and parents are: left child = 2 * i, right child = 2 * i + 1, and parent = i / 2.

## Code ->
```cpp
// Online C++ compiler to run C++ program online
#include <iostream>
#include <bits/stdc++.h>

using namespace std;

class heap {
public:
    int arr[100];
    int size;

    heap() {
        arr[0] = -1;
        size = 0;
    }

    void insert(int val) {
        size++;
        int index = size;
        arr[index] = val;

        while (index > 1) {
            int parent = index / 2;
            // If the parent is smaller than the current node, swap them
            if (arr[parent] < arr[index]) {
                swap(arr[parent], arr[index]);
                index = parent;
            } else {
                return; // Break if the heap order property is satisfied
            }
        }
    }

    void print() {
        for (int i = 1; i <= size; i++)
            cout << arr[i] << " ";
    }
};

int main() {
    heap h;
    h.insert(50);
    h.insert(55);
    h.insert(53);
    h.insert(52);
    h.insert(54);
    h.print();

    return 0;
}
```

TC-> O(log n)

# Delete from heap
## Approach->
Suppose we have to delete from a max heap, then we perform 3 steps.

1. Since we have to delete the root of the heap so we put last element of the heap into first index i.e. last leaf node to root node
2. Then we remove the last element 
3. Then we take the root node to its correct position 

## Code ->
```cpp
// Online C++ compiler to run C++ program online
#include <iostream>
#include <bits/stdc++.h>

using namespace std;

class heap{
    public:
    int arr[100];
    int size;
    
    heap(){
        arr[0] = -1;
        size = 0;
    }
    
    void insert(int val){
        size++;
        int index = size;
        arr[index] = val;
        
        while(index>1){
            int parent = index/2;
            if(arr[parent] < arr[index]){
                swap(arr[parent], arr[index]);
                index = parent;
            } 
            else return;
        }
    }
    
    void deleteFromHeap(){
        if(size==0){
            cout << "all elements deleted";
            return;
        }
        
        // step 1: put last element on first index
        arr[1] = arr[size];
        
        // step 2: remove last element
        size--;
        
        // step 3: put root node aka first elem on its correct pos
        int i = 1;
        while(i<size){
            int leftIndex = i*2;
            int rightIndex = i*2+1;
            
            // if left child is greater then swap the element with left child
            if(leftIndex<size && arr[i]<arr[leftIndex]){
                swap(arr[i], arr[leftIndex]);
                i = leftIndex;
            } 
            // if right child is greater then swap the element with right child
            else if(rightIndex<size && arr[i]<arr[rightIndex]){
                swap(arr[i], arr[rightIndex]);
                i = rightIndex;
            }
            else return;
        }
    }
    
    void print(){
        for(int i=1; i<=size; i++) cout << arr[i] << " ";
        cout << endl;
    }
};

int main() {
    heap h;
    h.insert(50);
    h.insert(55);
    h.insert(53);
    h.insert(52);
    h.insert(54);
    h.print();
    
    h.deleteFromHeap();
    h.deleteFromHeap();
    h.print();

    return 0;
}
``` 

# Heapify
## Approach ->
Heapify is a process of converting an array into a heap, where the heap property is satisfied. The heap property states that for each node 'i' at index 'i', the value of the node is greater than or equal to the values of its children (for a max heap). Similarly, for a min heap, the value of the node is less than or equal to the values of its children. We generally perform heapify for the root node. People generally forget this in the interview, so practice it once. It is a recursive approach for making a heap

# Code ->
```cpp
void heapify(vector<int>& arr, int n, int i) {
    int largest = i;
    int left = 2 * i + 1; // +1 for left and +2 for right when there is 0 based indexing
    int right = 2 * i + 2;

    // Compare with the left child
    if (left < n && arr[left] > arr[largest])
        largest = left;

    // Compare with the right child
    if (right < n && arr[right] > arr[largest])
        largest = right;

    // If the largest value is not the root, swap and recursively heapify the affected subtree
    if (largest != i) {
        swap(arr[i], arr[largest]);
        heapify(arr, n, largest);
    }
}
```
TC -> O(log n)

# [Heap Sort](https://leetcode.com/problems/sort-an-array/)

## Code ->
```cpp
class Solution {
public:
    // Function to heapify a subtree rooted at index i in a vector nums of size n
    void heapify(vector<int> &nums, int n, int i) {
        // Assume the current node is the largest
        int greatest = i;
        // Calculate indices of left and right children
        int leftChild = i * 2 + 1;
        int rightChild = i * 2 + 2;

        // Check if left child exists and is greater than the current greatest
        if (leftChild < n && nums[greatest] < nums[leftChild]) {
            greatest = leftChild;
        }
        // Check if right child exists and is greater than the current greatest
        if (rightChild < n && nums[greatest] < nums[rightChild]) {
            greatest = rightChild;
        }

        // If the greatest element is not the current root, swap and recursively heapify the affected subtree
        if (greatest != i) {
            swap(nums[i], nums[greatest]);
            heapify(nums, n, greatest);
        }
    }

    // Function to perform Heap Sort on the vector nums
    vector<int> sortArray(vector<int>& nums) {
        int n = nums.size();

        // Build a max heap by heapifying each non-leaf node in reverse order
        for(int i = n/2-1; i >= 0; i--) {
            heapify(nums, n, i);
        }

        // Extract elements from the heap one by one and heapify the remaining array
        for(int i = n-1; i > 0; i--) {
            // Swap the root (maximum element) with the last element
            swap(nums[0], nums[i]);
            // Heapify the reduced array to maintain the max heap property
            heapify(nums, i, 0);
        }

        // The vector nums is now sorted in ascending order
        return nums;
    }
};
```
## Explanation ->
Heapify Function: This function ensures that a subtree rooted at index i in the vector nums of size n satisfies the max heap property. It recursively checks and swaps elements to maintain the heap property.
Build Max Heap: The sortArray function starts by building a max heap. It iterates through non-leaf nodes in reverse order and performs heapify operations to establish the max heap property.
Heap Sort: After building the max heap, the function extracts elements from the heap one by one, placing the maximum element at the end of the vector. This process is repeated until the entire vector is sorted in ascending order.
The overall strategy is to use a max heap to efficiently find and extract the maximum element at each step, resulting in a sorted array.

# Priority Queue STL
- Priority queue works exactly like heap it is just an stl. syntax - priority_queue<int> pq;
- To build a min heap -> priority_queue<int, vector<int>, greater<int>> pq;
- Essential operations include top(), pop(), and push(). These operations have time complexities of O(log n) for push() and top(), and O(1) for pop().

# 215. Kth Largest Element in an Array
## Approaches ->
1. Other than sorting the array approach which will be your brute force with TC-> O(n log n), we can use the max heap and insert all the elements inside, then pop back k times and return the top element. TC-> O(n log n) because the TC for traversal in the array is n and insertion of each element in max heap will take O(log n) time complexity
2. We can use min heap to decrease the time complexity of the above approach. The idea is to push first k elements into min heap by traversing the array k times. Now traverse the remaining array and compare if the element on ith index is greater than the top of min heap. If yes, pop the top element of min heap and insert the element on ith index. That way there will always be the kth largest element on the top of the min heap after the traversal is completed. Dry run the code to understand. Best TC-> O(n log k) because we are traversing the entire array by only pushing the elements k times. Worst time complexity will obviously be O(n log n)
3. By hashing we can solve this question. Simply create an unordered map and store the frequency of each element as well as the min and max element. Traverse from max to min in backwards order and subtract the frequency from k. When k reaches 0, that element is your answer. TC-> O ( n + min(nums[i]) + max(nums[i]) )

---
## Codes->
2. 
```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int, vector<int>, greater<int>> min_heap;

        int i=0;
        while(k--) min_heap.push(nums[i++]);

        for(i; i<nums.size(); i++){
            if(nums[i]>min_heap.top()){
                min_heap.pop();
                min_heap.push(nums[i]);
            }
        }

        return min_heap.top();
    }
};
```
3. 
```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        unordered_map <int, int> mp;
        int maxim = INT_MIN;
        int minim = INT_MAX;
        for(int i=0; i<nums.size(); i++){
            maxim = max(maxim, nums[i]);
            minim = min(minim, nums[i]);
            mp[nums[i]]++;
        }

        for(int i=maxim; i>=minim; i--){
            if(mp.find(i)!=mp.end()){
                k-=mp[i];
                if(k<=0) return i;
            }
        }
        return -1;
    }
};
```

Note: Kth Smallest Element in an Array can also be solved using the approaches above we just have to use max heap instead of min heap and if the element is smaller than heap's top then pop and push that elem in.

# [Is Binary Tree Heap](https://www.geeksforgeeks.org/problems/is-binary-tree-heap/1)

## Approach ->
In the provided code, the isHeap function checks whether a given binary tree follows the max heap property. The approach involves three helper functions: countNodes to calculate the total number of nodes, isCBT to verify if the tree is a complete binary tree, and isMaxOrder to ensure it adheres to the max heap ordering. The key idea is to validate completeness first, and then verify the max heap property recursively. If both conditions hold, the tree is deemed a max heap.

## Code ->
```cpp
class Solution {
public:
    // Helper function to count the number of nodes in the binary tree
    int countNodes(struct Node* tree) {
        if (tree == NULL) return 0;
        return 1 + countNodes(tree->left) + countNodes(tree->right);
    }

    // Helper function to check if the binary tree is a complete binary tree
    bool isCBT(struct Node* tree, int index, int cnt) {
        if (tree == NULL) return true;

        if (index >= cnt) return false;

        bool left = isCBT(tree->left, index * 2 + 1, cnt);
        bool right = isCBT(tree->right, index * 2 + 2, cnt);

        return left && right;
    }

    // Helper function to check if the binary tree follows max heap property
    bool isMaxOrder(struct Node* tree) {
        if (tree->left == NULL && tree->right == NULL) return true;

        if (tree->right == NULL) {
            return tree->data > tree->left->data;
        } else {
            bool left = isMaxOrder(tree->left);
            bool right = isMaxOrder(tree->right);

            return (left && right && tree->data > tree->left->data && tree->data > tree->right->data);
        }
    }

    // Main function to check if the binary tree is a max heap
    bool isHeap(struct Node* tree) {
        int index = 0;
        int totalCount = countNodes(tree);
        
        // Check if the binary tree is a complete binary tree and follows max heap property
        if (isCBT(tree, index, totalCount) && isMaxOrder(tree)) return true;
        else return false;
    }
};
```

# [Merge two binary Max heaps](https://www.geeksforgeeks.org/problems/merge-two-binary-max-heap0144/1)

## Approach ->
Just merge the two given arrays and then heapify them to get your answer.

## Code ->
```cpp
class Solution{
public:
    // Function to heapify a subtree rooted at node i in the given vector 'a' of size 'n'
    void heapify(vector<int> &a, int i, int n){
        int largest = i;
        int left = i * 2 + 1;
        int right = i * 2 + 2;

        // Compare with left child
        if (left < n && a[left] > a[largest])
            largest = left;

        // Compare with right child
        if (right < n && a[right] > a[largest])
            largest = right;

        // If the largest is not the current node, swap and recursively heapify
        if (i != largest){
            swap(a[i], a[largest]);
            heapify(a, largest, n);
        }
    }

    // Function to merge two binary max heaps represented by vectors 'a' and 'b' of sizes 'n' and 'm'
    vector<int> mergeHeaps(vector<int> &a, vector<int> &b, int n, int m) {
        // Merge both heaps into one vector 'a'
        for (int i = 0; i < m; i++)
            a.push_back(b[i]);

        // Build heap using the merged array
        for (int i = (n + m) / 2 + 1; i >= 0; i--){
            heapify(a, i, a.size());
        }

        return a;
    }
};
```

So during heapifying the array why do we run the loop from size/2-1? That's because when heapifying an array to maintain the heap property, we start the loop from size/2-1 because all the elements beyond this point are leaf nodes in the binary heap. Since leaf nodes don't have children, there is no need to perform heapification on them, making the process more efficient by avoiding unnecessary operations on nodes that won't affect the heap structure.

# [Minimum Cost of ropes](https://www.geeksforgeeks.org/problems/minimum-cost-of-ropes-1587115620/1)

## Code ->
```cpp
class Solution
{
    public:
    //Function to return the minimum cost of connecting the ropes.
    long long minCost(long long arr[], long long n) {
        // Your code here
        priority_queue<long long, vector<long long>, greater<long long>> min_heap(arr, arr+n);

        long long ans = 0;
        
        while(min_heap.size()>1){
            long long n1 = min_heap.top();
            min_heap.pop();
            long long n2 = min_heap.top();
            min_heap.pop();
            
            ans+=n1+n2;
            min_heap.push(n1+n2);
        }
        
        return ans;
    }
};
```

# [K-th Largest Sum Subarray](https://www.codingninjas.com/studio/problems/k-th-largest-sum-contiguous-subarray_920398?leftPanelTabValue=PROBLEM)

## Approach ->
1. Run a n^2 loop and store the subarray sum in a vector. Then sort the vector to find your answer. The complexity analysis is in the code itself.
2. Use a min heap to find the kth largest sum subarray. Find out yourself by looking at the code how we did that.

## Code ->
1. 
```cpp
#include <bits/stdc++.h>
int getKthLargest(vector<int> &arr, int k)
{
	//	Write your code here.
	vector<int> ans;


	for(int i=0; i<arr.size(); i++){
		int sum = 0;
		for(int j=i; j<arr.size(); j++){
			sum+=arr[j];
			ans.push_back(sum);
		}
	}

	sort(ans.begin(), ans.end());

	return ans[ans.size()-k];
}

// TC -> O(n^2 * log(n^2)) = O(2 * n^2 * log(n)) = O(n^2 * log(n))
// This is because sorting TC is O(n log n) and the ans array that we are sorting is already of size n^2.
//SC->O(n^2)
```
2. 
```cpp
#include <bits/stdc++.h>
int getKthLargest(vector<int> &arr, int k)
{
	//	Write your code here.
	priority_queue<int, vector<int>, greater<int>> min_heap;

	for(int i=0; i<arr.size(); i++){
		int sum = 0;
		for(int j=i; j<arr.size(); j++){
			sum+=arr[j];

			if(min_heap.size()<k) min_heap.push(sum);
			else{
				if(min_heap.top()<sum){
					min_heap.pop();
					min_heap.push(sum);
				}
			}
		}
	}
	return min_heap.top();
}

//  TC->O(n^2 * log(k))
// SC->O(n)
```

# [Merge K Sorted Arrays](https://www.codingninjas.com/studio/problems/merge-k-sorted-arrays_975379)
--- Pending

# [632. Smallest Range Covering Elements from K Lists](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/description/) 
--- Pending

# [295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/description/)

## Approach ->
We have to return the median, so if n is odd we just have to return the middle element and if n is even we return the sum of two middle elements divided by 2. Coming to the question, we can imagine this large stream of data in two parts from the middle. eg-> 1,2,3,4,5 can be divided into two parts i.e. 1,2,3 and 4,5. To represent the left part we can take a max heap and to represent the right part from the middle we can take a min heap. That way the top of both the heaps will always give the middle element. So the top of both the heaps in our example will give us 3 and 4. That way we can easily find our median by looking at the value of n that we will also be maintaining. 

Now, how do we insert in the heaps? We know for a fact that the left heap will always be equal in size or one greater than the right heap. So first when we encounter an element just check if it is smaller than the top of left max heap, if it is then push the element in the left heap because it belongs to the left part of the array (data stream) else push it in the right part. Then check if the size of right heap became greater than the left heap, if yes then pop one element from the top and push it in left heap. If the size of left heap is two greater than the right heap then do the same, pop and push in right. We can now easily find out our answer. Look at the code and you'd understand.

## Code ->
```cpp
class MedianFinder {
public:
    // Max heap for the left part of the array (data stream)
    priority_queue<int> leftMaxHeap;
    // Min heap for the right part from the middle of the array (data stream)
    priority_queue<int, vector<int>, greater<int>> rightMinHeap;
    // Total number of elements in the data stream
    int n = 0;

    // Constructor to initialize the MedianFinder object
    MedianFinder() {
    }
    
    // Function to add an integer 'num' from the data stream to the data structure
    void addNum(int num) {
        // Check if the leftMaxHeap is empty or if the current element belongs to the left part
        if (leftMaxHeap.size() == 0 || leftMaxHeap.top() > num)
            leftMaxHeap.push(num);
        else
            rightMinHeap.push(num);

        // Balance the heaps if necessary
        if (rightMinHeap.size() > leftMaxHeap.size()) {
            int temp = rightMinHeap.top();
            rightMinHeap.pop();
            leftMaxHeap.push(temp);
        } else if (leftMaxHeap.size() > rightMinHeap.size() + 1) {
            int temp = leftMaxHeap.top();
            leftMaxHeap.pop();
            rightMinHeap.push(temp);
        }

        // Increment the total count of elements
        n++;
    }
    
    // Function to return the median of all elements so far in the data stream
    double findMedian() {
        // Check if the total count of elements is odd
        if (n % 2 == 1)
            return leftMaxHeap.top();
        // If even, return the average of the two middle elements
        else
            return double(double(leftMaxHeap.top() + rightMinHeap.top()) / 2);
    }
};
```

# [621. Task Scheduler](https://leetcode.com/problems/task-scheduler/description/)

## Approach ->
This question is based on a few observations. First observation is the question itself which you might interpret wrong. To understand the question let's take a custom testcase. 

tasks = A, A, A, A, A, B, B, B, C, C. n = 2.

Here we have 5 A's, 3 B's and 2 C's. ATQ we can repeat a number after n times. So we can repeat let's say A after 2 times. Another observation can be that the element with the most frequency should be taken first and given the most priority because we want to finish them first. So in this case A we will try to finish first.

So the CPU Cycles should be -> A, B, C, A, B, C, A, B, IDLE, A, IDLE, IDLE, A.
Did you notice something here? We are making a window of size n+1 with unique elements here. Here the value of n is 2 so the window size is 3. We can use this for solving the question. 

How to code this question?
To code the question follow these following steps, keep in mind that we don't need the characters to solve the question we will just need their frequencies so we will simply extract the frequencies of the chars and store them in our max heap to solve the question.

Steps:
1. Frequency Map: Start by creating a frequency map (unordered_map) to count the occurrences of each task.

2. Priority Queue: Use a priority queue (max heap) to store their frequency. This is because we want to process the most frequent tasks first. We don't need the characters at this point, we will proceed with the frequencies only.

3. Greedy Approach: In each iteration, create a window of size (n+1) to represent one cycle. This window ensures that we adhere to the cooling constraint.

4. Process Tasks: In each cycle, pop tasks from the priority queue and decrease their frequency. Append these tasks with one decreased frequency to a temporary vector.

5. Reinsert Tasks: After processing tasks in the cycle, reinsert the tasks with remaining frequency back into the priority queue.

6. Idle Time: If the priority queue is empty after processing tasks in the cycle, it means there are no more tasks left and we were able to finish all tasks before the n+1 cycle. In this case, add the actual time taken in the cycle to the final answer. Otherwise, add the maximum possible time for a full cycle (n+1).

7. Repeat: Continue this process until there are no more tasks left in the frequency map.

## Code ->
```cpp
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        unordered_map<char, int> mp;

        // Step 1: Frequency Map
        for(int i = 0; i < tasks.size(); i++) 
            mp[tasks[i]]++;

        priority_queue<int> pq;

        // Step 2: Create Priority Queue with just the frequencies
        for(auto a: mp) 
            pq.push(a.second);

        int ans = 0;

        // Step 3-7: Greedy Approach
        while(pq.size()){
            int time = 0;
            vector<int> temp;

            // Step 3: Iterate for the Window of size n+1
            for(int i = 0; i < n + 1; i++){
                if(pq.empty()) break;
                // Push the updated frequency in the temp vector
                temp.push_back(pq.top() - 1);
                pq.pop();
                time++;
            }

            // Step 4: Repopulate the priority queue
            for(int i = 0; i < temp.size(); i++){
                if(temp[i] > 0) 
                    pq.push(temp[i]);
            }

            // Step 5-7: Reinsert Tasks, Idle Time, Repeat
            ans += pq.empty() ? time : n + 1;
        }

        return ans;
    }
};
```
## Complexity Analysis ->
The time complexity of the provided code is O(N log N), where N is the number of tasks.
- The creation of the frequency map takes O(N) time, where N is the number of tasks.
- The priority queue operations involve inserting and extracting elements, each of which takes O(log N) time. In the worst case, we might insert and extract all tasks, resulting in O(N log N) for priority queue operations.