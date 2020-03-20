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

