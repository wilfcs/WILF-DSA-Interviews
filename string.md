# [1021. Remove Outermost Parentheses](https://leetcode.com/problems/remove-outermost-parentheses/)

## Code ->
```cpp
class Solution {
public:
    string removeOuterParentheses(string S) {
        string res;
        int opened = 0;
        for (char c : S) {
            if (c == '(' && opened++ > 0) res += c;
            if (c == ')' && opened-- > 1) res += c;
        }
        return res;
    }
};
```

# [151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/description/)

## Approach ->
Use the concept of tokenization bro. Just divide the string into tokens based on spaces. But there will be empty tokens as well if there are multiple spaces together. So take care of that and about the reversal part, just don't do ans+=token, do ans = token + ans. That adds the tokens in reverse order.

## Code ->
```cpp
class Solution {
public:
    string reverseWords(string s) {
        // Using stringstream for tokenization
        stringstream ss (s); 
        string token = "", ans="";
        
        // Extract tokens from the string on basis of spaces and reverse the order
        while(getline(ss, token, ' ')) 
            if(token.size())ans = token + " " + ans;

        // Remove the trailing space at the end and return the result  
        return ans.substr(0, ans.size()-1);
    }
};
```

# [1903. Largest Odd Number in String](https://leetcode.com/problems/largest-odd-number-in-string/description/)
## Approach ->
We just iterate from right to left, and if we found an odd number then return from 0 till that odd number. We do this because in order to find the largest valued odd number we obviously should return from start to the last odd digit.

## Code ->
```cpp
class Solution {
public:
    string largestOddNumber(string num) {
        string ans = "";

        // Variable to store the index of the rightmost odd digit
        int en=-1;

        // Iterate from the right to the left in the input string
        for(int i=num.size()-1; i>=0; i--){
            if((num[i]-'0') % 2){
                // If odd, update the index and break the loop
                en = i;
                break;
            }
        }

        // If no odd digit is found, return an empty string
        if(en==-1) return ans;

        // Return the substring from the beginning to the rightmost odd digit
        return num.substr(0, en+1);
    }
};
```

# [205. Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/description/)

## Approach ->
The provided C++ code determines whether two given strings, s and t, are isomorphic. It uses an unordered_map to store the mapping of characters from s to t and a set to track characters in t that have been used as a mapping. The code iterates through the characters, ensuring that no two characters in s map to the same character in t, and checks for isomorphism conditions. If the lengths of the strings are different or the mapping conditions are violated, the function returns false; otherwise, it returns true.

## Code->
```cpp
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        // Using an unordered_map to store the mapping of characters from string s to string t
        unordered_map<char, char> mp;
        // Using a set to keep track of characters in string t that have already been mapped
        set<char> st;
        
        // Check if the lengths of both strings are not equal, return false if they are not
        if(s.size()!=t.size()) return false;

        // Iterate through each character in the strings
        for(int i=0; i<s.size(); i++){
            // Check if the current character in string s has already been mapped
            if(mp.count(s[i])){
                // If mapped, check if the mapping is the same as the corresponding character in string t
                if(mp[s[i]]!=t[i]) return false;
            }
            else{
                // If the character in string s is not mapped yet
                // Check if the corresponding character in string t has already been used as a mapping
                if(st.find(t[i])!=st.end()) return false;
                
                // If not, create a mapping from the character in string s to the character in string t
                mp[s[i]] = t[i];
                // Add the character in string t to the set to mark it as used
                st.insert(t[i]);
            }
        }
        
        // If all characters in string s can be mapped to string t, return true
        return true;
    }
};
```

# [796. Rotate String](https://leetcode.com/problems/rotate-string/description/)

## Approach ->
We can use the typical approach of rotating the string and then finding if the two strings match but we will have to rotate the string from every position to avoid edge cases. Instead of that complex approach just use this one liner..

## Code ->
```cpp
class Solution {
public:
    bool rotateString(string s, string goal) {
        return s.size() == goal.size() && (s + s).find(goal) != string::npos;
    }
};
```

## Explanation ->
s.size() == goal.size(): This condition checks if the lengths of the two strings s and goal are equal. If they are not of the same length, it's impossible for s to become goal through rotations.

(s + s).find(goal) != string::npos: This part checks if the concatenated string (s + s) contains the string goal. The find function returns the position of the first occurrence of goal in the concatenated string. If goal is not found, find returns string::npos, which is a special constant representing "no position" or "not found."

If find does not return npos, it means that goal is found in the concatenated string, indicating that s can become goal after some rotations.

If find returns npos, it means that goal is not found in the concatenated string, and therefore, s cannot become goal through rotations.

TC -> O(len(s) * len(goal)).

## Alternative Code 
```cpp
class Solution {
public:
    bool rotateString(string s, string goal) {
        for(int i=0;i<=s.size();i++){
            s.push_back(s[0]);
            string temp=s.erase(0,1);
            if(s==goal){
                return true;
            }
        }
        return false;
        
    }
};
```

TC -> Same as above

# [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/description/)

## Approaches ->
1. Sort the strings and compare. TC-> O(n log n)
2. Store them in hashmap and compare. TC-> O(n). SC-> O(1) because we only have 26 characters so SC will still be O(1)

## Code ->
```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        multiset <char> st;
        multiset <char> st2;
        if(s.size() != t.size()) return false;
        for(int i=0; i<s.size(); i++){
            st.insert(s[i]);
        }
        for(int i=0; i<t.size(); i++){
            st2.insert(t[i]);
        }
        
        if(st == st2) return true;
        else return false;
    }
};
```

# [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/description/)

## Code ->
```cpp
class Solution {
public:
    string frequencySort(string s) {
        // Create a vector of pairs to store the frequency and corresponding ASCII character
        vector<pair<int, char>> vec(256);

        // Initialize the vector with zeros and ASCII characters
        for(int i = 0; i < 256; i++){
            vec[i].first = 0;
            vec[i].second = i;
        }

        // Update the frequency of each character in the string
        for(int i = 0; i < s.size(); i++){
            int pos = s[i];
            vec[pos].first++;
        }

        // Sort the vector in descending order based on frequency
        sort(vec.begin(), vec.end(), greater<>());

        // Construct the sorted string based on the sorted vector
        string ans = "";
        for(int i = 0; i < 256; i++){
            // Break if the frequency is zero, as the rest of the elements will also be zero
            if(vec[i].first == 0) break;

            int sz = vec[i].first;
            char ch = vec[i].second;
            while(sz--){
                ans += ch;
            }
        }
        return ans;
    }
};
```

# [1614. Maximum Nesting Depth of the Parentheses](https://leetcode.com/problems/maximum-nesting-depth-of-the-parentheses/submissions/1185933324/)

## Approaches ->
1. Using stack
2. Using simple cnt variable.

## Code ->
```cpp
#include <bits/stdc++.h>
class Solution {
public:
    int maxDepth(string s) {
        int cnt = 0;
        int ans = 0;

        for(int i=0; i<s.size(); i++){
            if(s[i]=='('){
                cnt++;
            }
            else if(s[i]==')'){
                cnt--;
            }

            ans = max(ans, cnt);
        }
        return ans;
    }
};
```