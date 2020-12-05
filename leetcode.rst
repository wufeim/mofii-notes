LeetCode
=====================================

2. **Add Two Numbers**

   You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

   You may assume the two numbers do not contain any leading zero, except the number 0 itself.

   .. code-block:: Python

      class Solution:
          def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
              carry = 0
              root = n = ListNode(0)
              while l1 or l2 or carry:
                  val = 0
                  if l1:
                      val += l1.val
                      l1 = l1.next
                  if l2:
                      val += l2.val
                      l2 = l2.next
                  val += carry
                  if val > 9:
                      val -= 10
                      carry = 1
                  else:
                      carry = 0
                  n.next = ListNode(val)
                  n = n.next
              return root.next

3. **Longest Substring Without Repeating Characters**

   Given a string :code:`s`, find the length of the longest substring without repeating characters.

   .. code-block:: Python

      class Solution:
          def lengthOfLongestSubstring(self, s: str) -> int:
              char_idx = [-1] * 256
              max_len = 0
              start = -1
              for i in range(len(s)):
                  if char_idx[ord(s[i])] > start:
                      start = char_idx[ord(s[i])]
                  char_idx[ord(s[i])] = i
                  max_len = max(max_len, i - start)
              return max_len
