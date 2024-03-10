# DP Basics ->

**Key Points:**
1. **Dynamic Programming Introduction:**
   - Definition: Storing answers to sub-problems for efficient solving of the main problem.
   - Two approaches: Memoization and Tabulation.

2. **Memoization (Top-Down):**
   - Recursive solution with overlapping sub-problems.
   - Steps:
      - Create dp array.
      - Check if the answer is already calculated (dp[n] != -1).
      - If not, calculate and store in dp array.
   - Time Complexity: O(N), Space Complexity: O(N).

3. **Tabulation (Bottom-Up):**
   - Iterative approach starting from base cases.
   - Steps:
      - Declare dp array.
      - Initialize base conditions.
      - Iterate to calculate dp[i] using dp[i-1] and dp[i-2].
   - Time Complexity: O(N), Space Complexity: O(N).

4. **Space Optimization:**
   - Utilize only the last two values (prev and prev2) instead of maintaining an entire array.
   - Time Complexity: O(N), Space Complexity: O(1).

**Code Examples:**
1. **Memoization:**
   ```cpp
   int f(int n, vector<int>& dp){
       if(n<=1) return n;
       if(dp[n]!= -1) return dp[n];
       return dp[n] = f(n-1, dp) + f(n-2, dp);
   }
   ```

2. **Tabulation:**
   ```cpp
   vector<int> dp(n+1, -1);
   dp[0] = 0;
   dp[1] = 1;
   for(int i=2; i<=n; i++)
       dp[i] = dp[i-1] + dp[i-2];
   ```

3. **Space Optimization:**
   ```cpp
   int prev2 = 0;
   int prev = 1;
   for(int i=2; i<=n; i++){
       int cur_i = prev2 + prev;
       prev2 = prev;
       prev = cur_i;
   }
   ```

# [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/description/)

## Approaches ->
This q is v similar to fibonacci.

## Codes ->
1. Recursion
```cpp
class Solution {
public:
    int fibo(int n, vector<int> &dp){
        // Base Case:
        // when we go to the 0th stair we should return 1 
        //When there are 0 stairs (n=0), there is only one way to climb them, 
        //which is to not climb at all. Returning 1 in this case signifies that there is one way to climb 0 stairs.
        // We will go to 0th stair when we move 1 step i.e. only one way to go to 0
        if(n==0 || n==1) return 1;

        if(dp[n]!=-1) return dp[n];

        return dp[n] = fibo(n-1, dp) + fibo(n-2, dp);

    }
    int climbStairs(int n) {
        vector<int> dp(n+1,-1);
        return fibo(n, dp);
    }
};
```

2. Tabulation
```cpp
class Solution {
public:
    int climbStairs(int n) {
        vector<int> dp(n+1,-1);

        dp[1] = 1;
        dp[2] = 2;

        for(int i=3; i<n+1; i++){
            dp[i] = dp[i-1] + dp[i-2];
        }

        return dp[n];
    }
};
```

3. Space Optimization
```cpp
class Solution {
public:
    int climbStairs(int n) {
        int prev1 = 2;
        int prev2 = 1;
        int ans;

        for(int i=3; i<n+1; i++){
            ans = prev1+prev2;

            prev2 = prev1;
            prev1 = ans;
        }

        return ans;
    }
};
```