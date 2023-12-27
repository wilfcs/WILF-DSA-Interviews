# [62. Unique Paths](https://leetcode.com/problems/unique-paths/)

## Approach -> 
If we reach the end (m-1 && n-1 index) then return 1. If we go out of bounds then return 0. These two are the base conditions. Now what result we have got from the left transition and the right transition will sum it up and return the answer.

---
## Code ->
```cpp
class Solution {
public:
    int helper(int m, int n, int i=0, int j=0){
        if(i==m-1 && j==n-1) return 1;
        if(i>=m || j>=n) return 0;

        return helper(m, n, i+1, j) + helper(m, n, i, j+1);
    }
    int uniquePaths(int m, int n) {
        return helper(m, n);
    }
};
```
# [78. Subsets](https://leetcode.com/problems/subsets/description/)

## Approach ->
- The code generates all possible subsets of a given array by using a recursive approach. For each element in the array, it explores two options: including the element in the current subset or excluding it. The process continues recursively until all elements are considered, and the subsets are stored in the ans vector.

TC-> O(2^N) * O(N). SC-> O(N)

---
## Code ->
```cpp
class Solution {
public:
    vector<vector<int>> ans;

    void subsets(vector<int>& nums, vector<int> temp, int n){
        if(n<0){ // if n is negative, push temp in ans
            ans.push_back(temp);
            return;
        }
        
        // Include the current element in the subset
        temp.push_back(nums[n]);
        subsets(nums, temp, n-1);

        // Exclude the current element from the subset
        temp.pop_back();
        subsets(nums, temp, n-1);
    }

    vector<vector<int>> subsets(vector<int>& nums) {
        vector<int> temp;
        int n = nums.size();

        subsets(nums, temp, n-1);
        return ans;
    }
};
```

# [ Subset Sum Equal To K](https://www.codingninjas.com/studio/problems/subset-sum-equal-to-k_1550954?leftPanelTabValue=PROBLEM)

## Approach ->
- Same as of last problem, just we need to keep a track of the sum instead of storing the subsets. Although this will throw TLE as this needs more optimization using DP, but we will see that later. 

## Code ->
- Parameterised recursion call
```cpp
#include <bits/stdc++.h> 
bool ans=false;
void helper(int n, int k, vector<int> &arr, int sum){
    if(n<0){
        if(sum==k) ans = true;
        return;
    }
    sum+=arr[n];
    helper(n-1, k, arr, sum);

    sum-=arr[n];
    helper(n-1, k, arr, sum);
}
bool subsetSumToK(int n, int k, vector<int> &arr) {
    // Write your code here.
    ans = false;
    helper(n-1, k, arr, 0);
    return ans;
}
```

- Functional recursion call
```cpp
#include <bits/stdc++.h> 
bool helper(int n, int k, vector<int> &arr, int sum){
    if(n<0){
        if(sum==k) return true;
        return false;
    }
    sum+=arr[n];
    if(helper(n-1, k, arr, sum)) return true; 
    // as soon as the helper returns true, the recursion calls stops and it starts to backtrack

    sum-=arr[n];
    if(helper(n-1, k, arr, sum)) return true;
    return false;
}
bool subsetSumToK(int n, int k, vector<int> &arr) {
    // Write your code here.
    return helper(n-1, k, arr, 0);
}
```

- The parameterised recursion will keep on executing till the very last recursive call but the functional one will stop making any further recursive calls as soon as the true condition is met.