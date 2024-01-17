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
bool helper(int n, int k, vector<int> &arr, int sum){
    if(sum==k) return true;
    if(n<0) return false;
    return (helper(n-1, k, arr, sum+arr[n]) || helper(n-1, k, arr, sum));   
}
bool subsetSumToK(int n, int k, vector<int> &arr) {
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
# Intuition:
- The problem involves finding all unique combinations of candidates that sum to the target.
- The key challenge is to avoid duplicate combinations in the result.
- A recursive backtracking approach is used to explore and generate combinations.
# Approach:
- Sort the Candidates: Before starting the recursive calls, sort the candidates. Sorting is crucial to avoid duplicate combinations and to make the combinations appear in a sorted order.

- Define Recursive Helper Function: The helper function is a recursive function that explores combinations.
It takes parameters: the current candidates array, the remaining target, the current temporary combination (temp), the current sum, and the current index (idx).

- Base Cases: If the current sum exceeds the target, return. If the current sum equals the target, add the current combination to the result (ans) and return.

- Loop Through Candidates: Use a loop starting from the current index (idx) to the end of the candidates array. Skip consecutive duplicates to avoid duplicate combinations. If the current candidate is greater than the remaining target, break the loop (optimization). Include the current candidate in the temporary combination. Recursively call the helper function with updated parameters. Remove the last added element to backtrack and explore other combinations.

- Recursive Calls: The recursive calls explore combinations with the current element included (temp.push_back) and without it (temp.pop_back).

- Main Function: The combinationSum2 function initializes an empty vector (temp) and calls the helper function with the starting parameters. The sorted candidates array is used, and the result is returned.

Extra Note: In this question we are required to ignore duplicates and return the answer in sorted order.
So an approach might come in your head that why not keep the code same as of the last question and just increase the index even when we pick the element. Well that would have worked if there was no condition saying "find all unique combinations in candidates where the candidate numbers sum to target". But since we also have to keep the ans with unique elements only we will have to apply a different method of doing so.

Here's an example of understanding what is the wrong output and what is the right output:

Wrong output: [[1,2,5],[1,7],[1,6,1],[2,6],[2,1,5],[7,1]]
Right output: [[1,2,5],[1,7],[1,6,1],[2,6]]

---
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

            // helper(candidates, target, temp, sum, i+1); -> we will not include this line like the previous solution. reason given below.
        }
        

        
    }

    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end()); // sort the array to avoid duplicates in the future
        vector<int> temp;
        helper(candidates, target, temp, 0, 0);
        return ans;
    }
};
```
Question -> Why did we not include helper(candidates, target, temp, sum, i+1);

Reason ->  The specific line helper(candidates, target, temp, sum, i+1); (which represents the recursive call without including the current candidate) is omitted in this particular implementation because of the nature of the problem and the way duplicates are handled.

The condition if(i > idx && candidates[i] == candidates[i-1]) continue; takes care of skipping over duplicate elements at the current level of recursion. In other words, when you encounter a duplicate, you skip the recursive call for that duplicate element, ensuring that you don't generate duplicate combinations.

If you were to include the line helper(candidates, target, temp, sum, i+1);, it would mean that you are considering the case where you skip the current element and move to the next one. However, the way duplicates are handled in the current code essentially achieves the same effect without explicitly making that recursive call.

# Time Complexity:O(2^n*k)

Reason: Assume if all the elements in the array are unique then the no. of subsequence you will get will be O(2^n). we also add the ds to our ans when we reach the base case that will take “k”//average space for the ds.

# Space Complexity:O(k*x)

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

2. Swapping to generate all permutations Approach ->
[Look at the recursive tree without fail](https://www.youtube.com/watch?v=f2ic2Rsc9pU&t=26s)

- If the current index idx reaches the size of the array, it means a valid permutation is found. Add it to the result vector ans.

- Iterate through each element in the array starting from the current index idx.
- Swap the current element with the element at index idx.
- Recursively call the helper function with the updated array and the next index (idx+1).
- After the recursive call, backtrack by swapping the elements back to their original positions.

- After the recursion, the ans vector contains all the valid permutations, and it is returned.

The time complexity is O(N!), where N is the number of distinct integers in the input array nums. The space complexity is O(N) for the recursion stack, as the recursion explores each element in the array. The space complexity is optimized compared to the previous approach as it doesn't use additional temporary vectors in each recursive call.

## Code ->
```cpp
class Solution {
public:
    void helper(vector<int> nums, vector<vector<int>> &ans, int idx){
        if(idx==nums.size()){
            ans.push_back(nums);
            return;
        }

        for(int i=idx; i<nums.size(); i++){
            swap(nums[idx], nums[i]);
            helper(nums, ans, idx+1);
            swap(nums[idx], nums[i]);
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> ans;
        helper(nums, ans, 0);
        return ans;
    }
};
```

# [22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/description/)
## Approach ->
The helper function is called recursively with three parameters: the remaining counts of open parentheses (open), close parentheses (close), and the current combination string (str). The base case is when both open and close counts are zero, at which point the current combination is added to the result vector (ans).

The recursion explores two possibilities at each step:

If there are remaining open parentheses (open > 0), it appends an open parenthesis to the current combination and calls the helper function with reduced open count.
If there are remaining close parentheses (close > 0 and there are more open parentheses than closed ones), it appends a close parenthesis to the current combination and calls the helper function with reduced close count.

## Code ->
```cpp
class Solution {
public:
    void helper(int n, int open, int close, vector<string> &ans, string str){
        if(open==0 && close==0){
            ans.push_back(str);
            return;
        }
        if(open){
            helper(n, open-1, close, ans, str+'(');
        } 
        
        if(close && open<close){
            helper(n, open, close-1, ans, str+')');
        }
    }
    vector<string> generateParenthesis(int n) {
        vector<string> ans;
        helper(n, n, n, ans, "");
        return ans;
    }
};
```
# [51. N-Queens](https://leetcode.com/problems/n-queens/description/)
## [Approaches](https://takeuforward.org/data-structure/n-queen-problem-return-all-distinct-solutions-to-the-n-queens-puzzle/)

---
## Codes ->
Code 1
```cpp
class Solution {
public:
    bool isValid(int row, int col, int n, vector<string>board){
        // checking left
        int r = row;
        int c = col;
        while(c>=0){
            if(board[r][c]=='Q') return false;
            c--;
        }

        // checking down-left diagonal
        r = row;  
        c = col;
        while(r<n && c>=0){
            if(board[r][c]=='Q') return false;
            r++;
            c--;
        }
        
        // checking up-left diagonal
        r = row;
        c = col;
        while(r>=0 && c>=0){
            if(board[r][c]=='Q') return false;
            r--;
            c--;
        }
        return true;
    }
    void helper(int col, vector<string> &board, vector<vector<string>> &ans, int n){
        if(col == n){
            ans.push_back(board);
            return;
        }

        for(int row=0; row<n; row++){
            if(isValid(row, col, n, board)){
                board[row][col]='Q';
                helper(col+1, board, ans, n);
                board[row][col]='.';
            }
        }
    }
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> ans;
        vector<string> board(n);
        string s (n, '.');
        for(int i=0; i<n; i++) board[i]=s;
        helper(0, board, ans, n);
        return ans;
    }
};
```

Code 2
```cpp
class Solution {
public:
    void helper(int col, vector<string> &board, vector<vector<string>> &ans, int n, vector<int> &leftrow, vector<int> &upperDiagonal, vector<int> &lowerDiagonal){
        if(col == n){
            ans.push_back(board);
            return;
        }

        for(int row=0; row<n; row++){
            if(leftrow[row]==0 && upperDiagonal[n-1+col-row]==0 && lowerDiagonal[row+col]==0){
                board[row][col]='Q';
                leftrow[row]=1;
                upperDiagonal[n-1+col-row]=1;
                lowerDiagonal[row+col]=1;
                helper(col+1, board, ans, n, leftrow, upperDiagonal, lowerDiagonal);
                board[row][col]='.'; // backtract to reset
                leftrow[row]=0;
                upperDiagonal[n-1+col-row]=0;
                lowerDiagonal[row+col]=0;
            }
        }
    }
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> ans;
        vector<string> board(n);
        string s (n, '.');
        for(int i=0; i<n; i++) board[i]=s;
        vector<int>leftrow(n, 0);
        vector<int> upperDiagonal(2*n-1, 0);
        vector<int> lowerDiagonal(2*n-1, 0);
        helper(0, board, ans, n, leftrow, upperDiagonal, lowerDiagonal);
        return ans;
    }
};
```

# [131. Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)

## [Approach](https://takeuforward.org/data-structure/palindrome-partitioning/)
Note that here we need to partition the string, so this is how we partition. Remember this pattern for further partitioning questions.

## Code ->
```cpp
class Solution {
public:
    bool isPalindrome(int idx, int i, string s){
        while(idx<i){
            if(s[idx++] != s[i--]) return false;
        }
        return true;
    }
    void helper(string s, vector<vector<string>> &ans, vector<string> &temp, int idx){
        if(s.size()==idx){
            ans.push_back(temp);
            return;
        }

        for(int i=idx; i<s.size(); i++){
            if(isPalindrome(idx, i, s)){ // if the substring from start to end is palindrome
                temp.push_back(s.substr(idx, i-idx+1)); // push substring start to end in temp
                helper(s, ans, temp, i+1); // call recursion to find other partitions
                temp.pop_back(); // reset temp
            }
        }
        
    }
    vector<vector<string>> partition(string s) {
        vector<vector<string>> ans;
        vector<string> temp;
        helper(s, ans, temp, 0);
        return ans;
    }
};
```

# [17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)

## Approach ->
Make a mapping for all the numbers. Extract the alphabets from given digit one by one and for every letter for a given digit, call the recursive function.
[Recursive tree](https://www.youtube.com/watch?v=vgnhZzw-kfU)

## Code ->
```cpp
class Solution {
public:
    void helper(string digits, vector<string> digitMapping, string temp, int idx, vector<string> &ans){
        if(idx==digits.size()){
            ans.push_back(temp);
            return;
        }

        string curStr = digitMapping[digits[idx]-'0']; // extract the alphabets from given digit to curStr. Also don't forget to subtract digits[idx] with '0' because it is not a number it is a character.

        for(int i=0; i<curStr.size(); i++)
            helper(digits, digitMapping, temp+curStr[i], idx+1, ans); // recursive call for next index and temp+curStr[i]
    }
    vector<string> letterCombinations(string digits) {
        vector<string> ans; // declare answer to return
        if(digits.size()==0) return ans; // important base condition
        vector<string> digitMapping = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"}; // declaring a string mapping of size 10
        helper(digits, digitMapping, "", 0, ans);
        return ans;
    }
};
```
# [216. Combination Sum III](https://leetcode.com/problems/combination-sum-iii/description/)

## Approach ->
Generate all combinations of length k from the set {1, 2, ..., 9}, and filter those combinations where the sum equals the target n.

---
## Code ->
```cpp
#include <vector>

class Solution {
public:
    // Helper function to find combinations
    void helper(int k, int n, vector<vector<int>> &ans, vector<int> temp, int sum, vector<int> &hash, int idx) {
        // if the sum exceeds the target or the size of the combination is reached
        if (sum > n) return;
        if (temp.size() == k) {
            // If the combination size is k and the sum is n, add it to the answer
            if (sum == n)
                ans.push_back(temp);
            return;
        }

        // Iterate through numbers starting from idx
        for (int i = idx; i <= 9; i++) {
            // If the number is not used in the current combination
            if (hash[i] == 0) {
                // Add the number to the combination
                temp.push_back(i);
                hash[i] = 1; // Mark the number as used
                // Recursively call the helper function with updated parameters
                helper(k, n, ans, temp, sum + i, hash, i + 1);
                temp.pop_back(); // Backtrack: remove the last added number
                hash[i] = 0;    // Mark the number as unused for the next iteration
            }
        }
    }

    vector<vector<int>> combinationSum3(int k, int n) {
        vector<vector<int>> ans;
        vector<int> temp;
        vector<int> hash(10, 0);
        helper(k, n, ans, temp, 0, hash, 1);
        return ans;
    }
}
```

# [79. Word Search](https://leetcode.com/problems/word-search/description/)

## Approach ->
Approach:  We are going to solve this by using backtracking, in this approach first we will linearly search the entire matrix to find the first letters matching our given string. If we found those letters then we can start backtracking in all four directions to find the rest of the letters of the given string.

Step 1: Find the first character of the given string.

Step 2: Start Backtracking in all four directions until we find all the letters of sequentially adjacent cells.

Step 3: At the end, If we found our result then return true else return false.

Edge cases: Now think about what will be our stopping condition, we can stop or return false if we reach the end of the boundaries of the matrix or the letter at which we are making recursive calls is not the required letter.

We will also return if we found all the letters of the given word i.e. we found the number of letters equal to the length of the given word.

NOTE: Do not forget that we cannot reuse a cell again.

That is, we have to somehow keep track of our position so that we cannot find the same letter again and again.

In this approach, we are going to mark visited cells with some random character that will prevent us from revisiting them again and again.

---
## Code ->
```cpp
class Solution {
public:
    bool helper(vector<vector<char>> &board, string word, int row, int col, int idx){
        // if index reaches at the end that means we have found the word
        if(idx == word.size()) return true;
        // Checking the boundaries if the position at which we are placed is not out of bounds
        // checking if the letter is same as of letter required or not
        if(row<0 || col<0 || row>=board.size() || col>=board[0].size() || board[row][col]!=word[idx]) return false;

        // this is to prevent reusing of the same character
        char prevWord = board[row][col];
        board[row][col] = '#';

        // check all directions
        bool op1 = helper(board, word, row+1, col, idx+1);
        bool op2 = helper(board, word, row, col+1, idx+1);
        bool op3 = helper(board, word, row-1, col, idx+1);
        bool op4 = helper(board, word, row, col-1, idx+1);

        // undo change
        board[row][col] = prevWord;

        return (op1 || op2 || op3 || op4);
    }
    bool exist(vector<vector<char>>& board, string word) {
        // First search the first character to start recursion process
        for(int i=0; i<board.size(); i++){
            for(int j=0; j<board[0].size(); j++){
                if (board[i][j] == word[0]){
                    if(helper(board, word, i, j, 0)) return true;
                }      
            }
        }
        return false;
    }
};
```

# [Rat in a Maze Problem - I](https://www.geeksforgeeks.org/problems/rat-in-a-maze-problem/1)

## Approach ->
It is failry simple. ATQ the answer should be in lexicographically sorted order, so we will keep that in mind. We also cannot visit a path that has 0 on it, so we will have to keep that in mind too. Keeping these two things in mind we make our recursive calls.

## Code ->
```cpp
class Solution{
    public:
    void helper(vector<vector<int>>&m, int n, vector<string> &ans, string s, int row, int col){
        // If the destination is reached, add the current path to the result
        if(row==n-1 && col==n-1){
            ans.push_back(s);
            return;
        }
        
        // Check for out-of-bounds or obstacles(0 on any index) in the matrix
        if(row<0 || col<0 || row>=n || col>=n || m[row][col]!=1) return;
        
        // Mark the current cell as visited
        m[row][col]=2;
        // Recursion call in all four directions in lexicographical order
        helper(m, n, ans, s+'D', row+1, col);
        helper(m, n, ans, s+'L', row, col-1);
        helper(m, n, ans, s+'R', row, col+1);
        helper(m, n, ans, s+'U', row-1, col);
        // Backtrack: Mark the current cell as unvisited
        m[row][col]=1;
    }
    vector<string> findPath(vector<vector<int>> &m, int n) {
        vector<string> ans;
        // Important base condition
        if (m[0][0] == 0 || m[n - 1][n - 1] == 0) {
            // If the source or destination is blocked, return -1
            ans.push_back("-1");
            return ans;
        }
        
        helper(m, n, ans, "", 0, 0);
        return ans; 
    }
};
```

# [37. Sudoku Solver](https://leetcode.com/problems/sudoku-solver/description/)

## [Approach](https://takeuforward.org/data-structure/sudoku-solver/)

## Code ->
```cpp
class Solution {
public:
    // Function to solve Sudoku puzzle
    void solveSudoku(vector<vector<char>>& board) {
        // Call helper function to start solving
        helper(board);
    }

    // Recursive helper function to solve Sudoku
    bool helper(vector<vector<char>> & board) {
        // Loop through each cell in the board
        for(int i = 0; i < board.size(); i++) {
            for(int j = 0; j < board[0].size(); j++) {
                // Check if the cell is empty (indicated by '.')
                if(board[i][j] == '.') {
                    // Try filling the empty cell with digits 1 to 9
                    for(char x = '1'; x <= '9'; x++) {
                        // Check if placing x in the current cell is valid
                        if(isValid(i, j, board, x)) {
                            // Place x in the cell
                            board[i][j] = x;

                            // Recursively call the helper function
                            if(helper(board)) return true; // Continue with the next cell

                            // If the placement doesn't lead to a solution, backtrack and try a different digit
                            else board[i][j] = '.';
                        }
                    }
                    // If no valid digit can be placed, backtrack to the previous cell
                    return false;
                }
            }
        }
        // If all cells are filled, the Sudoku is solved
        return true;
    }

    // Function to check if placing x in the given cell is valid
    bool isValid(int row, int col, vector<vector<char>> & board, char x) {
        // Check if x is not present in the current row and column
        for(int i = 0; i < 9; i++) {
            if(board[row][i] == x) return false; // Check row
            if(board[i][col] == x) return false; // Check column

            // Check the 3x3 sub-box
            if(board[3 * (row / 3) + i / 3][3 * (col / 3) + i % 3] == x) return false;
        }
        // If x can be placed in the cell without violating Sudoku rules, it is valid
        return true;
    }
};
```

# [1849. Splitting a String Into Descending Consecutive Values](https://leetcode.com/problems/splitting-a-string-into-descending-consecutive-values/description/)

## Approach ->
As we are required to split the string in such a way that all the splits should have numbers in a decreasing manner (just by 1) so in the main function we can iterate through the entire string and call a recursive function helper for the first split. Inside the helper function, we check if the current substring, formed by converting a portion of the input string to a numerical value, is one less than the previous value. If this condition is met, the function is recursively called on the remaining portion of the string, exploring all possible valid splits.

---
## Code ->
```cpp
class Solution {
public:
    bool splitString(string s) {
        unsigned long long int val = 0;

        // Iterate through the string (excluding the last character)
        for(int i = 0; i < s.size() - 1; i++) {
            // Build val
            val = 10 * val + (s[i] - '0');

            // Check if splitting is possible using the helper function
            if(helper(s, val, i + 1)) {
                return true;  // If possible, return true
            }
        }

        return false; // Else return false
    }

    bool helper(string s, unsigned long long int val, int idx) {
        if(s.size() == idx) {
            return true;  // If the entire string has been processed, return true
        }

        unsigned long long int curVal = 0;

        // Iterate through the remaining characters of the string i.e. from idx
        for(int i = idx; i < s.size(); i++) {
            // Build the current numerical value
            curVal = 10 * curVal + (s[i] - '0');

            // Check if the difference between val and curVal is 1
            // If true, then and only then recursively check the remaining string else just iterate in the loop (that's why we used && operator so that if val - curVal == 1 then and only then call the recursion to make further splits in the string.)
            if(val - curVal == 1 && helper(s, curVal, i + 1)) {
                return true;  // If a valid split is found, return true
            }
        } 

        return false; // If no valid split is found, return false
    }
};
```

# [77. Combinations](https://leetcode.com/problems/combinations/description/)

## Code ->
```cpp
// figure out on yourself first kiddo, you've done these kinds of patterns.
class Solution {
public:
    void helper(int n, int k, vector<vector<int>> &ans, vector<int> temp, int idx){
        if(temp.size()==k){
            ans.push_back(temp);
            return;
        }

        for(int i=idx; i<=n; i++){
            temp.push_back(i);
            helper(n, k, ans, temp, i+1);
            temp.pop_back();
        }
        return;
    }
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> ans;
        vector<int> temp;
        helper(n, k, ans, temp, 1);
        return ans;
    }
};
```
# [1239. Maximum Length of a Concatenated String with Unique Characters](https://leetcode.com/problems/maximum-length-of-a-concatenated-string-with-unique-characters/description/)

## Code ->
```cpp
class Solution {
public:
    // Helper function to check if a string has unique characters
    bool isValid(string curStr, vector<int> &selected) {
        // Self-check for unique characters in the string
        vector<int> selfCheck(26, 0);
        for (int i = 0; i < curStr.size(); i++) {
            if (selfCheck[curStr[i] - 'a'] == 1) return false;
            selfCheck[curStr[i] - 'a'] = 1;
        }

        // Check if the characters in the string are already selected
        for (int i = 0; i < curStr.size(); i++) {
            if (selected[curStr[i] - 'a'] == 1) return false;
        }

        return true;
    }

    // Recursive helper function to explore all possible combinations
    int helper(vector<string> &arr, int &ans, int idx, vector<int> &selected) {
        // If we reach the end of the array, return the current answer
        if (idx == arr.size()) {
            return ans;
        }

        // Get the current string
        string curStr = arr[idx];

        // If the current string is not valid, move to the next index
        if (isValid(curStr, selected) == false) {
            return helper(arr, ans, idx + 1, selected);
        } else {
            // Mark characters in the current string as selected
            for (int i = 0; i < curStr.size(); i++) selected[curStr[i] - 'a'] = 1;
            // Update the answer by adding the length of the current string
            ans += curStr.size();
            // Recursively explore with the current string selected
            int op1 = helper(arr, ans, idx + 1, selected);

            // Backtrack: Mark characters in the current string as not selected
            for (int i = 0; i < curStr.size(); i++) selected[curStr[i] - 'a'] = 0;
            // Backtrack: Subtract the length of the current string from the answer
            ans -= curStr.size();
            // Recursively explore without selecting the current string
            int op2 = helper(arr, ans, idx + 1, selected);

            // Return the maximum of the two options
            return max(op1, op2);
        }
    }

    // Main function to find the maximum length of a concatenated string with unique characters
    int maxLength(vector<string>& arr) {
        int ans = 0;
        vector<int> selected(26, 0);
        return helper(arr, ans, 0, selected);
    }
};
```

# [93. Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/description/)

## Code ->
```cpp
class Solution {
public:
void helper(string& s, int i, int dots, string res, vector<string>& ans) {
        // Base case: If four parts are formed, and the entire string is processed
        if (dots == 4 && i == s.length()) {
            // Add the valid IP address to the result
            ans.push_back(res.substr(0, res.length() - 1)); // Remove the trailing dot
            return;
        }

        // If more than 4 parts are formed, or if the string is not fully processed, return
        if (dots > 4) {
            return;
        }

        // Explore all possible parts by considering substrings of length 1 to 3
        for (int j = i; j < min(i + 3, static_cast<int>(s.length())); ++j) {
            // Convert the substring to an integer
            int currnum = stoi(s.substr(i, j - i + 1));

            // Check if the part is less than or equal to 255 and does not have leading zeros
            if (currnum <= 255 && (i == j || s[i] != '0')) {
                // Recursively call the helper function with the updated parameters
                helper(s, j + 1, dots + 1, res + s.substr(i, j - i + 1) + ".", ans);
            }
        }
    }

    vector<string> restoreIpAddresses(string s) {
        vector<string> ans;
        helper(s, 0, 0, "", ans);
        return ans;
    }

};
```

# [473. Matchsticks to Square](https://leetcode.com/problems/matchsticks-to-square/description/)
## Approaches ->
1. The code uses a recursive approach to check if it's possible to form a square using matchsticks, ensuring each matchstick is used exactly once. The helper function explores different combinations by distributing matchsticks among four sides, and the main function checks if the total sum is divisible by 4 before calling the helper function. If successful, it returns true; otherwise, it returns false. Look at code 1 for better understanding. Although this approach will throw a TLE, so we will be optimizing the further. Also keep note of the optimization 1 done in the code below.

2. Now to solve the TLE we need to further optimize the code. Second optimization that we can do is that we will take a target variable that will suggest the value that should be in each matchSides array. In optimization 2 we will simply not call the recursive function if the value in the matchSides (after summing the already existing value in matchSides with matchsticks[i]) comes greater than target value. Third optimization we can do is that we will sort matchsticks array in decreasing order before passing it, that way if there is an element that is way larger than target will be encountered at the beginning itself and there will be no further recursion calls. Fourth and last optimization can be that for a given number in matchsticks array, if adding that number to a position of matchSides(let's say 0th having integer 5) gave us a false result, then if we encounter the same number (5 again) on matchSides on some other position then there is no point in calling the recursion. So we will simply continue our loop without calling the recursion. Now have a look at the code for better understanding.

## Codes ->

### 1. 
```cpp
class Solution {
public:
    bool helper(vector<int> &matchsticks, vector<int> matchSides, int idx){
        // When all matchsticks are used, check if sides are equal
        if(idx==matchsticks.size()){
            if(matchSides[0]==matchSides[1] && matchSides[1]==matchSides[2] && matchSides[2]==matchSides[3]) return true;
            else return false;
        }

        // Try distributing the current matchstick among the four sides and recursively explore
        for(int i=0; i<4; i++){
            matchSides[i]+=matchsticks[idx];
            if(helper(matchsticks, matchSides, idx+1)) return true;
            matchSides[i]-=matchsticks[idx];
        }
        return false;
    }
    bool makesquare(vector<int>& matchsticks) {
        // Calculate the sum of matchstick lengths
        int sum = accumulate(matchsticks.begin(), matchsticks.end(), 0);
        // Check if the number of matchsticks is not zero and the sum is divisible by 4 (Optimization 1)
        if(matchsticks.size()==0 || sum%4!=0) return false;

        vector<int> matchSides(4, 0);
        return helper(matchsticks, matchSides, 0);
    }
};
```
### 2. 
```cpp
// Same code with a 4 optimizations->
class Solution {
public:
    bool helper(vector<int> &matchsticks, vector<int> matchSides, int idx, int target){
        if(idx==matchsticks.size()){
            if(matchSides[0]==matchSides[1] && matchSides[1]==matchSides[2] && matchSides[2]==matchSides[3]) return true;
            else return false;
        }

        for(int i=0; i<4; i++){
            if(matchSides[i] + matchsticks[idx] > target) continue; // Optimization 2

            // Optimization 4 starts here
            int j = i-1;
            while(j>=0){
                if(matchSides[j] == matchSides[i]) break;
                j--;
            }
            if(j!=-1) continue;
            // Optimization 4 ends here

            matchSides[i]+=matchsticks[idx];
            if(helper(matchsticks, matchSides, idx+1, target)) return true;
            matchSides[i]-=matchsticks[idx];
        }
        return false;
    }
    bool makesquare(vector<int>& matchsticks) {
        int sum = accumulate(matchsticks.begin(), matchsticks.end(), 0);
        if(matchsticks.size()==0 || sum%4!=0) return false; // Optimization 1

        int target = sum/4;

        vector<int> matchSides(4, 0);

        sort(matchsticks.begin(), matchsticks.end(), greater<int>()); // Optimization 3
        return helper(matchsticks, matchSides, 0, target);
    }
};
```

# [241. Different Ways to Add Parentheses](https://leetcode.com/problems/different-ways-to-add-parentheses/description/)

## Code ->
```cpp
class Solution {
public:
    vector<int> diffWaysToCompute(string expression) {
        vector<int> ans;
        int n = expression.size();

        // Iterate through each character in the expression
        for(int i = 0; i < n; i++) {
            char ch = expression[i];

            // If the current character is an operator
            if(ch == '+' || ch == '-' || ch == '*') {
                // Recursively compute the left and right parts of the expression
                vector<int> left = diffWaysToCompute(expression.substr(0, i));
                vector<int> right = diffWaysToCompute(expression.substr(i + 1, n));

                // Combine the results of left and right using the current operator
                for(int x : left) {
                    for(int y : right) {
                        if(ch == '-') ans.push_back(x - y);
                        else if(ch == '+') ans.push_back(x + y);
                        else ans.push_back(x * y);
                    }
                }
            }
        }

        // If the expression contains only a number (no operator)
        if(ans.empty()) ans.push_back(stoi(expression));

        return ans;
    }
};
```
## Approach->
The basic idea and the crux of this game is this->

1. When it is your turn, do your best
2. When it is your opponent's turn, expect the worst from result because your opponent is also playing optimally.

So for example you have {1,5,2} and if you pick 1 then your opponent will have {5,2} as option so they will pick 5 for sure and you'll be left with minimum (hence the expect the worst from result is true). And if you picked 2 from {1,5,2} then also your opponent would have chose 5.

So in this approach we are just going to find the most optimal total price of player 1 using recursion. Then we will subtract that total optimal price of player 1 with the total price available in the nums array to find the total price of player 2. And then we can return true if player 1 has more or equal than player 2.

So it is very clear that we will only find the optimal price of player 1. So player 1 has two options-> to pick the first element i.e. from st or to pick from the end i.e. en. If player 1 picks from st, player 2 can pick from either st or en. So player 1 in the next turn will have to pick from st+2,en or st+1,en-1. If player 1 picks from en, in the next turn player 1 will have to pick from st+1,en-1 or st+2,en.

So everytime we will choose both st and en prices and call recursion for the minimum value of recursion because the opponent is not letting us choose the maximum value (opponent's optimal moves that's why we are expecting the least (crux 2)). And at the end we will return the maximum value among the two (our optimal move because it's our chance to pick (crux 1))

## Code ->
```cpp
class Solution {
public:
    // Helper function to find the optimal total price for Player 1
    int helper(vector<int>& nums, int st, int en){
        // If st is greater than en, there are no elements left
        if(st > en) return 0;
        // If st is equal to en, there is only one element left
        if(st == en) return nums[st];

        // Option 1 for Player 1: Pick from the start and consider opponent's optimal moves i.e. anticipate minimum result from the future
        int op1ForPlayer = nums[st] + min(helper(nums, st + 2, en), helper(nums, st + 1, en - 1));
        // Option 2 for Player 1: Pick from the end and consider opponent's optimal moves i.e. anticipate minimum result from the future
        int op2ForPlayer = nums[en] + min(helper(nums, st, en - 2), helper(nums, st + 1, en - 1));

        // Return the maximum value among the two options for Player 1 because it's your turn and you will obviously do your best
        return max(op1ForPlayer, op2ForPlayer);
    }

    // Main function
    bool predictTheWinner(vector<int>& nums) {
        // Calculate the total sum of all elements in the nums array
        int total = accumulate(nums.begin(), nums.end(), 0);

        // Find the optimal total price for Player 1 using the helper function
        int p1 = helper(nums, 0, nums.size() - 1);

        // Calculate the total price for Player 2
        int p2 = total - p1;

        // Return true if Player 1's total price is greater than or equal to Player 2's total price
        return (p1 >= p2);
    }
};
```

# [Count Inversion](https://www.codingninjas.com/studio/problems/count-inversions_615?leftPanelTabValue=PROBLEM)

## [Approaches](https://takeuforward.org/data-structure/count-inversions-in-an-array/)

## Code->
```cpp
// The code from top to bottom is a merge sort code, just added one additional line to count answer.
#include <bits/stdc++.h> 
void merge(vector<long long> &ar, long long low, long long mid, long long high, long long &ans){
    long long left = low;
    long long right = mid+1;

    vector<int> vec;

    while(left <= mid && right<= high){
        if(ar[left]>ar[right]){
            ans += (mid - left + 1); // only additional line to count answer
            vec.push_back(ar[right++]);
        } 
        else vec.push_back(ar[left++]);
    }
    while(left<= mid) vec.push_back(ar[left++]);
    while(right<=high) vec.push_back(ar[right++]);

    int x = 0;
    for(int i=low; i<=high; i++) ar[i] = vec[x++];
}
void mergeSort(vector<long long> &ar, long long low, long long high, long long &ans){
    if(low >= high){
        return;
    }

    long long mid = (high + low) / 2;

    mergeSort(ar, low, mid, ans);
    mergeSort(ar, mid+1, high, ans);
    merge(ar, low, mid, high, ans);
}

long long getInversions(long long *arr, int n){
    vector<long long> ar;
    for(int i=0; i<n; i++) ar.push_back(arr[i]);
    long long ans = 0;
    mergeSort(ar, 0, n-1, ans);
    return ans;
}
```

TC ->  O(N*logN), SC-> O(N), as in the merge sort We use a temporary array to store elements in sorted order.

# [493. Reverse Pairs](https://leetcode.com/problems/reverse-pairs/description/)

## Approach -> Exactly same as the approach in the question above. Just make another function to calculate the answer and call the function before merging the two arrays(sorted). [link](https://takeuforward.org/data-structure/count-reverse-pairs/)
```cpp
class Solution {
public:
    void merge(vector<long long int> &ar, int low, int mid, int high, long long int &ans){
        int left = low;
        int right = mid+1;

        vector<int> vec;

        while(left <= mid && right<= high){
            if(ar[left]>ar[right]){
                vec.push_back(ar[right++]);
            } 
            else vec.push_back(ar[left++]);
        }
        while(left<= mid) vec.push_back(ar[left++]);
        while(right<=high) vec.push_back(ar[right++]);

        int x = 0;
        for(int i=low; i<=high; i++) ar[i] = vec[x++];
    }

    // Function to calculate the answer
    void calculate(vector<long long int> &ar, int low, int mid, int high, long long int &ans){
        int left = low, right = mid+1;
        while(left<=mid && right<=high){
            if( ar[left] <= (ar[right])*2 ) left++;
            else{
                ans+=(mid-left+1);
                right++;
            }
        }
    }

    void mergeSort(vector<long long int> &ar, int low, int high, long long int &ans){
        if(low >= high){
            return;
        }

        int mid = (high + low) / 2;

        mergeSort(ar, low, mid, ans);
        mergeSort(ar, mid+1, high, ans);
        calculate(ar, low, mid, high, ans); // calculate function call
        merge(ar, low, mid, high, ans);
    }

    int reversePairs(vector<int>& nums) {
        long long int ans = 0;
        vector<long long int> ar;
        for(int i=0; i<nums.size(); i++) ar.push_back(nums[i]);
        mergeSort(ar, 0, nums.size()-1, ans);
        return ans;
    }
};
```
TC-> O(2N*logN). SC-> O(N)


# [Count of Smaller Numbers After Self ](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

