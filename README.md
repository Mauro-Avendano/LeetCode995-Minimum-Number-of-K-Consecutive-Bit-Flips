# LeetCode995-Minimum-Number-of-K-Consecutive-Bit-Flips

[LeetCode 995 Minimum Number of K Consecutive Bit Flips](https://leetcode.com/problems/minimum-number-of-k-consecutive-bit-flips/description/)

To solve this problem we can follow our intuition an start to perform the flips from left to right. That means start at index 0, check if it needs a flip and perform the operation over all the k values starting with the current index.

```java
class Solution {
    public int minKBitFlips(int[] nums, int k) {
        int count = 0;

        for(int i = 0; i < nums.length; i++) {
            // needs a flip
            if (nums[i] == 0) {
                for (int j = 0; j < k; j++) {
                    if (i+j >= nums.length) return - 1;
                    if (nums[i+j] == 0) nums[i + j] = 1;
                    else nums[i+j] = 0;
                }
                count++;
            }
        }

        return count;
    }
}
```

Surprisingly, this intuitive solution works because this problem can be solved with a Greedy approach but after submitting we get a TLE because of the O(n*k) time complexity.

In order to reduce the time complexity we will combine our Greedy approach with a technique called [sliding window](https://www.geeksforgeeks.org/window-sliding-technique/).

Here we will need a variable **windowFlips** to keep track of how many flips for the given window of size k have been done, this is used to check if at index **i** we need a flip or not using the **%** operator.

We also will need a **Boolean Array flipped** to keep track if at the index **i**, a flip was needed, this is used to recalculate the **windowsFlips** while the window is being shifted.

Finally, for both solutions we need to check that if a flip needs to be done, the remaining indexes in the array should be enough to satisfy the complete window. If this is not possible, then we return -1.

```java
class Solution {
    public int minKBitFlips(int[] nums, int k) {
        //we need to keep track the number of flips on each window of size k 
        int windowFlips = 0;
        //also keep track if a given index was flipped
        boolean[] flipped = new boolean[nums.length];
        //total flips
        int flips = 0;

        for (int i = 0; i < nums.length; i++) {
            //recalculate the number of flips for each window
            if (i >= k) {
                if (flipped[i - k]) windowFlips--;
            }

            //check if needs to be flipped
            if ((nums[i] + windowFlips) % 2 == 0) {
                //if the whole window can not be flipped, it's not possible
                if (i + k > nums.length) return - 1;

                flipped[i] = true;
                flips++;
                windowFlips++;
            }
        }

        return flips; 
    }
}
```

With this approach we have reduce the time complexity to O(n) and the space complexity is O(n) due to the flipped array.

