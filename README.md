# leetcode日记

语言：Java

仅用于记录自己的leetcode解题思路，第一遍做，可能不是最优解，以后再补充。

Show you the code, and talk is not cheap.

## 目录

* [1.两数之和]

[1.两数之和]: #1.两数之和

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

遍历数组，将值作为key，下标作为value存储到一个Map中。每次遍历时，取 target 与 nums[i] 的差值，判断Map中以此差值为key的键值对是否存在，若存在，则返回 i 与所取到的键值对中的value，否则将 nums[i] 作为key，i 作为value存入Map，continue。


## 2. 两数相加

### 题目描述：

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




