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

# 2. Least Significant Bit which is set

## Question ->
Find the position of the first 1 from right to left, in the binary representation of an Integer.

Examples:
Input: n = 18
Output: 2

Explanation: Binary Representation of 18 is 010010, hence the position of the first set bit from the right is 2.

---
## Approach ->
Start from 0 positions and iterate till the 32nd-bit position if any bit is 1 break the loop and print the index else 

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