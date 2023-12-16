# [1572. Matrix Diagonal Sum](https://leetcode.com/problems/matrix-diagonal-sum/)

## Approach->
- There is an observation that the the middle cell will only collide in case of the matrix size being in odd number. For example, if the matrix is 3x3 then the middle cell will be the same for both sides diagonal. But in the case of a 4x4 matrix that is not the case. So traverse the diagonals and keep calculating the sum. If the matrix is odd then subtract the middle element from the final answer.

## Code->
```cpp
int diagonalSum(vector<vector<int>>& mat) {
        int size = mat.size();
        int ans = 0;
        for(int i=0; i<size; i++){
            ans+=mat[i][I]; // calculating sum for diagonal left to right
            ans+=mat[i][size-i-1]; // for diagonal right to left
        }
        if(size%2){
            ans -= mat[size/2][size/2];
        }
        return ans;
    }
```

# [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)
## Approach->
- If you notice closely then at any position the amount of water that can be stored is the [min( greatest height at the left, greatest height at the right) - height at the current position]. So brute force would be to traverse at the left and right from  a position and find the greatest at the left and right. TC -> O(n^2).
- You can maintain a prefix and suffix sum using hashing. That way you can easily find the greatest on the left and right. TC-> O(n). SC->O(n).
```
int trap(vector<int>& height) {
        vector <int> prefix(height.size(), 0);
        vector <int> suffix(height.size(), 0);
        int premax = 0;

        // making a prefix array.
        for(int i=0; i<height.size(); i++){
            int curr = height[i];
            premax = max(premax, curr);
            prefix[i] = premax;
        }
        int sufmax = 0;

        // making a suffix array
        for(int i=height.size()-1; i>=0; i--){
            int curr = height[i];
            sufmax = max(sufmax, curr);
            suffix[i] = sufmax;
        }
        // finding ans
        int ans = 0;
        for(int i=0; i<height.size(); i++)
            ans += min(prefix[i], suffix[i]) - height[i];
        
        return ans;
    }
```
- The most optimal solution would be the two-pointer solution. So we will first discuss the solution and then discuss the intuition behind it. We will keep two pointers l and r on the left and right end of the array. Take two variables leftMax and rightMax and initialize them to 0. If height[l] is less than or equal to height[r] then if leftMax is less than height[l] update leftMax to height[l] else add leftMax-height[l] to your final answer and move the l pointer to the right i.e l++. If height[r] is less than height[l], then now we are dealing with the right block. If height[r] is greater than rightMax, then update rightMax to height[r] else add rightMax-height[r] to the final answer. Now move r to the left. Repeat these steps till l and r crosses each other. Now the intuition behind this approach is that we should subtract the current height with he min bw leftmax and rightmax height. So when we are selecting the position in the first if statement that we are dealing with we are making sure that it is the smaller height bw the left and the right pointer. This always makes sure that we are taking the smaller height bw the two heights. Look at the solution and think about it to understand properly
```
int trap(vector<int>& height) {
        int l = 0;
        int r = height.size()-1;
        int leftmax = 0;
        int rightmax = 0;
        int ans = 0;

        while(l<r){ // do this till left is smaller than right pointer
            if(height[l]<=height[r]){ // we will select the position that we are dealing with as the smaller height bw the left and the right pointer so that we are always sure that we are dealing with the smaller height so that the water cannot ever overflow.
                if(height[l]<leftmax){
                    ans+=leftmax-height[l];
                }
                else{
                    leftmax = height[l];
                }
                l++;
            }
            else{
                if(height[r]<rightmax){
                    ans+=rightmax-height[r];
                }
                else{
                    rightmax = height[r];
                }
                r--;
            }
        }
        return ans;
    }
```
If you still didn't understand the solution just look at striver's solution with patience and you will be able to understand.

# [54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

[Dry run this diagram to understand the intuition and approach](https://www.youtube.com/clip/Ugkx_pJWYKBCRcyxe8XBNIXJa_Ios62CkcPt)

note: make a 4x4 matrix in the interview before coming up with this approach.
note: code this if possible cause you can make mistakes while coding this.

## Approach ->
Create 4 variables that will track the left,right,top and bottom position indexes so that they remain in the range. We will traverse the matrix until left<=right and top<=bottom.
	Left to right traversal:
	We will traverse all the columns keeping the top index fixed
	```
    for(int i=left; i<=right; i++)
            {
                res.push_back(matrix[top][i]);
            }
    ```

	we will increment the top variable top++;
	
	top to bottom traversal:
	We will traverse all the rows (top to bottom) keeping the right index fixed
	```
    for(int i=top; i<=bottom; i++)
            {
                res.push_back(matrix[right][i]);
            }
	right++;
	```

	NOTE: CHECK HERE THAT TOP<=BOTTOM because if you dry run the diagram you will see that left->right and top->bottom is always possible but after that there might be a case that every element is exhausted so we must have a check here.
	
	right to  left traversal:
	We will traverse all the rows (right to  left) keeping the bottom index fixed
    ```
	for(int i=right; i>=left; i++)
            {
                res.push_back(matrix[bottom][i]);
            }
	bottom++;
    ```

	NOTE: CHECK HERE THAT LEFT<=RIGHT
	bottom to up traversal:
	We will traverse all the rows (bottom to up) keeping the left index fixed
    ```
	for(int i=bottom; i>=up; i++)
            {
                res.push_back(matrix[left][i]);
            }
	left++;
    ```
	
## Code ->
```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix)     {
        vector <int> ans;
        
        int left = 0;
        int right = matrix[0].size()-1;
        int bottom = matrix.size()-1;
        int top = 0;
        
        while(left<=right && top<=bottom){
            for(int i=left; i<=right; i++) // note the for loop here is going forward
                ans.push_back(matrix[top][i]); // note here we are writing matrix[top][i]
            top++;
            
            for(int i=top; i<=bottom; i++)
                ans.push_back(matrix[i][right]); // but note here we are writing matrix[i][right]
            right--;
            
            if(top<=bottom){ // here we are checking for top and bottom and not left and right because we will get an edge case if there is only one row or one column
                for(int i=right; i>=left; i--) // note the for loop here is going backwards
                    ans.push_back(matrix[bottom][i]);
                bottom--;
            }
            
            if(left<=right){
                for(int i=bottom; i>=top; i--)
                    ans.push_back(matrix[i][left]);
                left++;
            }
            
        }
        
        return ans;
    }
};
```

# [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
## Approach -> 
you don't need any approach lol you've solved it like a thousand times
## Code-> 
```cpp
int maxSubArray(vector<int>& nums) {
        int ans = nums[0];
        int sum = 0;

        for(int i=0; i<nums.size(); i++){
            sum+=nums[i];
            ans = max(sum, ans);
            if(sum<0) sum = 0;
            
        }

        return ans;
    }
```

# [73. Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)
## Approaches ->
1. Make a duplicate vector and traverse in the first vector and make rows and columns of duplicate matrix 0.
2. Ask the interviewer if the values in the matrix are non-negative or not. If the values are non-negative, you can tell the following approach -> Visit every element of the array. If an element is 0 then make its row and column elements as -1 but don't make the row and column elements -1 if its 0 too (because this 0 might change the values of other elements). After this is done, visit every element of the array again and make all the -1s as 0. [TC -> (N*M) * (N+M)] i.e. N*M for traversal in N*M vector and N+M for traversing the rows and columns of each 0 element. [SC -> O(1)].
3. Hashing -> Create two vectors of size N and M respectively. Mark the row and column as 0 if there is a 0 in the main array. Accordingly convert the main array to 0s by looking at the two vectors. [TC -> O(N*M + N*M)] [SC -> O(N + M) for taking two dummy vectors]. Note-> Tried this approach after a year but the mistake I did is assume that the size of row and col will be the same and took just one hash table. Don't repeat that in the interview
4. Optimisation of 2nd approach ->  Consider the 0th row and 0th col to be the hashes for the given array. Make two variables row and col and set them to false. Check if the 0th row or the 0th col has any 0s in it. If yes then mark row or col as true. Now consider the 0th row and 0th column as the hash for the remaining array and apply the 3th approach for the remaining array. At the end check if row is true then make the entire 0th row as 0 and do the same for col. Note-> When I solved using this approach after 1 year I applied too much brain. Just simply run all the loops like in example, don't reverse the positions of i and j like don't make matrix[i][0] as matrix[0][I]

# [31. Next Permutation](https://leetcode.com/problems/next-permutation/)
## Code->
```cpp
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        //1 3 5 4 2
        // ans -> 1 4 2 3 5
        // First take index=-1 and we iterate backwards in the vector. If we encounter the order from backwards to be descending at any point, we store that index in "index". eg. in 1 3 5 4 2 if we move from the back, we get 2 4 5 (increasing) 3 (decreasing).. as soon as we get 3 we store its index. 
        int index=-1;
        for(int i=nums.size()-2; i>=0; i--){
            if(nums[i]<nums[i+1]){
                index = i;
                break;
            }
        }
        
         
        // if there is no decreasing order from the back, that means that the vector is already lexicographically greatest. In that case atq we return lexicographically smallest vector.
        if(index==-1){
            reverse(nums.begin(), nums.end());
            return;
        }
        // We go from back again and see if there is any number greater than the number on "index"'s index, and we swap them.
        for(int i=nums.size()-1; i>=0; i--){
            if(nums[i]>nums[index]){
                swap(nums[i], nums[index]);
                break;
            }
        }
        // at the end we will also reverse the array from index+1 beacause we have already created a lexicographically greater order by swapping position index with i, but we need to make the order from index+1 as lexicographically smallest so that the overall order is next lexicographically greater order. 
        reverse(nums.begin()+index+1, nums.end());
    }
};
```
# [74. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)
## Approach->
You assume the 2d matrix to be 1d matrix and use binary search.

## Code->
```
 bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix[0].size(); int n = matrix.size();
        int low = 0;
        int high = (m*n)-1;
        int mid;
        while(low<=high){
            mid = (low+high)/2;
            int val = matrix[mid/m][mid%m]; // note this part 
            if(val==target) return true;
            if(val>target) high = mid-1;
            else low = mid+1;
        }
        return false;
    }
```
# [240. Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)
## Approach ->
A simple search with TC O(n*m). Apply binary search in each row with TC O(n*log m). Most optimal solution is->
Now this might be a little tricky buy I want you to look at the first picture and try to observe something here. On index 0,0 the elements in the row as well as columns is in increasing order. So this won't help us into solving this problem. Same is the case with the index 4,4.. The row is decreasing and the columns is decreasing too. But if you go to index 0,4 or 4,0 the case is different and we can take advantage of that. So for instance let's choose 0,4 and the idea is to see if the target is greater or smaller than the element at that position. If the target is greater simply go to the left, if the target is smaller simply go down. Do this while we are inside the bounds. This is basically a binary search problem as well because although here we are not applying any typical binary search but we are reducing the size of the search using logic and hence it is a binary search problem.

## Code->
```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int row = 0, col = matrix[0].size()-1; // This is the row and col we are going to use to compare it with target.
        while(row<matrix.size() && col>=0){ // While we are still inside the legal boundaries
            if(matrix[row][col]==target) return true;
            else if(matrix[row][col]>target) col--; // go down
            else row++;    // go left
        }
        return false;
    }
};
```
# [769. Max Chunks To Make Sorted](https://leetcode.com/problems/max-chunks-to-make-sorted/)
## Approach-> 
In the question the condition says that there will be elements from 0 to n-1. Approach is very simple yet multiple wrong approaches might come to your head while solving this problem. The correct approach is that you maintain a maximum element while traversing in the array. If the maximum element found till time is equal to i then that is one chunk. The intuition is that each chunk must have one condition that the last element of the chunk must be equal to i. And at the same time there should not be any element in that chunk that is greater than the last element because if that's the case then even if we sort that chunk the elements will not be arranged in the right order because there is one element that is greater than everything else and we must have the greatest element found till time equal to i.
## Code->
```cpp
class Solution {
public:
    int maxChunksToSorted(vector<int>& arr) {
        int ans = 0, maxim=0;
        for(int i=0; i<arr.size(); i++){
            maxim = max(arr[i], maxim);
            if(maxim==i) ans++;
        }
        return ans;
    }
};
```
# [768. Max Chunks To Make Sorted II](https://leetcode.com/problems/max-chunks-to-make-sorted-ii/)
could not solve. 

# [204. Count Primes](https://leetcode.com/problems/count-primes/)
## Approach -> 
Seive of Eratosthenes-> The idea is to assume that every number in the range 0 to n is a prime number. so mark them as true. Then traverse every number and traverse their table and keep marking the elements in their table as false. When you keep doing that till sq. root of n then we will acquire all the prime numbers and composite numbers in our hashtable. The solution given below is beautifully optimized too..
## Code->
```cpp
class Solution {
public:
    int countPrimes(int n) {
        if(n==0)  return 0;
        vector<bool> prime(n, true);
        prime[0] = false, prime[1] = false;
        for (int i = 0; i < sqrt(n); ++i) {
            if (prime[i]) {
                for (int j = i*i; j < n; j += i) { // note below
                    prime[j] = false;
                }    
            }    
        }
        return count(prime.begin(), prime.end(), true);
    }
    
};

// note: instead of i*i you can write i*2 as well but i*i is more efficient. Numbers smaller than i * i would have been already marked as not prime by the time we reach i.
```
TC-> O(n*log(log n))

# [118. Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/)
## Approach ->
- Pretty easy approach. We will at least have numRows=1 according to the constraints. So just make a vector of vector ans and push back 1 in it. We can also observe that no matter what the row number is, it's first and the last elements are always 1. So run a loop from 1 to numRows-1 and on each iteration push back 1 at the start and end. And in the middle observe how the value is the sum of the values of the above two indexes.
---
## Code ->
```cpp
vector<vector<int>> generate(int numRows) {
        vector<vector<int>> ans(numRows);
        ans[0].push_back(1);
        for(int i=1; i<numRows; i++){
            ans[i].push_back(1);
            for(int j=1; j<i; j++){
                ans[i].push_back(ans[i-1][j-1] + ans[i-1][j]);
            }
            ans[i].push_back(1);
        }

        return ans;
    }
```
# [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/description/)

## Approach ->
Kadane's Algorithm
---

## Code ->
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int ans = nums[0];
        int sum = 0;

        for(int i=0; i<nums.size(); i++){
            sum+=nums[i];
            ans = max(sum, ans);
            if(sum<0) sum = 0;     
        }
        return ans;
    }
};
```
