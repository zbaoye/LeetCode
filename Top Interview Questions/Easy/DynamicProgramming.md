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