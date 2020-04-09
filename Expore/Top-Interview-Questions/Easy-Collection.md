# Array

## 1. Remove Duplicates from Sorted Array
Given a sorted array nums, remove the duplicates in-place such that each element appear only once and return the new length.  

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.  
### Example
```
Given nums = [1,1,2],
Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.
It doesn't matter what you leave beyond the returned length.
```

### Code
```c++
int removeDuplicates(vector<int>& nums) {
    if(nums.empty()) return 0;
    int frontIndex = 0;
    int endIndex = 0;
    
    while(endIndex < nums.size()){
        if(nums[endIndex] != nums[frontIndex]){
            nums[++frontIndex] = nums[endIndex++];
        }else{
            endIndex++;
        }
    }
    return frontIndex+1;
}
```

## 2. Best Time to Buy and Sell Stock II

Say you have an array for which the ith element is the price of a given stock on day i.  
Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).  
*Note: You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).*  

### Example
```
Input: [7,1,5,3,6,4]  
Output: 7  
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4. Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
``

### Code
```c++
int maxProfit(vector<int>& prices) {
    int sum = 0;
    for(int i = 1; i< prices.size(); i++){
        if (prices[i] > prices[i-1] ){
            sum += prices[i] - prices[i-1];
        }
    }
    return sum;        
}
```

## 3. Rotate Array
Given an array, rotate the array to the right by k steps, where k is non-negative.

### Example
```
Input: [1,2,3,4,5,6,7] and k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```

### Code
```c++
void rotate(vector<int>& nums, int k) {
    k = k%nums.size();
    reverse(nums.begin(), nums.end());
    reverse(nums.begin(), nums.begin()+k);
    reverse(nums.begin()+k, nums.end());
}
```

## 4. Contains Duplicate
Given an array of integers, find if the array contains any duplicates.  
Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.  

### Example
```
Input: [1,1,1,3,3,4,3,2,4,2]
Output: true
```

### Code
```c++
bool containsDuplicate(vector<int>& nums) {
    if(nums.size()<2) return false;
    sort(nums.begin(), nums.end());
    for(int i = 1; i<nums.size(); i++){
        if(nums[i] == nums[i-1]){
            return true;
        }
    }
    return false;
}
```

## 5. Single Number
Given a non-empty array of integers, every element appears twice except for one. Find that single one.  
*Note: Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?*

### Example
```
Input: [4,1,2,1,2]
Output: 4
```
***Hit: XOR***

### Code
```c++
int singleNumber(vector<int>& nums) {
    if(nums.empty()) return 0;
    sort(nums.begin(), nums.end());
    int sum = nums[0];
    for (int i =1; i<nums.size(); i++){
        if(nums[i-1] == nums[i]){
            sum -= nums[i];
        }else{
            sum += nums[i];
        }
    }
    return sum;
}
```

## 6. Intersection of Two Arrays II
Given two arrays, write a function to compute their intersection.
### Example
```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [4,9]
```
### Code
```c++
vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
    map<int,int> numMap;
    vector<int> result;
    for(int s:nums1) numMap[s]++;
    for(int s:nums2){
        if( numMap[s]-- >0) result.push_back(s);
    }
    return result;
}
```

## 7. Plus One
Given a non-empty array of digits representing a non-negative integer, plus one to the integer.  
The digits are stored such that the most significant digit is at the head of the list, and each element in the array contain a single digit.  
You may assume the integer does not contain any leading zero, except the number 0 itself.

### Example
```
Input: [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.
```

### Code
```c++
vector<int> plusOne(vector<int>& digits) {
    if(digits.empty()) return digits;
    int carry = 1;
    for(int i=digits.size()-1; i>=0; i--){
        int sum = carry+digits[i];
        carry = sum/10;
        digits[i] = sum%10;
    }
    if(carry == 1){
        digits.insert(digits.begin(), carry);
    }
    return digits;
}
```

## 8. Move Zeroes
Given an array *nums*, write a function to move all *0*'s to the end of it while maintaining the relative order of the non-zero elements.
### Example
```
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
```
### Code
```c++
void moveZeroes(vector<int>& nums) {
    int lastNoZeroIndex = 0;
    for(int i=0; i<nums.size(); i++){
        if(nums[i]!=0){
            nums[lastNoZeroIndex++] = nums[i];
        }
    }
    for(int i=lastNoZeroIndex; i<nums.size(); i++){
        nums[i] = 0;
    }
}
```

## 9. Two Sum
Given an array of integers, return indices of the two numbers such that they add up to a specific target.  
You may assume that each input would have exactly one solution, and you may not use the same element twice.
### Example
```
Given nums = [2, 7, 11, 15], target = 9,  
Because nums[0] + nums[1] = 2 + 7 = 9,  
return [0, 1].
```
### Code
```c++
vector<int> twoSum(vector<int>& nums, int target) {
    int vectorSize = nums.size();
    map<int, int> numsMap;
    for (int i = 0; i<vectorSize; i++){
        int res = target - nums[i];
        if(numsMap.count(res)){
            return {numsMap[res], i};
        }else{
            numsMap[nums[i]]=i;
        }
    }
    return {0,0};
}
```

## 10. Valid Sudoku
Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:  
Each row must contain the digits 1-9 without repetition.  
Each column must contain the digits 1-9 without repetition.  
Each of the 9 3x3 sub-boxes of the grid must contain the digits 1-9 without repetition.
### Example
```
Input:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: true
```
### Code
```c++
bool isValidSudoku(vector<vector<char>>& board) {
    bool rows[9][9] = {false};
    bool cols[9][9] = {false};
    bool boxes[9][9] = {false};
    
    for(int i=0; i<9; i++){
        for(int j=0; j<9; j++){
            if(board[i][j] == '.') continue;
            int val = board[i][j] - '1';
            int boxesIndex = (i/3)*3+(j/3);
            if(rows[i][val] || cols[j][val] || boxes[boxesIndex][val]){
                return false;
            }else{
                rows[i][val] = cols[j][val] = boxes[boxesIndex][val] = true;
            }
        }
    }
    return true;
}
```

## 11. Rotate Image
You are given an n x n 2D matrix representing an image.  
Rotate the image by 90 degrees (clockwise).
*Note: You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.*
### Example
```
Given input matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

rotate the input matrix in-place such that it becomes:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```
### Code
```c++
void rotate(vector<vector<int>>& matrix) {
    int n = matrix.size();
    for (int i=0; i<n; i++){
        for (int j=i; j<n; j++){
            swap(matrix[i][j],matrix[j][i]);
        }
        reverse(matrix[i].begin(), matrix[i].end());
    }
}
```

# Strings

## 1. Reverse Integer
Given a 32-bit signed integer, reverse digits of an integer.
### Example
```
Input: 123
Output: 321
```
### Code
```c++
int reverse(int x) {
    int result = 0;
    while(x != 0){
        int pop = x%10;
        x = x/10;
        if(result>INT_MAX/10 || (result == INT_MAX/10 && pop>7)) return 0;
        if(result<INT_MIN/10 || (result == INT_MIN/10 && pop<-8)) return 0;
        result = result*10 + pop;
    }
    return result;
}
```

## 2. First Unique Character in a String
Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.
### Example
```
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.
```
### Code
```c++
int firstUniqChar(string s) {
    int list[26] = {};
    
    for(int i =0; i<s.size(); i++){
        list[s[i] - 'a']++;
    }
    for(int i =0; i<s.size(); i++){
        if(list[s[i]-'a'] == 1){
            return i;
        }
    }
    return -1;
}
```

## 3. Valid Anagram
Given two strings s and t , write a function to determine if t is an anagram of s.
### Example
```
Input: s = "anagram", t = "nagaram"
Output: true
```
### Code
```c++
bool isAnagram(string s, string t) {
    int list[26] = {0};
    for(auto c:s){
        list[c-'a']++;
    }
    for(auto c:t){
        list[c-'a']--;
    }
    for(auto i:list)
        if(i != 0)
            return false;
    return true;
}
```

## 4.  String to Integer (atoi)
Implement atoi which converts a string to an integer.  
The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.  
The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.  
If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.  
If no valid conversion could be performed, a zero value is returned.
### Example
```
Input: "4193 with words"
Output: 4193
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.
```
### Code
```c++
int myAtoi(string str) {
    int startIndex = 0;
    int n = str.size();
    while(startIndex < n && str[startIndex] == ' '){
        startIndex++;
    }
    bool flag = false;
    int neg = 1;
    int result=0;
    for(int i=startIndex; i<n; i++){
        if(str[i] =='-' || str[i]=='+'){
            if(flag) break;
            if(str[i]=='-'){
                neg = -1;   
            }
            flag = true;
        }else if(str[i]>='0'&&str[i]<='9'){
            flag = true;
            int temp = str[i]-'0';
            if(result>INT_MAX/10 || (result==INT_MAX/10 && temp>7)) return INT_MAX;
            if(result<INT_MIN/10 || (result==INT_MIN/10 && temp>8)) return INT_MIN;
            result = result*10 + neg*temp;
        }else{
            break;
        }
    }
    return result;
}
```

## 5. Implement strStr()
Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.
### Example 
```
Input: haystack = "hello", needle = "ll"
Output: 2
```
### Code
```c++
int strStr(string haystack, string needle) {
    if(needle.empty()) return 0;
    int stackSize = haystack.size();
    int needleSize = needle.size();
    for(int i=0; i<stackSize-needleSize+1; i++){
        int j=0;
        for(; j<needleSize; j++){
            if(haystack[i+j] != needle[j]) break;
        }
        if(j == needleSize) return i;
    }
    return -1;
}
```

## 6. Count and Say
The count-and-say sequence is the sequence of integers with the first five terms as following:
### Example
```
1.     1
2.     11
3.     21
4.     1211
5.     111221
```
### Code
```c++
string parse(string input){
    string result="";
    char pre_char = input[0];
    int count = 0;
    for (int i=0; i<input.size(); i++){
        if( input[i] == pre_char){
            count++;
        }else{
            result.push_back(count+'0');
            result.push_back(pre_char);
            pre_char = input[i];
            count=1;
        }
    }
    result.push_back(count+'0');
    result.push_back(pre_char);
    return result;
}
string countAndSay(int n) {
    if(n == 1) return "1";
    string pre = countAndSay(n-1);
    return parse(pre);
}
```

## 7. Longest Common Prefix
Write a function to find the longest common prefix string amongst an array of strings.  
If there is no common prefix, return an empty string "".
### Example
```
Input: ["flower","flow","flight"]
Output: "fl"
```
### Code
```c++
string getComm(string s1, string s2){
    int size = (s1.size()>s2.size())?s2.size():s1.size();
    string result = "";
    for (int i=0; i<size; i++){
        if(s1[i] == s2[i]){
            result.push_back(s1[i]);
        }else{
            return result;
        }
    }
    return result;
}
string longestCommonPrefix(vector<string>& strs) {
    if(strs.empty()) return "";
    int size = strs.size();
    string common = strs[0];
    for (int i=1; i<size; i++){
        common = getComm(common,strs[i]);
        if(common == ""){
            return "";
        }
    }
    return common;
}
```

# Linked List
## 1. Delete Node in a Linked List
Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.  
Given linked list -- head = [4,5,1,9], which looks like following:
### Example
```
Input: head = [4,5,1,9], node = 5
Output: [4,1,9]
Explanation: You are given the second node with value 5, the linked list should become 4 -> 1 -> 9 after calling your function.
```
### Code
```cpp
void deleteNode(ListNode* node) {
    node->val = node->next->val;
    node->next = node->next->next;
}
```

## 2. Remove Nth Node From End of List
Given a linked list, remove the n-th node from the end of list and return its head.
### Example
Given linked list: 1->2->3->4->5, and n = 2. 
After removing the second node from the end, the linked list becomes 1->2->3->5.
### Code
```cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
    ListNode* newHead = new ListNode(0);
    newHead->next = head;
    ListNode* fast = newHead;
    ListNode* slow = newHead;
    for(int i=0; i<n+1; i++){
        fast = fast->next;
    }
    while(fast != NULL){
        fast = fast->next;
        slow = slow->next;
    }
    slow ->next = slow->next->next;
    return newHead->next;   
}
```

## 3. Reverse Linked List
Reverse a singly linked list.
### Example
```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```
### Code
```cpp
ListNode* reverseList(ListNode* head) {
    if (head == NULL) return head;
    ListNode* newHead = new ListNode(0);
    ListNode* cur = head;
    while(head != NULL){
        head = head->next;
        cur->next = newHead->next;
        newHead->next = cur;
        cur = head;
    }
    return newHead->next;
}
```

## 4. Merge Two Sorted Lists
Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.
### Example
```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```
### Code
```cpp
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    if (l1 == NULL) return l2;
    if (l2 == NULL) return l1;
    
    ListNode *mergedList = new ListNode(0);
    ListNode *firstNode = mergedList;
    while(l1!=NULL && l2!=NULL){
        if(l1->val <= l2->val){
            mergedList -> next = l1;
            l1 = l1->next;
        }else{
            mergedList -> next = l2;
            l2 = l2->next;
        }
        mergedList = mergedList->next;
    }
    if(l1==NULL){
        mergedList ->next = l2;
    }
    if(l2==NULL){
        mergedList ->next = l1;
    }
    return firstNode->next;
}
```

## 5. Palindrome Linked List
Given a singly linked list, determine if it is a palindrome.
### Example
```
Input: 1->2->2->1
Output: true
```
### Code
```cpp
ListNode* reverse(ListNode* head){
    ListNode* pre = NULL;
    ListNode* cur = head;
    while(cur != NULL){
        ListNode* temp = cur ->next;
        cur ->next = pre;
        pre = cur;
        cur = temp;
    }
    return pre;
}
bool isPalindrome(ListNode* head) {
    if(head == NULL) return true;
    ListNode* fast = head;
    ListNode* slow = head;
    while(fast->next!=NULL && fast->next->next != NULL){
        slow = slow->next;
        fast = fast->next->next;
    }
    slow = reverse(slow->next);
    while(slow != NULL){
        if(slow->val != head->val){
            return false;
        }else{
            slow = slow->next;
            head = head->next;
        }
    }
    return true;
}
```

## 6. Linked List Cycle
Given a linked list, determine if it has a cycle in it.  
To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.
### Example
```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```
### Code
```cpp
bool hasCycle(ListNode *head) {
    if(head == NULL || head -> next == NULL) return false;
    
    ListNode* slow = head;
    ListNode* fast = head->next;
    while(fast!=slow){
        if(fast == NULL || fast->next == NULL){
            return false;
        }
        fast = fast -> next ->next;
        slow = slow -> next;
    }
    return true;
}
```

# Trees
## 1. Validate Binary Search Tree
Given a binary tree, determine if it is a valid binary search tree (BST).  
Assume a BST is defined as follows:  
- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than the node's key.
- Both the left and right subtrees must also be binary search trees.
### Example
```
    2
   / \
  1   3

Input: [2,1,3]
Output: true
```
### Code
```cpp
vector<int> nodeList;
bool isValidBST(TreeNode* root) {
    if(!root) return true;
    inOrder(root);
    for(int i=1; i<nodeList.size(); i++){
        if(nodeList[i]<=nodeList[i-1]){
            return false;
        }
    }
    return true;
}
void inOrder(TreeNode* root){
    if (!root) return;
    inOrder(root->left);
    nodeList.push_back(root->val);
    inOrder(root->right);
}
```


## 2. Symmetric Tree
Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).  
For example, this binary tree [1,2,2,3,4,4,3] is symmetric:
### Example
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
### Code
```cpp
bool isMirror(TreeNode* left, TreeNode* right){
    if(left == NULL && right == NULL) return true;
    if(left == NULL || right == NULL) return false;
    if(left->val != right->val) return false;
    return isMirror(left->left, right->right) && isMirror(left->right, right->left);
}
bool isSymmetric(TreeNode* root) {
    if(root == NULL) return true;
    return isMirror(root->left, root->right);
}
```

## 3. Binary Tree Level Order Traversal
Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).
### Example
```
    3
   / \
  9  20
    /  \
   15   7
```
return its level order traversal as:
```
[
  [3],
  [9,20],
  [15,7]
]
```
### Code
```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> result ={};
    if (root==NULL) return result;
    
    vector<TreeNode*> level = {root};
    
    while(!level.empty()){
        vector<int> tempVal;
        vector<TreeNode*> levelTemp;
        for (auto node : level){
            tempVal.push_back(node->val);
            if(node->left) levelTemp.push_back(node->left);
            if(node->right) levelTemp.push_back(node->right);
            
        }
        level = levelTemp;
        result.push_back(tempVal);
    }
    return result;
}
```

## 4. Convert Sorted Array to Binary Search Tree
Given an array where elements are sorted in ascending order, convert it to a height balanced BST.  
For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

## Example
```
Given the sorted array: [-10,-3,0,5,9],
One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:
      0
     / \
   -3   9
   /   /
 -10  5
```

### Code
```cpp
TreeNode* sortedArrayToBST(vector<int>& nums) {
    return helper(nums, 0, nums.size()-1);
}
TreeNode* helper(vector<int>& nums, int begin, int end){
    if(begin>end) return NULL;
    int mid = begin + (end-begin)/2;
    TreeNode* temp = new TreeNode(nums[mid]);
    temp -> left = helper(nums, begin, mid-1);
    temp -> right = helper(nums, mid+1, end);
    return temp;
}
```

# Dynamic Programming

## Best Time to Buy and Sell Stock
Say you have an array for which the ith element is the price of a given stock on day i.  
If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.
### Example
```
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
```
### Code
```cpp
int maxProfit(vector<int>& prices) {
    if (prices.size()<2) return 0;
    int sum = 0;
    int max = 0;
    for (int i=1; i<prices.size(); i++){
        sum = sum + (prices[i]-prices[i-1]);
        if(sum > max) max = sum;
        if(sum < 0) sum =0;
    }
    return max;
}
```

## Maximum Subarray
Given an integer array *nums*, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.
### Example
```
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```
### Code
```cpp
int maxSubArray(vector<int>& nums) {
    if(nums.empty()) return 0;
    int dp[nums.size()];
    dp[0] = nums[0];
    int max = nums[0];
    for (int i=1; i<nums.size(); i++){
        dp[i] = (dp[i-1]>0)?dp[i-1]+nums[i]:nums[i];
        if(dp[i]>max) max = dp[i];
    }
    return max;
}
```

## House Robber
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and *it will automatically contact the police if two adjacent houses were broken into on the same night*.  
Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight *without alerting the police*.
### Example
```
Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```
### Code
```cpp
int rob(vector<int>& nums) {
    if(nums.empty()) return 0;
    int dp[nums.size()];
    dp[0] = nums[0];
    if(nums.size()>1) dp[1] = (nums[1]>nums[0])?nums[1]:nums[0];
    for(int i=2; i<nums.size(); i++){
        dp[i] = (nums[i]+dp[i-2] > dp[i-1] )?nums[i]+dp[i-2]:dp[i-1];
    }
    return dp[nums.size()-1];
}
```

# Math
## Count Primes
Count the number of prime numbers less than a non-negative number, *n*.
### Example
```
Input: 10
Output: 4
Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
```
### Code
```cpp
int countPrimes(int n) {
    int count = 0;
    vector<bool> prime(n,true);
    for (int i = 2; i<n; i++){
        if(prime[i] == true){
            count++;
            for(int j = 2; i*j<n; j++){
                prime[i*j] = false;
            }
        }
    }
    return count;
}
```

# Others
## Hamming Distance
The Hamming distance between two integers is the number of positions at which the corresponding bits are different.  
Given two integers *x* and *y*, calculate the Hamming distance.
### Example
```
Input: x = 1, y = 4
Output: 2
Explanation:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑
The above arrows point to positions where the corresponding bits are different.
```
### Code
```cpp
int hammingDistance(int x, int y) {
    int dis = 0;
    while(x>0 && y>0){
        dis = dis + (x%2!=y%2);
        x /= 2;
        y /= 2;
    }
    while(x>0){
        dis = dis + (x%2);
        x /= 2;
    }
    while(y>0){
        dis = dis + (y%2);
        y /= 2;
    }
    return dis;
}
```

## Pascal's Triangle
Given a non-negative integer numRows, generate the first numRows of Pascal's triangle.
### Example
```
Input: 5
Output:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```
### Code
```cpp
    vector<vector<int>> generate(int numRows) {    
        vector<vector<int>> Pascal(numRows);
        for (int i = 0; i<numRows;i++){
            Pascal[i].push_back(1);
            for(int j=0; j<i-1; j++){
                Pascal[i].push_back(Pascal[i-1][j] + Pascal[i-1][j+1]);
            }
            if(i != 0){
                Pascal[i].push_back(1);
            }
        }
        return Pascal;
    }
```

## Missing Number
Given an array containing n distinct numbers taken from `0, 1, 2, ..., n`, find the one that is missing from the array.
### Example
```
Input: [9,6,4,2,3,5,7,0,1]
Output: 8
```
### Code
```cpp
    int missingNumber(vector<int>& nums) {
        int size = nums.size();
        return size*(size+1)/2 - accumulate(nums.cbegin(),nums.cend(),0);
    }
```