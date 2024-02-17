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
