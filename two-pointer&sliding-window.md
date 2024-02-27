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
```

# [904. Fruit Into Baskets](https://leetcode.com/problems/fruit-into-baskets/description/)

## Approach ->
Maintain a window from l to r where you're moving. Maintain a map to keep track of the types of fruits in the two baskets as you move, if more than 2 types of fruits come in the basket then remove from the left till one type is completly gone. Then keep calculating your answer as you move.
## Code ->
```cpp
class Solution {
public:
    int totalFruit(vector<int>& fruits) {
        // Initialize pointers 'l' and 'r', and variable 'ans' to store the maximum number of fruits
        int l = 0, r = 0, ans = 0;

        // Create an unordered_map to store the count of each fruit type in the current window
        unordered_map<int, int> mp;

        // Iterate through the array using the 'r' pointer
        while (r < fruits.size()) {
            // Update the count of the current fruit in the map
            mp[fruits[r]]++;

            // Adjust 'l' pointer if more than two types of fruits are present in the window
            while (mp.size() > 2) {
                // keep subtracting the frequency of fruits on the left and keep moving the left at the end
                mp[fruits[l]]--;

                // If the count of a fruit becomes zero, that means our baskets have only one type of fruit left
                // so remove the history of that fruit from the map
                if (mp[fruits[l]] == 0)
                    mp.erase(fruits[l]);

                // keep moving the left pointer
                l++;
            }

            // Update the maximum number of fruits picked in the current window
            ans = max(ans, r - l + 1);

            // Move 'r' pointer to the next position
            r++;
        }

        // Return the final result
        return ans;
    }
};
```
# [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/description/)

## Approach ->
The approach is simple.. we will keep updating the window and its elements in a map. If the windowSize - maxRepeating (windowsize-maxrepeating gives us the value of extra elements in the window that we need to convert to make the window largest) element comes out to be greater than k then that means that we have other elements in the map that exceed the value of k. So we remove that element's frequency from the map and move our window.

GPT Explanation:

The approach involves maintaining a sliding window and updating the frequency of each character within the window using a map. The key insight is to track the difference between the window size and the maximum frequency of any character in the window (windowSize - maxRepeating). This difference represents the number of extra elements in the window that need to be converted to make the window as large as possible.

If this difference becomes greater than the allowed operations (k), it implies there are other characters in the window exceeding the permitted modifications. To address this, the algorithm adjusts the window by decrementing the frequency of the leftmost character, effectively moving the left boundary of the window. This process continues iteratively, and at each step, the length of the current window is compared with the maximum length encountered so far, ensuring the result reflects the longest substring containing the same letter achievable within the given constraints.

## Code ->
```cpp
class Solution {
public:
    int characterReplacement(string s, int k) {
        // Create a map to store the frequency of each character in the window
        unordered_map<char, int> mp;
        
        // Pointers for the left and right boundaries of the window
        int l = 0, r = 0;
        
        // Variable to store the maximum repeating character count in the current window
        int maxRepeating = 0;
        
        // Variable to store the length of the longest substring
        int ans = 0;

        // Iterate through the string
        while (r < s.size()) {
            // Update the frequency of the current character in the window
            mp[s[r]]++;
            
            // Update the maximum repeating character count in the current window
            maxRepeating = max(maxRepeating, mp[s[r]]);
            
            // Calculate the size of the current window
            int windowSize = r - l + 1;

            // If the number of elements to be changed to make all elements the same
            // is greater than k, then we need to shrink the window
            if (windowSize - maxRepeating > k) {
                // Reduce the frequency of the leftmost character in the window
                mp[s[l]]--;
                
                // Move the left boundary of the window to the right
                l++;
            } else {
                // Update the length of the longest substring
                ans = max(ans, windowSize);
            }

            // Move the right boundary of the window to the right
            r++;
        }

        // Return the length of the longest substring
        return ans;
    }
};
```

# [930. Binary Subarrays With Sum](https://leetcode.com/problems/binary-subarrays-with-sum/description/)

## Approaches ->
- Brute Force
- Exactly same as subarray sum equals k -> Maintain the prefix sum of the elements of the array (don't make a separate vector for it, keep it in an integer). Make a hashmap. If the prefix sum is equal to k then increase the answer with 1 as because you were directly able to find the sum of subarray equal to k. Now find if the value of prefix sum - k exist in the map. If it does then add the frequency of sum - k to answer. Now update the map (insert the subarray sum in the map).
- Most optimal method: Using 2 pointer, will add later

## Code ->
2. 
```cpp
class Solution {
public:
    int numSubarraysWithSum(vector<int>& nums, int goal) {
        unordered_map<int, int> mp;
        int sum = 0, ans = 0;

        for(int i=0; i<nums.size(); i++){
            sum += nums[i];
            if(sum==goal) ans++;
            if(mp.count(sum-goal)) ans+=mp[sum-goal];

            mp[sum]++;
        }   
        return ans;

    }
};
```

# [1248. Count Number of Nice Subarrays](https://leetcode.com/problems/count-number-of-nice-subarrays/description/)

## Approaches ->
Same as last approaches

## Code ->
```cpp
class Solution {
public:
    int numberOfSubarrays(vector<int>& nums, int k) {
        unordered_map<int, int> mp;
        int sum = 0, ans = 0;

        for(int i=0; i<nums.size(); i++){
            if(nums[i]%2==1) sum++;

            if(sum==k) ans++;
            if(mp.count(sum-k)) ans += mp[sum-k];

            mp[sum]++;
        }
        return ans;
    }
};
```