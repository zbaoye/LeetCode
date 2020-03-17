# Array

## 1. Remove Duplicates from Sorted Array
Given a sorted array nums, remove the duplicates in-place such that each element appear only once and return the new length.  

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.  
### Example
Given nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.

It doesn't matter what you leave beyond the returned length.

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
Input: [7,1,5,3,6,4]  
Output: 7  
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4. Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.

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
