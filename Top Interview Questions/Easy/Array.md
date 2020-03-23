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

## 12. Reverse Integer
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

## 13. First Unique Character in a String
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

## 14. Valid Anagram
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