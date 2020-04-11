# Medium Collection
## 3Sum
Given an array `nums` of *n* integers, are there elements a, b, c in `nums` such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.
### Example
```
Given array nums = [-1, 0, 1, 2, -1, -4],
A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```
### Code
```cpp
vector<vector<int>> threeSum(vector<int>& nums) {
    vector<vector<int>> output;
    if(nums.size()<3) return output;
    sort(nums.begin(), nums.end());
    int front = 0,end = 0,resValue = 0;
    for(int index=0; index<nums.size(); index++){
        if(nums[index]>0) break;
        resValue = -nums[index];
        front = index+1;
        end = nums.size()-1;
        while(front<end){
            if(nums[front]+nums[end]==resValue){
                output.push_back({nums[index],nums[front++],nums[end--]});
                while(front<end && nums[front]==nums[front-1]) front++;
                while(front<end && nums[end]==nums[end+1]) end--;
            }else if(nums[front]+nums[end]>resValue){
                end--;
            }else{
                front++;
            }
        }
        while(index<nums.size()-1 && nums[index]==nums[index+1]) index++;
    }
    return output; 
}
```

## Set Matrix Zeroes
Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in-place.
### Example
Input: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
Output: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]
### Code
```cpp
void setZeroes(vector<vector<int>>& matrix) {
    int R = matrix.size();
    int C = matrix[0].size();
    bool firstCol=false;
    
    for (int i=0; i<R; i++){
        if (matrix[i][0] == 0){
            firstCol = true;
        }
        for (int j=1; j<C; j++){
            if(matrix[i][j]==0){
                matrix[i][0]=0;
                matrix[0][j]=0;
            }
        }
    }
    for (int i=1; i<R; i++){
        for (int j=1; j<C; j++){
            if(matrix[i][0] == 0 || matrix[0][j] ==0){
                matrix[i][j]=0;
            }
        }
    }
    if(matrix[0][0] == 0){
        for(int j=0; j<C; j++){
            matrix[0][j] = 0;
        }
    }
    if(firstCol){
        for(int i=0; i<R; i++){
            matrix[i][0] = 0;
        }
    }
}
```