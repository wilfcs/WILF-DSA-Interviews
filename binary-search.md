# Implement Lower bound
## Question -> 
The lower bound algorithm finds the first or the smallest index in a sorted array where the value at that index is greater than or equal to a given key i.e. x.

The lower bound is the smallest index, ind, where arr[ind] >= x. But if any such index is not found, the lower bound algorithm returns n i.e. size of the given array.

Example-> arr[] = {1, 2, 4, 4, 4, 5, 9, 9}, x=4.

Output: 2 (as lower bound of 4 is at index 2. whereas upper bound of 4 is at index 5 as till index 4 there was 4 but at index 5 there is something greater than 4 (this will depend according to the question, don't worry about it so much))

---
## Approaches ->
- Naive approach (Using linear search)
- Optimal Approach (Using Binary Search): 
1. Place the 2 pointers i.e. low and high: Initially, we will place the pointers like this: low will point to the first index, and high will point to the last index.
2. Calculate the ‘mid’: Now, we will calculate the value of mid using the following formula:
`mid = (low+high) / 2`
3. Compare arr[mid] with x: With comparing arr[mid] to x, we can observe 2 different cases:
-  Case 1 – If arr[mid] >= x: This condition means that the index mid may be an answer. So, we will update the ‘ans’ variable with mid and search in the left half if there is any smaller index that satisfies the same condition. Here, we are eliminating the right half.
-  Case 2 – If arr[mid] < x: In this case, mid cannot be our answer and we need to find some bigger element. So, we will eliminate the left half and search in the right half for the answer.

## Code ->
```cpp
int lowerBound(vector<int> arr, int n, int x) {
    int low = 0, high = n - 1;
    int ans = n;

    while (low <= high) {
        int mid = (low + high) / 2;
        // maybe an answer
        if (arr[mid] >= x) {
            ans = mid;
            //look for smaller index on the left
            high = mid - 1;
        }
        else {
            low = mid + 1; // look on the right
        }
    }
    return ans;
}
```
- There's another way of doing this and that is using STL in cpp. 
- `lb = lower_bound(arr.begin(), arr.end(), x) - arr.begin();`
- This will return the index where there is lb.
- Here, x is the element we are searching the lb for. lower_bound(arr.begin(), arr.end(), x) will return an iterator hence we are subtracting it with arr.begin() to get the index value. So while solving problems you will most likely be using stl so remember it.

# Upper bound
Example -> arr[] = {1, 2, 4, 4, 4, 5, 9, 9}, x=4.

Output -> 5 (as at index 5 there is something greater than 4)

## Code ->
```cpp
int lowerBound(vector<int> arr, int n, int x) {
    int low = 0, high = n - 1;
    int ans = n;

    while (low <= high) {
        int mid = (low + high) / 2;
        // maybe an answer
        if (arr[mid] > x) { // Only this changed, rest everything is same as the code of lower bound
            ans = mid;
            //look for smaller index on the left
            high = mid - 1;
        }
        else {
            low = mid + 1; // look on the right
        }
    }
    return ans;
}
```

- STL: `ub = upper_bound(arr.begin(), arr.end(), x) - arr.begin();`

# [34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/description/)

## Approach ->
- This is a slight variation of the lower bound and upper bound problem. Here the approach is fairly simple, if we need to find the first positon of an element then use normal binary search but when you find the element we acutally need to update the answer but also check on the left side of the found position. So we will find the left side and in case of the last position when we find the element at any position we will only check on its right.

## Code ->
```cpp
class Solution {
public:
    int lowerBound(vector<int> nums, int target){
        int lb = -1, lo=0, hi=nums.size()-1, mid; // Initilize lb with -1 because if there is no ans then -1 will be returned.
         while(lo<=hi){
            mid = (lo+hi)/2;
            if(nums[mid]==target){ // if target is found
                lb = mid; // update the lb to be returned 
                hi = mid-1; // search on the left
            }
            else if(nums[mid]>target)
                hi = mid-1;
            else 
                lo = mid+1;
        }
        return lb;
    }
    int upperBound(vector<int> nums, int target){
        int ub = -1, lo=0, hi=nums.size()-1, mid; // Initilize ub with -1 because if there is no ans then -1 will be returned.
         while(lo<=hi){
            mid = (lo+hi)/2;
            if(nums[mid]==target){ // if target is found
                ub = mid; // update the lb to be returned 
                lo = mid+1; // search on the right
            }
            else if(nums[mid]>target)
                hi = mid-1;
            else 
                lo = mid+1;
        }
        return ub;
    }

    vector<int> searchRange(vector<int>& nums, int target) {
        vector<int> ans;
        ans.push_back(lowerBound(nums, target));
        ans.push_back(upperBound(nums, targt));

        return ans;
    }
};
```

# [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/description/)

## Approach ->
The approach is fairly simple once you actually understand it. To utilize the binary search algorithm effectively, it is crucial to ensure that the input array is sorted. By having a sorted array, we guarantee that each index divides the array into two sorted halves. In the search process, we compare the target value with the middle element, i.e. arr[mid], and then eliminate either the left or right half accordingly. This elimination becomes feasible due to the inherent property of the sorted halves(i.e. Both halves always remain sorted).

However, in this case, the array is both rotated and sorted. As a result, the property of having sorted halves no longer holds. This disruption in the sorting order affects the elimination process, making it unreliable to determine the target’s location by solely comparing it with arr[mid]. 

Key Observation: Though the array is rotated, we can clearly notice that for every index, one of the 2 halves will always be sorted.

So, to efficiently search for a target value using this observation, we will follow a simple two-step process:

- First, we identify the sorted half of the array. 
- Once found, we determine if the target is located within the bounds of this sorted half. 
-  If not, we eliminate that half from further consideration. 
-  Conversely, if the target does exist in the sorted half, we eliminate the other half.

It is necessary to first check which half is sorted so we can be completely sure that whether an element is falling within that half or not to make an informative decision whether to element that half or not.

