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

## Code ->
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int lo = 0, hi = nums.size()-1, mid;

        while(lo <= hi){
            mid = (lo+hi)/2;
            if(nums[mid]==target) return mid; // if target found, return index

            if(nums[mid]>=nums[lo]){ // identified that left side is sorted
                if(nums[mid]>target && nums[lo]<=target) // if the target is between the sorted side
                    hi = mid-1;
                else lo = mid+1; // not between the sorted side
            }
            else{   // identifed that right side is sorted
                if(nums[mid]<target && nums[hi]>=target) // if the target is between the sorted side
                    lo = mid+1;
                else hi = mid-1; // not between the sorted side
            }
        }

        return -1;
    }
};
```
# [162. Find Peak Element](https://leetcode.com/problems/find-peak-element/description/)

## Approach ->
First let us understand the question. What is a peak element?

A peak element in an array refers to the element that is greater than both of its neighbors. Basically, if arr[i] is the peak element, arr[i] > arr[i-1] and arr[i] > arr[i+1].

The element outside the bounds is minus infinity so it will always be smaller than the 0th index and last index element.

The approach is fairly simple, since we want to find the element that is greater than its neighbour element so we will be on a persuit of a greater element always. Hence always eliminate the half that has the smaller neighbour element. Look at the code and you would understand. Remember to handle the edge cases in the beginning and not inside the while loop to avoid long code.

## Code ->
```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int low=1, hi=nums.size()-2, mid, n=nums.size();
        if(n==1) return 0; // if size of nums is 1 return 0
        if(nums[0]>nums[1]) return 0; //if first element is peak return 0
        if(nums[n-1]>nums[n-2]) return n-1; // if last element is peak 

        while(low<=hi){
            mid = (low+hi)/2;

            if(nums[mid]>nums[mid-1] && nums[mid]>nums[mid+1])
                return mid; // if you find peak, return index
            if(nums[mid+1]>nums[mid-1])
                low = mid+1; // if the element to the right of index is greater than the element to the left of index then eliminate the left side completely 
            else hi = mid-1; // else eliminate the right side
        }
        return -1;
    }
};
```

# [540. Single Element in a Sorted Array](https://leetcode.com/problems/single-element-in-a-sorted-array/description/)

## Approach->
You already know the linear search approaches like hashing and bitwise xor but we have to do it in log n time complexity.

Binary search approach:
Let us observe something here and then the code will be a breeze to write. If you observe the indexing here then its something like this -> if an element appears twice in a sequence then its first occurence should be even and second should be odd (normal occurence). For example in `nums = [1,1,2,3,3,4,4,8,8]` -> first 1 has 0 index but second has index as 1 (even, odd). But if the occurences are in reverse i.e. first occurence odd and second even then that means that a single element before the appearance of the two elements has disrupted the index position. For example in above example -> first 3 has index 3 and second 3 has index 4 (odd, even).

Thus we can conclude that if the occurence case is normal (even, odd) for element at mid then we have to eliminate the left half because our single element appears somewhere on the right side. But if the occurence case is (odd, even) then we will have to eliminate the right half.

## Code ->
```cpp
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {
         int low = 1, hi = nums.size()-2, mid, n=nums.size();
         if(n==1) return nums[0];
         if(nums[0]!=nums[1]) return nums[0];
         if(nums[n-1]!=nums[n-2]) return nums[n-1]; // check both base conditions at first to avoid long code inside while loop and it gets confusing too if you check base cases with so many if else inside while loop.

         while(low<=hi){
             mid = (low+hi)/2;

             if(nums[mid]!=nums[mid-1] && nums[mid]!=nums[mid+1]) return nums[mid];
             
             // by observation we know that if an element appears twice in a sequence then its first occurence should be even and second should be odd (normal occurence). For example in example 1 -> first 1 has 0 index but second has index as 1. But if the occurences are in reverse i.e. first occurence odd and second even then that means that a single element before the appearance of the two elements has disrupted the index position.

             if(nums[mid]==nums[mid-1]){ // finding if the element before mid is equal to element at mid
                 if(mid%2==1) low = mid+1; // if normal occurence then eliminate left side
                 else hi = mid-1; // eliminate right side
             }
             else{
                 if(mid%2==1) hi = mid-1;
                 else low = mid+1;
             }
         }    

         return -1; 
    }
};
```