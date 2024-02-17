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