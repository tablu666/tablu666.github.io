# Double Pointer Algorithm

##  Double Pointer Problems
According to [CS-Notes](https://cyc2018.github.io/CS-Notes/#/notes/Leetcode%20%E9%A2%98%E8%A7%A3%20-%20%E5%8F%8C%E6%8C%87%E9%92%88), I want to share some thoughts about 6 common double pointer problems on [**Leetcode**](https://leetcode.com/problemset/all/).
<!-- more -->

### Sorted Array - Two Sum
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
Having done this before, the first thing that came to mind was to iterate over the array, storing each value and its index into a HashMap, and then iterate over the array again, looking for the existence of (target- current value) in the dictionary.
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
It turns out that we could use double pointer algorithm to solve this problem. The idea is very simple. As the topic said, only unique solutions, there must be a sum of two numbers equals to target in the array. We could arrange one pointer at the head and the other at the tail, pointing to minimum and maximum respectively. By judging the difference between the sum of two numbers and the target value, we could easilty tune the maximum value or minimum, ensuring that getting the answer with at most one iteration, and the space complexity is reduced from O (N) to O (1).
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

### Two Sum Square

[633. Sum of Square Numbers (Easy)](https://leetcode.com/problems/sum-of-square-numbers/description/)

**Problem**
Given a non-negative integer c, your task is to decide whether there're two integers a and b such that $a^2+b^2=c$.

**Example:**
Input: 5
Output: True
Explanation: $1\times1+2\times2=5$

**Solution**
I couldn't believe that we could use double pointer algorithm in this problem. After seeing the solution, I would say it is incredible! The idea is that one pointer points to 0 and the other pointer points to the square root of the input value (taking the integer part). This ensures that the minimum and maximum values cover all the integer values in the input, and then narrowing the range is enough.

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

### Reverse Vowels

[345. Reverse Vowels of a String (Easy)](https://leetcode.com/problems/reverse-vowels-of-a-string/description/)

**Problem**
Write a function that takes a string as input and reverse only the vowels of a string.

**Example**
Input: "hello"
Output: "holle"

**Solution**
This problem is also very interesting, using two pointers perfectly solved (head and tail + judging the vowel + exchange), the only thing to pay attention to is the vowel "AEIOU" contains big lowercase! No wonder thumbs down 👎 more than up 👍!
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

### Valid Palindrome

[680. Valid Palindrome II (Easy)](https://leetcode.com/problems/valid-palindrome-ii/description/)

**Problem**
Given a non-empty string s, you may delete at most one character. Judge whether you can make it a palindrome.

**Example**
Input: "abca"
Output: True

**Solution**
Double pointer is amazing! As long as the left and right ends of the pointer synchronous judgment, once the two Pointers point to different characters, then you can give a deletion of the opportunity, that is: the left to remove a character, continue to judge the rest; Or cut off a character on the right and judge the rest.
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

###Merge Array

[88. Merge Sorted Array (Easy)](https://leetcode.com/problems/merge-sorted-array/description/)

**Problem**
Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

**Example**
Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6], n = 3

Output: [1,2,2,3,5,6]

**Solution**
Solved this in March. I was using double pointer algorithm at that time. But naive as I was trying to solve it from the head this time. Actually, it is possible that the merge will overwrite the values that have not been compared yet, so the correct way to do the comparison is to start from the back, and note that those zeros are reserved seats!
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

### Linkedlist Cycle

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
At the beginning, I only thought of the data structure: just to traverse, store every Node and POS with HashMap, continue to see if the current Node can be found in the map. This should be a natural and intuitive idea.
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
If you use the double pointer algorithm, you can imagine the running race. There is often round running track. As long as there is a circle, as long as the time is unlimited, the fast runner will inevitably catch up with the slow runner. Therefore, the idea of the double pointer here is to set two fast and slow Nodes, the slow has one step, the fast has two steps. They will meet if there is a cycle. It is also known as Floyd's Tortoise and Hare Algorithm.
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

### Longest through del

[524. Longest Word in Dictionary through Deleting (Medium)](https://leetcode.com/problems/longest-word-in-dictionary-through-deleting/description/)

**Problem**
Given a string and a string dictionary, find the longest string in the dictionary that can be formed by deleting some characters of the given string. If there are more than one possible results, return the longest word with the smallest lexicographical order. If there is no possible result, return the empty string.

**Example**
Input:
s = "abpcplea", d = ["ale","apple","monkey","plea"]

Output: 
"apple"

**Solution**
Haven't solved many Medium level problems. I was kind of lost at the beginning. This one is really hard. In fact, by switching ideas, this is a typical double pointer pattern (judging subsequence). The list can be sorted by a dictionary, or it is also possible to judge the lexicographical order when traversing the list. Thoughts are just two parts: 1. Traversing the list to find the most value (the longest and the minimum lexicographical order), this is to define an external variable out of loop. 2. Judging subsequence. This can be perfectly solved with double pointer, a bit similar to the KMP algorithm, but this is much simpler!
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


