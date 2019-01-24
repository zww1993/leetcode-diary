# leetcode日记

语言：Java

仅用于记录自己的leetcode解题思路，第一遍做，可能不是最优解，以后再补充。

Show you the code, and talk is not cheap.

题目来自 https://leetcode-cn.com

持续更新中......

## 目录

* [1. 两数之和]
* [2. 两数相加]
* [3. 无重复字符的最长子串]
* [4. 寻找两个有序数组的中位数]

[1. 两数之和]: #1-两数之和
[2. 两数相加]: #2-两数相加
[3. 无重复字符的最长子串]: #3-无重复字符的最长子串
[4. 寻找两个有序数组的中位数]: #4-寻找两个有序数组的中位数

## 1. 两数之和

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

新建一个Map。遍历时，取 target 与 nums[i] 的差值，判断Map中以此差值为key的键值对是否存在，若存在，则返回 i 与所取到的键值对中的value，否则将 nums[i] 作为key，i 作为value存入Map，continue。


## 2. 两数相加

#### 题目描述：

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例：

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

#### 解答：

code:
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode start = null;
        ListNode end = null;
        int over = 0;
        while(l1 != null || l2 != null || over > 0) {
            int num1 = 0;
            int num2 = 0;
            if (l1 != null) {
                num1 = l1.val;
                l1 = l1.next;
            }
            if (l2 != null) {
                num2 = l2.val;
                l2 = l2.next;
            }
            int sum = num1 + num2 + over;
            if (sum >= 10) {
                over = 1;
                sum = sum % 10;
            }else {
                over = 0;
            }
            ListNode listNode = new ListNode(sum);
            if (start == null) {
                start = listNode;
            }
            if (end != null) {
                end.next = listNode;
            }
            end = listNode;
        }
        return start;
    }
}
```

talk:

一开始我的思路是将两个链表转换成数字，相加以后在转换回链表，后来发现当两个数字过大时，会造成溢出，所以这个思路走不通，就换了一个思路。将两个数字对应的每一位从链表中取出，两数以及进位数相加得到结果写入新的链表并记录下进位的情况，直至两个数字都读取到了最高位并且进位为0；

## 3. 无重复字符的最长子串

#### 题目描述：

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:
```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

示例 2:
```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

示例 3:
```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

#### 解答：

code:
```java
class Solution {
    public static int lengthOfLongestSubstring(String s) {
        int max = 0;
        for (int i = 0; i < s.length(); i++) {
            int length = 1;
            for (int j = i + 1; j < s.length(); j++) {
                String subStr = s.substring(i, j);
                String c = new String(new char[] {s.charAt(j)});
                if (subStr.contains(c)) {
                    break;
                }
                length++;
            }
            if (length > max) {
                max = length;
            }
        }
        return max;
    }
}
```

talk:

利用双浮标，浮标i为字符串起点，内层循环从i+1的位置开始判断下标为j的字符在[i, j-1]区间内是否存在，若不存在length自增，j向右移动一位。若存在break，如果length大于max，则将length赋值给max，然后i向右移动一位，继续下一次新的内层循环，直到浮标i移动到字符串结尾，返回max的值。

## 4. 寻找两个有序数组的中位数

#### 题目描述

给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。

请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 nums1 和 nums2 不会同时为空。

示例 1:
```
nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0
```

示例 2:
```
nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5
```

#### 解答：

code:
```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        if (nums1.length == 0) {
            return findMedian(nums2);
        }
        if (nums2.length == 0) {
            return findMedian(nums1);
        }
        
        int[] nums = sort(nums1, nums2);
        return findMedian(nums);
    }
    
    private double findMedian(int[] nums) {
        int length = nums.length;
        if (length % 2 == 0) {
            return (nums[length/2-1]+nums[length/2])/2.0f;
        }
        return nums[length/2];
    }
    
    private int[] sort(int[] nums1, int[] nums2) {
        int[] array = new int[nums1.length+nums2.length];
        int i1 = 0;
        int i2 = 0;
        while (i1 < nums1.length || i2 < nums2.length) {
            if (i1 >= nums1.length) {
                while (i2 < nums2.length) {
                    array[i1 + i2] = nums2[i2];
                    i2++;
                }
                break;
            }
            if (i2 >= nums2.length) {
                while (i1 < nums1.length) {
                    array[i1 + i2] = nums1[i1];
                    i1++;
                }
                break;
            }
            int num1 = nums1[i1];
            int num2 = nums2[i2];
            if (num1 > num2) {
                array[i1 + i2] = num2;
                i2++;
            }else {
                array[i1 + i2] = num1;
                i1++;
            }
        }
        return array;
    }
}
```

talk:

这个问题可以拆解成两个小问题：1.将两个有序数组合并成一个有序数组，2.求一个有序数组的中位数。我们依次来实现。

1.将两个有序数组合并成一个有序数组

首先创建一个长度为两个数组长度之和的数组，用来承接新的数组数据。然后while循环遍历两个数组中的内容。分别从nums1和nums2中根据下标i1和i2取出数据，将较小的数据存储新数组，然后较小数据的下标右移一位直到两个数组都遍历完成。需要注意的是，只要有一个数组遍历完成，直接将另一个数组中的数据依次插入新数组即可。

2.求一个有序数组的中位数

如果数组的length为奇数，则中位数等于下标为 length/2 的值，如果数组的length为偶数，则中位数等于下标为 length/2 - 1 和 length/2 的两个数的平均数。 





