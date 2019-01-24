# leetcode日记

语言：Java

仅用于记录自己的leetcode解题思路，第一遍做，可能不是最优解，以后再补充。

Show you the code, and talk is not cheap.

## 1.两数之和

#### 题目描述：

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:

```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

#### 解答：

code:
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < nums.length; i++) {
            int a = target - nums[i];
            if (map.containsKey(a)) {
                return new int[] {map.get(a), i};
            }
            map.put(nums[i], i);
        }
        return null;
    }
}
```

talk:

遍历数组，将值作为key，下标作为value存储到一个Map中。每次遍历时，取 “和” 与 nums[i] 的差值，判断Map中以此差值为key的键值对是否存在，若存在，则返回i与所取到的键值对中的value，否则将{nums[i] : i}存入Map，continue。
