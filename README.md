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
* [6. Z字形变换]
* [7. 整数反转]
* [8. 字符串转换整数(atoi)]

[1. 两数之和]: #1-两数之和
[2. 两数相加]: #2-两数相加
[3. 无重复字符的最长子串]: #3-无重复字符的最长子串
[4. 寻找两个有序数组的中位数]: #4-寻找两个有序数组的中位数
[6. Z字形变换]: #6-Z字形变换
[7. 整数反转]: #7-整数反转
[8. 字符串转换整数(atoi)]: #8-字符串转换整数(atoi)

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

#### 答案：

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

#### 答案：

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

#### 答案：

##### 方法一：

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

##### 方法二：滑动窗口

code:
```java
class Solution {
    public static int lengthOfLongestSubstring(String s) {
        int max = 0;
        Map<Character,Integer> map = new HashMap<>();
        int i = 0;
        for (int j = 0; j < s.length(); j++) {
            if (map.containsKey(s.charAt(j))) {
                i = Math.max(map.get(s.charAt(j)) + 1, i);
            }
            max = Math.max(max, j - i + 1);
            map.put(s.charAt(j), j);
        }
        return max;
    }
}
```

talk:

初始i和j都在0，j开始向右移动，并且每移动一位，判断当前对应的字符在Map中是否存在，如果存在并且该字符在Map中存储的下标大于i当前的位置，就将i移动到该字符下标+1的位置，然后将对应的字符作为key，下标作为value存入Map。

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

#### 答案：

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

## 6. Z字形变换

#### 题目描述

将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：

```
L   C   I   R
E T O E S I I G
E   D   H   N
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。

请你实现这个将字符串进行指定行数变换的函数：

```
string convert(string s, int numRows);
```

示例 1:

```
输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"
```

示例 2:

```
输入: s = "LEETCODEISHIRING", numRows = 4
输出: "LDREOEIIECIHNTSG"
解释:

L     D     R
E   O E   I I
E C   I H   N
T     S     G
```

#### 答案

code:
```java
class Solution {
    public String convert(String s, int numRows) {
        int y = 0;
        int max = Math.min(s.length(), numRows);
        if (max == 1) {
            return s;
        }
        boolean flag = true;
        List<List> list = new ArrayList<>();
        for (int i = 0; i < max; i++) {
            List<Character> cl = new ArrayList<>();
            list.add(cl);
        }
        for (int i = 0; i < s.length(); i++) {
            Character c = s.charAt(i);
            List<Character> cl = list.get(y);
            cl.add(c);
            if (flag) {
                y++;
                if (y >= max - 1) {
                    flag = false;
                }
            }else {
                y--;
                if (y <= 0) {
                    flag = true;
                }
            }
        }
        StringBuilder result = new StringBuilder();
        for (List<Character> cl: list) {
            for (Character c : cl) {
                result.append(c);
            }
        }
        return result.toString();
    }
}
```

talk:

根据规则把字符串转换成了一个二维数组，然后遍历二维数组组成新的字符串。

## 7. 整数反转

#### 题目描述

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

示例 1:
```
输入: 123
输出: 321
```

 示例 2:
```
输入: -123
输出: -321
```
 
 示例 3:
```
输入: 120
输出: 21
```
 
 注意:

假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

#### 答案

##### 方法一：

code:
```java
class Solution {
    public int reverse(int x) {
        try {
            if (x == 0) {
            return 0;
        }
        int flag = 1;
        String s = x + "";
        if (x < 0) {
            flag = -1;
            s = x * -1 + "";
        }
        StringBuilder str = new StringBuilder();
        for (int i = s.length() - 1; i >= 0; i--) {
            str.append(s.charAt(i));
        }
        int result = Integer.valueOf(str.toString()) * flag;
        return result;
        }catch(Exception e) {
            return 0;
        }
    }
}
```

talk:

把int转成字符串，翻转字符串后转回int。

##### 方法二：

code:
```java
class Solution {
    public int reverse(int x) {
        int result = 0;
        while (x != 0) {
            int m = x % 10;
            x = x / 10;
            if (result > Integer.MAX_VALUE/10 || (m > 7 && result == Integer.MAX_VALUE/10)) {
                return 0;
            }
            if (result < Integer.MIN_VALUE/10 || (m < -8 && result == Integer.MIN_VALUE/10)) {
                return 0;
            }
            result = result * 10 + m;
        }
        return result;
    }
}
```

talk:

循环取余，然后*10累加，需要注意的是2^31-1=2147483647，-2^31=-2147483648，不要忘记判断末位即可。

## 8. 字符串转换整数(atoi)

#### 题目描述

请你来实现一个 atoi 函数，使其能将字符串转换成整数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。

当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。

该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。

注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0。

说明：

假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，qing返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。

示例 1:
```
输入: "42"
输出: 42
```

示例 2:
```
输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
```

示例 3:
```
输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。
```

示例 4:
```
输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。
```

示例 5:
```
输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
     因此返回 INT_MIN (−231) 。
```

#### 答案

code:
```java
class Solution {
    public int myAtoi(String str) {
        if (str.length() == 0) {
            return 0;
        }
        String[] arr = str.split(" ");
        String s = null;
        for (String string: arr) {
            if (!string.isEmpty()) {
                s = string;
                break;
            }
        }
        if (s == null) {
            return 0;
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (c == '-' || c == '+') {
                if (i == 0) {
                    sb.append(c);
                    continue;
                }else {
                    break;
                }
            }
            if (c < '0' || c > '9') {
                break;
            }
            sb.append(c);
        }
        String resultStr = sb.toString();
        if (resultStr.isEmpty()) {
            return 0;
        }
        if (resultStr.equals("+") || resultStr.equals("-")) {
            return 0;
        }
        System.out.println(resultStr);
        try {
            return Integer.valueOf(resultStr);
        } catch (Exception e) {
            if (resultStr.startsWith("-")) {
                return Integer.MIN_VALUE;
            }
            return Integer.MAX_VALUE;
        }
    }
}
```

talk:

这题其实并不复杂，但是题目描述的并不清楚而且要考虑到各种情况，被测试用例坑了好久。先拿" "分割获取到第一个非空连续字符串，然后遍历字符拼成新串转回数字即可。
