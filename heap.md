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