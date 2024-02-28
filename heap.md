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




# 215. Kth Largest Element in an Array
## Approaches ->
1. We can use the max heap and insert all the elements inside, then pop back k times and return the top element. TC-> O(n log n) because the TC for traversal in the array is n and insertion of each element in max heap will take O(log n) time complexity
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