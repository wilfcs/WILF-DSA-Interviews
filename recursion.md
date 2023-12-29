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

# Merge Sort

## Approach ->
- Merge Sort is a divide and conquers algorithm, it divides the given array into equal parts and then merges the 2 sorted parts. 
- There are 2 main functions :
-  merge(): This function is used to merge the 2 halves of the array. It assumes that both parts of the array are sorted and merges both of them.
-  mergeSort(): This function divides the array into 2 parts. low to mid and mid+1 to high where,

```
low = leftmost index of the array

high = rightmost index of the array

mid = Middle index of the array 
```

We recursively split the array, and go from top-down until all sub-arrays size becomes 1.

---
## Code ->
```cpp
#include <bits/stdc++.h>
using namespace std;

void merge(vector<int> &arr, int low, int mid, int high) {
    vector<int> temp; // temporary array
    int left = low;      // starting index of left half of arr
    int right = mid + 1;   // starting index of right half of arr

    //storing elements in the temporary array in a sorted manner//

    while (left <= mid && right <= high) {
        if (arr[left] <= arr[right]) {
            temp.push_back(arr[left]);
            left++;
        }
        else {
            temp.push_back(arr[right]);
            right++;
        }
    }

    // if elements on the left half are still left //

    while (left <= mid) {
        temp.push_back(arr[left]);
        left++;
    }

    //  if elements on the right half are still left //
    while (right <= high) {
        temp.push_back(arr[right]);
        right++;
    }

    // transfering all elements from temporary to arr //
    for (int i = low; i <= high; i++) {
        arr[i] = temp[i - low];
    }
    // instead of using i-low we could have created another variable x=0 and could have done temp[x++]
}

void mergeSort(vector<int> &arr, int low, int high) {
    if (low >= high) return;
    int mid = (low + high) / 2 ;
    mergeSort(arr, low, mid);  // left half
    mergeSort(arr, mid + 1, high); // right half
    merge(arr, low, mid, high);  // merging sorted halves
}

int main() {

    vector<int> arr = {9, 4, 7, 6, 3, 1, 5}  ;
    int n = 7;

    cout << "Before Sorting Array: " << endl;
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " "  ;
    }
    cout << endl;
    mergeSort(arr, 0, n - 1);
    cout << "After Sorting Array: " << endl;
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " "  ;
    }
    cout << endl;
    return 0 ;
}
```
Time complexity: O(nlogn) 

Reason: At each step, we divide the whole array into two halves, for that logn and we assume n steps are taken to get sorted array, so overall time complexity will be nlogn

Space complexity: O(n)  

Reason: We are using a temporary array to store elements in sorted order.

# Quick Sort

## Approach/Intuition ->
Quick Sort is a divide-and-conquer algorithm like the Merge Sort. But unlike Merge sort, this algorithm does not use any extra array for sorting(though it uses an auxiliary stack space). So, from that perspective, Quick sort is slightly better than Merge sort.

This algorithm is basically a repetition of two simple steps that are the following:

Pick a pivot and place it in its correct place in the sorted array.
Shift smaller elements(i.e. Smaller than the pivot) on the left of the pivot and larger ones to the right.
Now, let’s discuss the steps in detail considering the array {4,6,2,5,7,9,1,3}:

Step 1: The first thing is to choose the pivot. A pivot is basically a chosen element of the given array. The element or the pivot can be chosen by our choice. So, in an array a pivot can be any of the following:

The first element of the array
The last element of the array
Median of array
Any Random element of the array
After choosing the pivot(i.e. the element), we should place it in its correct position(i.e. The place it should be after the array gets sorted) in the array. For example, if the given array is {4,6,2,5,7,9,1,3}, the correct position of 4 will be the 4th position.

Note: Here in this tutorial, we have chosen the first element as our pivot. You can choose any element as per your choice.

Step 2: In step 2, we will shift the smaller elements(i.e. Smaller than the pivot) to the left of the pivot and the larger ones to the right of the pivot. In the example, if the chosen pivot is 4, after performing step 2 the array will look like: {3, 2, 1, 4, 6, 5, 7, 9}. 

From the explanation, we can see that after completing the steps, pivot 4 is in its correct position with the left and right subarray unsorted. Now we will apply these two steps on the left subarray and the right subarray recursively. And we will continue this process until the size of the unsorted part becomes 1(as an array with a single element is always sorted).

So, from the above intuition, we can get a clear idea that we are going to use recursion in this algorithm.

To summarize, the main intention of this process is to place the pivot, after each recursion call, at its final position, where the pivot should be in the final sorted array.

## Code ->
```cpp
#include <bits/stdc++.h>
using namespace std;

// Function to partition the array and return the pivot index
int partition(vector<int> &arr, int low, int high) {
    // Choose the first element as the pivot
    int pivot = arr[low];
    int i = low;
    int j = high;

    // Move elements smaller than the pivot to the left and larger to the right
    while (i < j) {
        while (arr[i] <= pivot && i <= high - 1) {
            i++;
        }

        while (arr[j] > pivot && j >= low + 1) {
            j--;
        }

        // Swap arr[i] and arr[j] if they are out of order
        if (i < j) swap(arr[i], arr[j]);
    }

    // Swap the pivot element to its correct position
    swap(arr[low], arr[j]);

    // Return the index of the pivot element
    return j;
}

// Quicksort recursive function
void qs(vector<int> &arr, int low, int high) {
    // Base case: if the partition size is greater than 1
    if (low < high) {
        // Find the pivot index such that elements on the left are smaller and on the right are larger
        int pIndex = partition(arr, low, high);

        // Recursively sort the subarrays on the left and right of the pivot
        qs(arr, low, pIndex - 1);
        qs(arr, pIndex + 1, high);
    }
}

// Wrapper function for quicksort
vector<int> quickSort(vector<int> arr) {
    // Call the quicksort function with the entire array
    qs(arr, 0, arr.size() - 1);

    // Return the sorted array
    return arr;
}

int main() {
    // Sample array for testing
    vector<int> arr = {4, 6, 2, 5, 7, 9, 1, 3};
    int n = arr.size();

    // Display the array before using quicksort
    cout << "Before Using Quick Sort: " << endl;
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;

    // Call the quicksort function and display the sorted array
    arr = quickSort(arr);
    cout << "After Using Quick Sort: " << "\n";
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << "\n";

    return 0;
}
```
- Time Complexity: O(N*logN), where N = size of the array.
- Space Complexity: O(1) + O(N) auxiliary stack space.

# Implement string to integer

## Code ->
```cpp
class Solution {
public:
    int helper(string s, int n){
        if (n == 0) 
            return s[n] - '0';

        // Recursive case: combine previous value with current digit
        int preNums = helper(s, n - 1);
        int curNum = s[n] - '0';

        // Combine values by multiplying the previous value by 10 and adding the current value
        return preNums * 10 + curNum;
    }
    int myAtoi(string s) {
        return helper(s, s.size()-1);
    }
};
```

# [50. Pow(x, n)](https://leetcode.com/problems/powx-n/description/)

## Approach ->
The recursive approach leverages the observation that exponentiation problems, such as x^n, can be optimized by breaking them down into subproblems. By dividing the exponent (n) into halves, the recursive function efficiently calculates the result, taking advantage of the fact that we can write x^8 as (x^4 * x^4) and we can write x^9 as (x^4 * x^4 * x) This approach reduces the time complexity to logarithmic (log n) by repeatedly halving the exponent.

## Code ->
```cpp
#include <cmath>

class Solution {
public:
    double helper(double x, int n) {
        // Base case: if x is 0, result is 0
        if (x == 0) return 0;
        // Base case: if n is 0, result is 1
        if (n == 0) return 1;

        // Recursive calculation for half of the exponent
        double res = helper(x, n / 2);
        // Square the result obtained from the recursive call
        res = res * res;

        // Adjust result for even exponent
        if (n % 2 == 0) return res;
        // Multiply the result by x for odd exponent
        else return res * x;
    }

    // Main function to calculate power with handling for negative exponent
    double myPow(double x, int n) {
        // Call the helper function to calculate power
        double ans = helper(x, n);

        // Adjust the result for negative exponent
        if (n < 0) return 1 / ans;
        else return ans;
    }
};
```

# [1922. Count Good Numbers](https://leetcode.com/problems/count-good-numbers/description/)

## Approach -> 
It is a simple PNC question where we mod everything and make everything long long as much as possible to avoid wrong answers due to overflow. Please remember to make everything long long as much as possible during the interview. We know that the even places will have 5 possible numbers(0,2,4,6,8) and odd places will have 4 possible numbers(1,3,5,7). So we will simply return 5^even * 4^odd as our answer and find the power using recursion because we need to mod everything inside power function.

## Code -> 
```cpp
class Solution {
public:
    int MOD = 1000000007;
    int power(long long int n, long long int m){
        if(n==0) return 0;
        if(m==0) return 1;

        long long int res = power(n, m/2);
        res = (res*res)%MOD;
        if(m%2==0) return (res);
        else return (res*n)%MOD;
    }
    int countGoodNumbers(long long n) {
        long long int odd = n/2;
        long long int even = (n+1)/2;

        long long int first = power(5, even);
        long long int second = power(4, odd);

        return int((first*second)%MOD);
    }
};
```

# [39. Combination Sum](https://leetcode.com/problems/combination-sum/)

## [Approach](https://takeuforward.org/data-structure/combination-sum-1/)

## Code ->
```cpp
class Solution {
public:
    vector<vector<int>> ans;

    void helper(vector<int>& candidates, int target, vector<int> temp, int sum, int idx){
        if(sum>target) return;
        if(idx==candidates.size()){
            if(sum==target) ans.push_back(temp);

            return;
        }

        temp.push_back(candidates[idx]);
        helper(candidates, target, temp, sum+candidates[idx], idx);

        temp.pop_back();
        helper(candidates, target, temp, sum, idx+1);
    }

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<int> temp;
        helper(candidates, target, temp, 0, 0);
        return ans;
    }
};
```
Time Complexity: O(2^t * k) where t is the target, k is the average length

Reason: Assume if you were not allowed to pick a single element multiple times, every element will have a couple of options: pick or not pick which is 2^n different recursion calls, also assuming that the average length of every combination generated is k. (to put length k data structure into another data structure)

Why not (2^n) but (2^t) (where n is the size of an array)?

Assume that there is 1 and the target you want to reach is 10 so 10 times you can “pick or not pick” an element.

Space Complexity: O(k*x), k is the average length and x is the no. of combinations

# [40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/description/)

## [Approach (look at the recursive tree)](https://takeuforward.org/data-structure/combination-sum-ii-find-all-unique-combinations/)
Note: In this question we are required to ignore duplicates and return the answer in sorted order.

## Code ->
```cpp
class Solution {
public:
    vector<vector<int>> ans;

    void helper(vector<int>& candidates, int target, vector<int> temp, int sum, int idx){
        if(sum>target) return;
        if(sum==target){
            ans.push_back(temp);
            return;
        }

        for(int i=idx; i<candidates.size(); i++){
            // Skip duplicates to avoid duplicate combinations
            if(i>idx && candidates[i]==candidates[i-1]) continue;
            // If the current candidate is greater than the target, break the loop. This will optimize the code more
            if(candidates[i]>target) break;

            // Include the current candidate in the temporary combination
            temp.push_back(candidates[i]);
            // Recursively call the helper function with updated parameters
            helper(candidates, target, temp, sum+candidates[i], i+1);
            // Remove the last added element to backtrack and explore other combinations
            temp.pop_back();

            // helper(candidates, target, temp, sum, idx+1); -> we will not include this line like the previous solution. reason given below.
        }
        

        
    }

    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        vector<int> temp;
        helper(candidates, target, temp, 0, 0);
        return ans;
    }
};
```
Why did we not include helper(candidates, target, temp, sum, idx+1);

Reason ->  in the current structure of the code, this line is not required and can lead to incorrect results. The reason is that the recursive call with idx+1 already covers the case of excluding the current element at index i. By making another recursive call with the same idx, you are essentially repeating the same combinations and potentially skipping valid combinations.

The correct structure is to make a single recursive call for each element at index i, and the loop itself takes care of iterating through all possible elements starting from the current index. 

Time Complexity:O(2^n*k)

Reason: Assume if all the elements in the array are unique then the no. of subsequence you will get will be O(2^n). we also add the ds to our ans when we reach the base case that will take “k”//average space for the ds.

Space Complexity:O(k*x)

Reason: if we have x combinations then space will be x*k where k is the average length of the combination.

# [Subset Sum](https://www.codingninjas.com/studio/problems/subset-sum_3843086?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf&leftPanelTabValue=PROBLEM)

## Approach -> 
Easy peasy using recursion

## Code ->
```cpp
void helper(vector<int> num, int sum, int idx, vector<int> &ans){
	if(idx==num.size()){
		ans.push_back(sum);
		return;
	}
	helper(num, sum+num[idx], idx+1, ans);
	helper(num, sum, idx+1, ans);
}
vector<int> subsetSum(vector<int> &num){
	// Write your code here.	
	vector<int> ans;
	helper(num, 0, 0, ans);
	sort(ans.begin(), ans.end());
	return ans;
}
```

# [90. Subsets II](https://leetcode.com/problems/subsets-ii/description/)

## Approach ->
Lets  understand  with an example where arr = [1,2,2 ].

Initially start with an empty data structure. In the first recursion, call make a subset of size one, in the next recursion call a subset of size 2, and so on. But first, in order to make a subset of size one what options do we have?

We can pick up elements from either the first index or the second index or the third index. However, if we have already picked up two from the second index, picking up two from the third index will make another duplicate subset of size one. Since we are trying to avoid duplicate subsets we can avoid picking up from the third index. This should give us an intuition that whenever there are duplicate elements in the array we pick up only the first occurrence.

The next recursion calls will continue from the point the previous one ended.

Let’s summarize:-

Sort the input array.Make a recursive function that takes the input array ,the current subset,the current index and  a list of list/ vector of vectors to contain the answer.
Try to make a subset of size n during the nth recursion call and consider elements from every index while generating the combinations. Only pick up elements that are appearing for the first time during a recursion call to avoid duplicates.
Once an element is picked up, move to the next index.The recursion will terminate when the end of array is reached.While returning backtrack by removing the last element that was inserted.

## Code -> 
```cpp
class Solution {
public:
    void helper(vector<int> nums, int idx, vector<int>temp, vector<vector<int>>& ans){
        // No explicit base condition because we want to generate subsets for all indices.
        ans.push_back(temp);

        for(int i=idx; i<nums.size(); i++){
            if(i>idx && nums[i]==nums[i-1]) continue;
            temp.push_back(nums[i]);
            helper(nums, i+1, temp, ans);
            temp.pop_back();
        }
    }
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(), nums.end()); // very important to sort since we are ignoring duplicate at a given position
        vector<int> temp;
        vector<vector<int>> ans;
        helper(nums, 0, temp, ans);
        return ans;
    }
};
```
Time Complexity: O(2^n) for generating every subset and O(k)  to insert every subset in another data structure if the average length of every subset is k. Overall O(k * 2^n).

Space Complexity: O(2^n * k) to store every subset of average length k. Auxiliary space is O(n)  if n is the depth of the recursion tree.


# [46. Permutations](https://leetcode.com/problems/permutations/description/)

## Approaches ->
1. Frequency array approach ->
- Create a frequency vector freq to keep track of whether an element has been used in the current permutation.

- If the size of the temporary vector temp becomes equal to the size of the input array nums, it means a valid permutation is found. Add it to the result vector ans.

- Iterate through each element in the nums array.
- If the element has not been used (frequency is 0), add it to the temporary vector temp.
- Mark the element as used by updating its frequency in the freq vector to 1.
- Recursively call the helper function with the updated temp and freq.
- After the recursive call, backtrack by marking the element as unused and removing it from temp.

Time Complexity:

The time complexity of the provided approach is O(N!), where N is the number of distinct integers in the input array nums. This is because, in each recursive call, the algorithm explores all possible choices for the next element, and there are N elements in the array. The recursive function is called N times for the first element, (N-1) times for the second element, (N-2) times for the third element, and so on, resulting in a total of N * (N-1) * (N-2) * ... * 1 = N! recursive calls.

Space Complexity:

The space complexity is O(N) for the recursion stack and O(N) for the temporary vectors used in each recursive call. The recursion stack depth is at most N, as the algorithm explores each element in the array. Additionally, each recursive call creates a temporary vector temp of size N. Therefore, the overall space complexity is O(N + N) = O(N).

## Code ->
```cpp
class Solution {
public:
    void helper(vector<int> nums, vector<int> temp, vector<vector<int>> &ans, vector<int> freq){
        if(temp.size()==nums.size()){
            ans.push_back(temp);
            return;
        }

        for(int i=0; i<nums.size(); i++){
            if(!freq[i]){
                temp.push_back(nums[i]);
                freq[i]=1;
                helper(nums, temp, ans, freq);
                freq[i]=0;
                temp.pop_back();
            }
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        vector<int> temp;
        vector<vector<int>> ans;
        vector<int> freq(nums.size(),0);
        helper(nums, temp, ans, freq);
        return ans;
    }
};
```