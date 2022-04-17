# 双指针算法


##  双指针问题
结合[CS-Notes](https://cyc2018.github.io/CS-Notes/#/notes/Leetcode%20%E9%A2%98%E8%A7%A3%20-%20%E5%8F%8C%E6%8C%87%E9%92%88)，对于LeetCode面试常见的双指针问题，分享一些心得体会。
<!-- more -->

### 有序数组 Two Sum
[167. Two Sum II - Input array is sorted (Easy)](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/description/)

**Problem:**
Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

**Note:**
Your returned answers (both index1 and index2) are not zero-based.
You may assume that each input would have exactly one solution and you may not use the same element twice.

**Example:**
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]

**Solution:**
由于之前做过twoSum印象深刻，首先想到的是遍历数组，把每个值及其索引存进HashMap，然后再次遍历数组，去字典里查找(target-当前值)是否存在。
```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        HashMap<Integer, Integer> map = new HashMap<>();
        int[] temp = new int[2];
        
        for(int i = 0; i < numbers.length; i++) {
            map.put(numbers[i], i);
        }
        
        for(int i = 0; i < numbers.length; i++) {
            if(map.containsKey(target- numbers[i])) {
                int index1 = i;
                int index2 = map.get(target- numbers[i]);
                temp[0] = index1 + 1;
                temp[1] = index2 + 1;
                return temp;
            }
        }
        return temp;
    }
}
```
学习发现可以用双指针解决，思路很简单，题目说有且仅有唯一解，因此数组中必定存在两数之和为目标值，那么让两个指针一个在头一个在尾，分别指向最小值和最大值，通过判断两数之和与目标值的差异，从而将最大值调小或最小值调大，这样可以确保最多遍历一遍数组获得解，空间复杂度从O(N)降到O(1)。
```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int min = 0;
        int max = numbers.length - 1;
        while ((numbers[min] + numbers[max]) != target) {
            if ((numbers[min] + numbers[max]) < target) {
                min++;
            } else {
                max--;
            }
        }
        int[] res = new int[]{min + 1, max + 1};
        return res;
    }
}
```
------

### 两数平方和 Two Sum Square

[633. Sum of Square Numbers (Easy)](https://leetcode.com/problems/sum-of-square-numbers/description/)

**Problem**
Given a non-negative integer c, your task is to decide whether there're two integers a and b such that $a^2+b^2=c$.

**Example:**
Input: 5
Output: True
Explanation: $1\times1+2\times2=5$

**Solution**
这题我是怎么也想不到可以用双指针，看完解决思路只有一个妙字可言！思路就是一个指针指向0，另一个指针指向input值的开方（取整数部分），这样可以确保最小值和最大值涵盖了input内所有整数取值范围，然后缩小范围即可。

```java
class Solution {
    public boolean judgeSquareSum(int c) {
        int min = 0;
        int max = (int) Math.sqrt(c);
        while (min <= max) {
            int sum = min * min + max * max; 
            if (sum < c) {
                min++;
            } else if (sum > c) {
                max--;
            } else {
                return true;
            }
        }
        return false;
    }
}
```
------

### 元音字符首尾交换 Reverse Vowels

[345. Reverse Vowels of a String (Easy)](https://leetcode.com/problems/reverse-vowels-of-a-string/description/)

**Problem**
Write a function that takes a string as input and reverse only the vowels of a string.

**Example**
Input: "hello"
Output: "holle"

**Solution**
这题也很有意思，用双指针可以完美解决（前后遍历+判断元音+交换），唯一要注意的是元音aeiou含大小写！ps: 难怪点踩👎比点赞👍多！
```java
class Solution {
    public String reverseVowels(String input) {
        if (input == null || input.equals("")) {
            return input;
        }
        char[] chars = input.toCharArray();
        Set<Character> set = new HashSet<Character>(Arrays.asList('a','e','i','o','u','A','E','I','O','U'));
        int s = 0;
        int e = input.length() - 1;
        while (s < e) {
            char start = chars[s];
            char end = chars[e];
            if (!set.contains(start)) {
                s++;
            } else if (!set.contains(end)) {
                e--;
            } else {
                chars[s++] = end;
                chars[e--] = start;
            }
        }
        return String.valueOf(chars);
    }
}
```
------

### 判断回文字符串 Valid Palindrome

[680. Valid Palindrome II (Easy)](https://leetcode.com/problems/valid-palindrome-ii/description/)

**Problem**
Given a non-empty string s, you may delete at most one character. Judge whether you can make it a palindrome.

**Example**
Input: "abca"
Output: True

**Solution**
双指针大法好！只要左右两端指针同步判断，一旦两个指针指向不同字符，那么可以给一次删除的机会，即：左边舍去一个字符，继续判断剩下的；或右边舍去一个字符，判断剩下的。
```java
class Solution {
    public boolean validPalindrome(String s) {
        for (int i = 0, j = s.length() - 1; i < j; i++, j--) {
            if (s.charAt(i) != s.charAt(j)) {
                return check(i, j - 1, s) || check(i + 1, j, s);
            }
        }
        return true;
    }
    
    private boolean check(int min, int max, String s) {
        while (min < max) {
            if (s.charAt(min++) != s.charAt(max--)) {
                return false;
            }
        }
        return true;
    }
}
```
------

### 归并两个有序数组 Merge Array

[88. Merge Sorted Array (Easy)](https://leetcode.com/problems/merge-sorted-array/description/)

**Problem**
Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

**Example**
Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6], n = 3

Output: [1,2,2,3,5,6]

**Solution**
这题3月份的时候做过了，当时确实用了双指针，但是天真的我两个指针都是从头开始比较，没办法，惯性思维嘛，结果麻烦得很，毕竟有可能合并的时候会覆盖还没比较的值。所以正确的打开方式是从后开始比较，注意那几个0就是预留的位子！
```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        while (n >= 1 && m >= 1) {
            if (nums1[m - 1] < nums2[n - 1]) {
                nums1[m + n - 1] = nums2[n - 1];
                n--;
            } else {
                nums1[m + n - 1] = nums1[m - 1];
                m--;
            }
        }
        if (m < 1) {
            while (n >= 1) {
                nums1[n - 1] = nums2[n - 1];
                n--;
            }
        }
    }
}
```
------

### 判断链表有环 Linkedlist Cycle

[141. Linked List Cycle (Easy)](https://leetcode.com/problems/linked-list-cycle/description/)

**Problem**
Given head, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.

Return true if there is a cycle in the linked list. Otherwise, return false.
```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
```
**Solution**
这题一开始我只想到了数据结构，无非进行遍历，用HashMap存储每一个Node和pos，往后走，看看当前Node在map里能不能找到。这应该是一个很自然直观的思路。
```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        Map<ListNode, Integer> con = new HashMap<>();
        
        if(head == null) {
        	return false;
        }

        for(int pos = 0; head != null; head = head.next) {
            if(con.containsKey(head)) {
            	return true;
            }
            con.put(head, pos++);
        }
        return false;
    }
}
```
如果用双指针的话，可以想象一下操场跑步比赛，经常有体育生套圈，只要有环，只要不限时间，跑的快的必然会追上跑的慢的。因此双指针思路就是设定两个快慢节点，慢的走一步，快的走两步，能遇上说明有cycle。
```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast!= null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {
                return true;
            }
        }
        return false;
    }
}
```
------

### 字典顺序最长子序列 Longest through del

[524. Longest Word in Dictionary through Deleting (Medium)](https://leetcode.com/problems/longest-word-in-dictionary-through-deleting/description/)

**Problem**
Given a string and a string dictionary, find the longest string in the dictionary that can be formed by deleting some characters of the given string. If there are more than one possible results, return the longest word with the smallest lexicographical order. If there is no possible result, return the empty string.

**Example**
Input:
s = "abpcplea", d = ["ale","apple","monkey","plea"]

Output: 
"apple"

**Solution**
Medium的题没有做过几道，有点小慌，看了题目果然没头绪，其实转换一下思路，这就是典型的双指针题目（判断子序列）。可以list按字典排序，也可以在遍历list的时候判断字典顺序。思路就是两点：1.遍历list找最值（最长和字典顺序最小），这个就是定义循环外变量；2.判断子序列，这个可以用双指针完美解决，有点类似KMP算法前期处理，但是这个要简单得多！
```java
class Solution {
    public String findLongestWord(String s, List<String> d) {
        String res = "";
        for (String element : d) {
            if (isContain(element, s)) {
                if (element.length() > res.length()
                    || (element.length() == res.length() && element.compareTo(res) < 0)) {
                    res = element;
                }
            }
        }
        return res;
    }
    
    private boolean isContain(String child, String parent) {
        int i = 0;
        int j = 0;
        while (i < child.length() && j < parent.length()) {
            if (child.charAt(i) == parent.charAt(j)) {
                i++;
            }
            j++;
        }
        return i == child.length();
    }
}
```

