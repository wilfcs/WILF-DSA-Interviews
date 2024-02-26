# [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)

## Approaches ->
You know the brute force, let's look at the optimized sol. We will have two pointers left and right. Pointer ‘left’ is used for maintaining the starting point of the substring while ‘right’ will maintain the endpoint of the substring.’ right’ pointer will move forward and check for the duplicate occurrence of the current element if found then the ‘left’ pointer will be shifted ahead so as to delete the duplicate elements. We will make a map that will take care of counting the elements and maintaining the frequency of each and every element as a unity by taking the latest index of every element.

## Code ->
```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        // Check if the string is empty, return 0 if true
        if(s.size() == 0) 
            return 0;

        // Create a map to store the latest index of each character in the string
        unordered_map<int, int> mp;

        // Initialize two pointers 'l' and 'r', and 'ans' to store the final result
        int l = 0, r = 0, ans = 0;

        // Iterate through the string using the 'r' pointer
        while(r < s.size()) {
            // Check if the current character at 'r' is already in the map and its index is greater than or equal to 'l'
            if(mp.find(s[r]) != mp.end() && mp[s[r]] >= l) {
                // If true, move 'l' pointer to the next position after the last occurrence of the current character
                l = mp[s[r]] + 1;
            }

            // Update the maximum length of substring without repeating characters
            ans = max(ans, r - l + 1);

            // Update the index of the current character in the map
            mp[s[r]] = r;

            // Move 'r' pointer to the next character
            r++;
        }

        // Return the final result
        return ans;
    }
};
```

# [1004. Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/description/)

## Code ->
```cpp
class Solution {
public:
    int longestOnes(vector<int>& nums, int k) {
        int i = 0, j = 0, ans = 0, temp = 0, zeroCnt = 0;

        // Iterate through the array using two pointers (i and j)
        while (j < nums.size()) {
            // Check if the current element at the right pointer (j) is 0, cause if it is 1 you just keep moving j and keep updating answer 
            if (nums[j] == 0) {
                zeroCnt++; // Increment the count of zeros
                // If the count of zeros exceeds the allowed flips (k),
                // move the left pointer (i) until you eliminate a 0 from the window
                if (zeroCnt > k) {
                    while (i <= j) {
                        if (nums[i] == 0) {
                            zeroCnt--; // Decrement the count as a zero eliminated from the window
                            i++; // Move the left pointer to the right
                            break; // Exit the loop once a zero is flipped back
                        }
                        i++; // Move the left pointer to the right
                    }
                }
            }
            j++; // Move the right pointer to the right
            ans = max(ans, j - i); // Update the maximum length of subarray with at most k flips
        }
        return ans; // Return the final result
    }
};
