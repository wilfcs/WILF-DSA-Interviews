# [1512. Number of Good Pairs](https://leetcode.com/problems/number-of-good-pairs/description/)

## Approach->
There is a clear obeservation that the pairs are made on the basis of every same number that has appeared previously. The number of pairs keep increasing with the increasing frequency. Suppose 1 occured 1 times then number of pairs will be 0. If 1 occured 2 times then 1 pair. If 1 occured 3 times then 2 pairs. If 1 occured 4 times then 6 pairs. We are clearly adding the resultant with the frequency every time frequency increases.

## Code->
```cpp
class Solution {
public:
    int numIdenticalPairs(vector<int>& nums) {
        unordered_map <int, int> mp;
        int res = 0;

        for(int i=0; i<nums.size(); i++){
            mp[nums[i]]++;
            res+=mp[nums[i]]-1;
        }

        return res;
    }
};
```

# [1688. Count of Matches in Tournament](https://leetcode.com/problems/count-of-matches-in-tournament/description/)
## Approach->
Each team other than the winner loses exactly once.
## Code->
```cpp
class Solution {
public:
    int numberOfMatches(int n) {
        return n-1;
    }
};
```
