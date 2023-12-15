# [1572. Matrix Diagonal Sum](https://leetcode.com/problems/matrix-diagonal-sum/)

## Approach->
- There is an observation that the the middle cell will only collide in case of the matrix size being in odd number. For example, if the matrix is 3x3 then the middle cell will be the same for both sides diagonal. But in the case of a 4x4 matrix that is not the case. So traverse the diagonals and keep calculating the sum. If the matrix is odd then subtract the middle element from the final answer.

## Code->
```
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

# 42. Trapping Rain Water
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