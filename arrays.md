# [1572. Matrix Diagonal Sum](https://leetcode.com/problems/matrix-diagonal-sum/)

<a href="https://leetcode.com/problems/matrix-diagonal-sum/" target="_blank">Opens in new tab</a>

Approach->
- There is an observation that the the middle cell will only collide in case of the matrix size being in odd number. For example, if the matrix is 3x3 then the middle cell will be the same for both sides diagonal. But in the case of a 4x4 matrix that is not the case. So traverse the diagonals and keep calculating the sum. If the matrix is odd then subtract the middle element from the final answer.

Code->
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