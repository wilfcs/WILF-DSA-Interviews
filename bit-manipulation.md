# Kth Bit is set or not

## Question->
Given a number N and a bit number K, check if the Kth bit of N is set or not. A bit is called set if it is 1.

Example:
Input: n = 5, k = 1

Output: SET
Explanation: 5 is represented as 101 in binary and has its first bit set.

---
## Approaches->
1. Check whether the K-th bit is set or not Using Right Shift Operator: Right shift the given number by k to create a number that has its last bit as k-th bit. Now you can check if the number is even then it has 0 as k-th bit else 1 as k-th bit. You can also bitwise & it with 1. If the resultant is 1 then k-th bit is 1 if the resultant is 0 then it is 0.
2. Make a mask by left shifting 1: Left shift 1 by k to create a mask that has only a set bit as kth bit and rest all the bits are 0. `temp = 1 << k` If bitwise AND of n and temp is non-zero, then result is SET else result is NOT SET.

---
## Code->
```cpp
void isKthBitSet(int n, int k)
{
    if (n & (1 << k))
        cout << "SET";
    else
        cout << "NOT SET";
}
```

# Least Significant Bit which is set

## Question ->
Find the position of the first 1 from right to left, in the binary representation of an Integer.

Examples:
Input: n = 18
Output: 2

Explanation: Binary Representation of 18 is 010010, hence the position of the first set bit from the right is 2.

---
## Approach ->
Start from 0 positions and iterate till the 32nd-bit position if any bit is 1 break the loop and print the index. Time complexity is O(1) because we are traversing the loop fixed number of times.

## Code ->
```cpp
int PositionRightmostSetbit(int n)
{
     if (n == 0) 
    {
        return 0;
    }
    else {
        int pos = 1;
        for (int i = 0; i < 32; i++) {
            if (!(n & (1 << i)))
                pos++;
            else
                break;
        }
        return pos;
    }
}
```

# [136. Single Number](https://leetcode.com/problems/single-number/description/)

## Approaches ->
- Brute force
- Hashtable
- The best solution is to use XOR. XOR of all array elements gives us the number with a single occurrence. The idea is based on the following two facts.
    1. XOR of a number with itself is 0.
    2. XOR of a number with 0 is the number itself.
-   Time Complexity: O(n). Space Complexity: O(1)

## Code ->
```cpp
int SingleOccuringELement(int a[],int n)
{
    int res = a[0];
    for (int i = 1; i < n; i++)
        res = res ^ a[i];
 
    return res;
}
```

# [190. Reverse Bits](https://leetcode.com/problems/reverse-bits/)

## Approach->
It's fair and simple. Extract least significant bit (lsb) from n and put it in the lsb of ans. Then left shift answer and right shift n so that we get all of its bits. Do this 32 times in a loop.

## Code ->
```cpp
class Solution {
public:
    uint32_t reverseBits(uint32_t n) {
        uint32_t ans=0;

        for(int i=0; i<32; i++){
            int bit = n&1; // Find the least significant bit of n
            n = n>>1; // move the 2nd least significant bit of n to the first lsb
            ans = (ans << 1) | bit; // left shift ans and then add the bit to its lsb
        }

        return ans;
    }
};
```
# [Swap two numbers without using temp variable]

## Snippet ->
```cpp
a = a ^ b;
b = a ^ b;
a = a ^ b;
```