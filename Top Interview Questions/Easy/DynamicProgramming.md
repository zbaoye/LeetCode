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