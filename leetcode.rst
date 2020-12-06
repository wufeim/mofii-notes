LeetCode
=====================================

以下是我刷LeetCode做的一些笔记，给出了一些我认为足够高效也足够通用的方法。本人博士在读，因此刷LeetCode也只是为了通过一些实习的笔试，对于一些算法挖掘的并不透彻，从这个意义上说，这些内容对于除我以外的别人并无任何参考价值。

2. **Add Two Numbers**

   You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

   You may assume the two numbers do not contain any leading zero, except the number 0 itself.

   - 这里将多个循环合并，即在 :code:`l1` 或 :code:`l2` 或 :code:`carry` “有意义”的情况下循环继续
   - 如果结果 :code:`n` 的初始值为 :code:`None` ，则循环内不能直接对 :code:`n.next` 赋值，这里使用了一个trick，即给定一个dummy node :code:`ListNode(0)` ，在返还结果时再跳过该node :code:`root.next`

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

   - 这里采用了sliding window的方法，从左向右寻找最长的substring
   - 对于每一个substring，如何检测新的字符是否已经在之前出现过呢？我们使用一个index array从字符ascii码对应到字符所在的index，如此以来只需要将映射得到的index与starting point位置做比较

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

5. **Longest Palindromic Substring**

   Given a string :code:`s` , return the longest palindromic substring in :code:`s` .

   - 这里采用的是从中心出发向两边扩张的方法
   - 回文字符串可以分为1个中心和两个中心的情况，因此共需要考虑 :math:`2n-1` 个情况
   - 总体时间复杂度 :math:`O(n^2)` ，:math:`O(1)` 的空间复杂度

   .. code-block:: Python

      class Solution:
          def longestPalindrome(self, s: str) -> str:
              if len(s) == 0:
                  return s
              longest_palindrome = ""
              for i in range(len(s)):
                  substr1 = self._longestPalindrome(s, i, i)
                  substr2 = self._longestPalindrome(s, i, i+1)
                  longest_palindrome = substr1 if len(longest_palindrome) < len(substr1) else longest_palindrome
                  longest_palindrome = substr2 if len(longest_palindrome) < len(substr2) else longest_palindrome
              return longest_palindrome

          def _longestPalindrome(self, s, l, r):
              while (l >= 0 and r < len(s) and s[l] == s[r]):
                  l -= 1
                  r += 1
              return s[l+1:r]
