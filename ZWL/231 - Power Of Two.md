### Description
Given an integer, write a function to determine if it is a power of two.

### Idea
When a integer is power of two, it means its top digit in binary form is 1 and the other digits is 0. So the `and` operation between n and n-1 returns 0 when n is power of two.

### Code 
```Java
class Solution {
    public boolean isPowerOfTwo(int n) {
        if (n<=0) return false;
        return 0 == (n & (n-1)) ? true : false;
    }
}
```
