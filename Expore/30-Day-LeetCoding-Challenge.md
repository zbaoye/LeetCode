## Group Anagrams
Given an array of strings, group anagrams together.
### Example 
```
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```
### Code
```cpp
vector<vector<string>> groupAnagrams(vector<string>& strs) {
    unordered_map<string,int> resultMap;
    vector<vector<string>> result;
    int count = -1;
    for (auto str : strs){
        string temp = str;
        sort(temp.begin(), temp.end());
        if(resultMap.count(temp) == 0){
            result.push_back({str});
            resultMap[temp] = ++count;
        }else{
            result[resultMap[temp]].push_back(str);
        }
    }
    return result;
}
```

## Counting Elements
Given an integer array *arr*, count element *x* such that *x + 1* is also in *arr*.  
If there're duplicates in *arr*, count them seperately.
### Example
```
Input: arr = [1,3,2,3,5,0]
Output: 3
Explanation: 0, 1 and 2 are counted cause 1, 2 and 3 are in arr.
```
### Code
```cpp
int countElements(vector<int>& arr) {
    if(arr.empty()) return 0;
    sort(arr.begin(),arr.end());
    int count = 0;
    int curCount = 0;
    int preNum = arr[0];
    
    for(int i=0; i<arr.size(); i++){
        if(arr[i] == preNum){
            curCount++;
        }else if((arr[i]-preNum) == 1 ){
            count += curCount;
            preNum = arr[i];
            curCount = 1;
        }else{
            preNum = arr[i];
            curCount = 1;
        }
    }
    return count;
}
```

## Middle of the Linked List
Given a non-empty, singly linked list with head node *head*, return a middle node of linked list.  
If there are two middle nodes, return the second middle node.
### Example
```
Input: [1,2,3,4,5]
Output: Node 3 from this list (Serialization: [3,4,5])
The returned node has value 3.  (The judge's serialization of this node is [3,4,5]).
Note that we returned a ListNode object ans, such that:
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, and ans.next.next.next = NULL.
```
### Code
```cpp
ListNode* middleNode(ListNode* head) {
    if(head==NULL || head->next==NULL) return head;
    ListNode* fast = head->next->next;
    ListNode* slow = head->next;
    while(fast!=NULL && fast->next!=NULL){
        slow = slow -> next;
        fast = fast -> next -> next;
    }
    return slow;
}
```

## Backspace String Compare
Given two strings `S` and `T`, return if they are equal when both are typed into empty text editors. `#` means a backspace character.
### Example
```
Input: S = "a##c", T = "#a#c"
Output: true
Explanation: Both S and T become "c".
```
### Code
```cpp
bool backspaceCompare(string S, string T) {
    int i = S.size()-1;
    int j = T.size()-1;
    int count1=0; int count2=0;
    while(i>=0 || j>=0){
        while(i>=0 && (S[i]=='#' || count1>0)){
            (S[i--] == '#')?count1++:count1--;
        }
        while(j>=0 && (T[j]=='#' || count2>0)){
            (T[j--] == '#')?count2++:count2--;
        }
        if(i<0 || j<0) return i==j;
        if(S[i--]!=T[j--]) return false;
    }
    return i==j;
}
```

## Diameter of Binary Tree
Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.
### Example
```
          1
         / \
        2   3
       / \     
      4   5    
Return 3, which is the length of the path [4,2,1,3] or [5,2,1,3].
```
### Code
```cpp
unordered_map<TreeNode*,int> m;
int diameterOfBinaryTree(TreeNode* root) {
    if(root == NULL) return 0;
    int way1 = maxDepth(root->left)+maxDepth(root->right);
    int way2 = diameterOfBinaryTree(root->left);
    int way3 = diameterOfBinaryTree(root->right);
    return max(way1,max(way2,way3));
}
int maxDepth(TreeNode* root){
    if(root==NULL) return 0;
    if(m.count(root) == 1) return m[root];
    int h = max(maxDepth(root->left),maxDepth(root->right))+1;
    m[root] = h;
    return h;
}
```

## Last Stone Weight
We have a collection of stones, each stone has a positive integer weight.
Each turn, we choose the two heaviest stones and smash them together.  Suppose the stones have weights `x` and `y` with `x <= y`.  The result of this smash is:
- If `x == y`, both stones are totally destroyed;
- If `x != y`, the stone of weight `x` is totally destroyed, and the stone of weight `y` has new weight `y-x`.
At the end, there is at most 1 stone left.  Return the weight of this stone (or 0 if there are no stones left.)
### Example
```
Input: [2,7,4,1,8,1]
Output: 1
Explanation: 
We combine 7 and 8 to get 1 so the array converts to [2,4,1,1,1] then,
we combine 2 and 4 to get 2 so the array converts to [2,1,1,1] then,
we combine 2 and 1 to get 1 so the array converts to [1,1,1] then,
we combine 1 and 1 to get 0 so the array converts to [1] then that's the value of last stone.
```
### Code
```cpp
int lastStoneWeight(vector<int>& stones) {
    priority_queue<int> q;
    for(int s:stones){
        q.push(s);
    }
    while(q.size()>1){
        int a = q.top();q.pop();
        if(!q.empty()){
            int b = q.top();q.pop();
            if(a!=b){
                q.push(a-b);
            }
        }
    }
    return q.empty()?0:q.top();
}
```

## Contiguous Array
Given a binary array, find the maximum length of a contiguous subarray with equal number of 0 and 1.
### Example
```
Input: [0,1,0]
Output: 2
Explanation: [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.
```
### Code
```cpp
int findMaxLength(vector<int>& nums) {
    unordered_map<int, int> m;
    int sum = 0;
    int ans = 0;
    for(int i=0; i<nums.size(); i++){
        sum += nums[i]?1:-1;
        if(sum == 0){
            ans = i+1;
        }else if(m.count(sum)){
            ans = max(ans,i-m[sum]);
        }else{
            m[sum] = i;
        }
    }
    return ans;
}
```

## Perform String Shifts
You are given a string s containing lowercase English letters, and a matrix shift, where shift[i] = [direction, amount]:  
- direction can be 0 (for left shift) or 1 (for right shift). 
- amount is the amount by which string s is to be shifted.
- A left shift by 1 means remove the first character of s and append it to the end.
- Similarly, a right shift by 1 means remove the last character of s and add it to the beginning.
Return the final string after all operations.
### Example
```
Input: s = "abcdefg", shift = [[1,1],[1,1],[0,2],[1,3]]
Output: "efgabcd"
Explanation:  
[1,1] means shift to right by 1. "abcdefg" -> "gabcdef"
[1,1] means shift to right by 1. "gabcdef" -> "fgabcde"
[0,2] means shift to left by 2. "fgabcde" -> "abcdefg"
[1,3] means shift to right by 3. "abcdefg" -> "efgabcd"
```
### Code
```cpp
string stringShift(string s, vector<vector<int>>& shift) {
    int operate = 0;
    int len = s.length();
    for(auto shift_1 : shift){
        if(shift_1[0]==0){
            operate -= shift_1[1];
        }else{
            operate += shift_1[1];
        }
    }
    operate = operate%len;
    if(operate < 0){
        return s.substr(0-operate,len+operate) + s.substr(0,0-operate);
    }else if(operate >0){
        return s.substr(len-operate,operate) + s.substr(0,len-operate);
    }else return s;
}
```

## Valid Parenthesis String
Given a string containing only three types of characters: '(', ')' and '*', write a function to check whether this string is valid. We define the validity of a string by these rules:  
1. Any left parenthesis '(' must have a corresponding right parenthesis ')'.
2. Any right parenthesis ')' must have a corresponding left parenthesis '('.
3. Left parenthesis '(' must go before the corresponding right parenthesis ')'.
4. '*' could be treated as a single right parenthesis ')' or a single left parenthesis '(' or an empty string.
5. An empty string is also valid.
### Example
```
Input: "(*))"
Output: True
```
### Code
```cpp
bool checkValidString(string s) {
    stack<int> left,star;
    for(int i=0; i<s.size(); i++){
        if(s[i] == '('){
            left.push(i);
        }else if(s[i] == '*'){
            star.push(i);
        }else{
            if(left.empty() && star.empty()) return false;
            else if(!left.empty()) left.pop();
            else star.pop();
        }
    }
    while(!left.empty() && !star.empty()){
        if(left.top()>star.top()) return false;
        left.pop();star.pop();
    }
    return left.empty();
}
```

##  Number of Islands
Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.
### Example
```
Input:
11000
11000
00100
00011  
Output: 3
```
### Code
```cpp
int numIslands(vector<vector<char>>& grid) {
    if (grid.empty()||grid[0].empty()) return 0;
    int row = grid.size();
    int col = grid[0].size();
    int res = 0;
    vector<vector<bool>> visited(row, vector<bool>(col));
    for(int i=0; i<row; i++){
        for(int j=0; j<col; j++){
            if(grid[i][j]=='0' || visited[i][j]) continue;
            helper(grid,visited,i,j);
            res++;
        }
    }
    return res;
}
void helper(vector<vector<char>>& grid, vector<vector<bool>>& visited, int x, int y) {
    if (x < 0 || x >= grid.size() || y < 0 || y >= grid[0].size() || grid[x][y] == '0' || visited[x][y]) return;
    visited[x][y] = true;
    helper(grid, visited, x - 1, y);
    helper(grid, visited, x + 1, y);
    helper(grid, visited, x, y - 1);
    helper(grid, visited, x, y + 1);
}
```

## Minimum Path Sum
Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.
### Example
```
Input:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
Output: 7
Explanation: Because the path 1→3→1→1→1 minimizes the sum.
```
### Code
```cpp
int minPathSum(vector<vector<int>>& grid) {
    if(grid.empty() || grid[0].empty()) return 0;
    int m = grid.size(); int n = grid[0].size();
    vector<vector<int>> dp(m, vector<int>(n));
    dp[0][0] = grid[0][0];
    for(int i=1; i<m; i++){
        dp[i][0] = dp[i-1][0]+grid[i][0];
    }
    for(int i=1; i<n; i++){
        dp[0][i] = dp[0][i-1]+ grid[0][i];
    }
    for(int i=1; i<m; i++){
        for (int j=1; j<n; j++){
            dp[i][j] = grid[i][j]+ min(dp[i-1][j],dp[i][j-1]);
        }
    }
    return dp[m-1][n-1];
}
```

## Search in Rotated Sorted Array
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.  
(i.e., [0,1,2,4,5,6,7] might become [4,5,6,7,0,1,2]).    
You are given a target value to search. If found in the array return its index, otherwise return -1.  
You may assume no duplicate exists in the array.  
Your algorithm's runtime complexity must be in the order of O(log n).
### Exmaple
```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```
### Code
```cpp
int search(vector<int>& nums, int target) {
    int left = 0; int right =nums.size()-1;
    while(left<=right){
        int mid = left+(right-left)/2;
        if(nums[mid] == target) return mid;
        if(nums[mid] < nums[right]){
            if(nums[mid]<target && target<=nums[right]) left = mid+1;
            else right = mid-1;
        }else{
            if(nums[left]<=target && target<nums[mid]) right =mid-1;
            else left = mid+1;
        }
    }
    return -1;
}
```

## Construct Binary Search Tree from Preorder Traversal
Return the root node of a binary search tree that matches the given preorder traversal.
### Example
```
Input: [8,5,1,7,10,12]
Output: [8,5,10,1,7,null,12]
```
### Code
```cpp
TreeNode* bstFromPreorder(vector<int>& preorder) {
    if(preorder.empty()) return NULL;
    return build(preorder,0,preorder.size()-1);
}
TreeNode* build(vector<int>& preorder, int front, int end){
    if (front>end) return NULL;
    TreeNode* root = new TreeNode(preorder[front]);
    int mid;
    for(mid = front+1; mid<end+1; mid++){
        if(preorder[front]<preorder[mid]) break;
    }
    root->left = build(preorder,front+1,mid-1);
    root->right = build(preorder,mid,end);
    return root;
}
```

## Leftmost Column with at Least a One
A binary matrix means that all elements are 0 or 1. For each individual row of the matrix, this row is sorted in non-decreasing order.  
Given a row-sorted binary matrix binaryMatrix, return leftmost column index(0-indexed) with at least a 1 in it. If such index doesn't exist, return -1.
### Example
```
Input: mat = [[0,0,0,1],[0,0,1,1],[0,1,1,1]]
Output: 1
```
### Code
```cpp
int leftMostColumnWithOne(BinaryMatrix &binaryMatrix) {
    vector<int> dim = binaryMatrix.dimensions();
    int row=0,col=dim[1]-1;
    while(row<dim[0]&&col>-1){
        if (binaryMatrix.get(row,col) == 1){
            col--;
        }else row++;
    }
    return (col==dim[1]-1)?-1:col+1;
}
```

## Subarray Sum Equals K
Given an array of integers and an integer k, you need to find the total number of continuous subarrays whose sum equals to k.
### Example
```
Input:nums = [1,1,1], k = 2
Output: 2
```
### Code
```cpp
int subarraySum(vector<int>& nums, int k) {
    unordered_map<int,int> m{{0,1}};
    int res = 0, sum=0, n = nums.size();
    for (auto num : nums){
        sum += num;
        res += m[sum-k];
        ++m[sum];
    }
    return res;
}
```

## Bitwise AND of Numbers Range
Given a range [m, n] where 0 <= m <= n <= 2147483647, return the bitwise AND of all numbers in this range, inclusive.
### Example
```
Input: [5,7]
Output: 4
```
### Code
```cpp
int rangeBitwiseAnd(int m, int n) {
    int i =0;
    while(m!=n){
        m>>=1;
        n>>=1;
        i++;
    }
    return (m<<i);
}
```

## LRU Cache
Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: get and put.

`get(key)` - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
`put(key, value)` - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

The cache is initialized with a positive capacity.
### Example
```
LRUCache cache = new LRUCache( 2 /* capacity */ );
cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```
### Code
```cpp
class LRUCache {
private:
    unordered_map<int,list<pair<int,int>>::iterator> hash_;
    list<pair<int,int>> ls_;
    int capacity_;
public:
    LRUCache(int capacity) {
        capacity_ = capacity;
    }
    
    int get(int key) {
        if(hash_.find(key) == hash_.end()){
            return -1;
        }else{
            int val = hash_[key]->second;
            ls_.erase(hash_[key]);
            ls_.push_front(make_pair(key,val));
            hash_[key] = ls_.begin();
            return val;
        }
    }
    
    void put(int key, int value) {
        if(hash_.find(key)!=hash_.end()){
            ls_.erase(hash_[key]);
        }else if(hash_.size()>=capacity_){
            hash_.erase(ls_.back().first);
            ls_.pop_back();
        }
        ls_.push_front(make_pair(key,value));
        hash_[key] = ls_.begin();
    }
};
```

## Jump Game
Given an array of non-negative integers, you are initially positioned at the first index of the array.  
Each element in the array represents your maximum jump length at that position.  
Determine if you are able to reach the last index.
### Example
```
Input: [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum
             jump length is 0, which makes it impossible to reach the last index.
```
### Code
```cpp
bool canJump(vector<int>& nums) {
    int n = nums.size(),reach = 0;
    for(int i =0; i<n; i++){
        if(i > reach || reach >= n-1) break;
        reach = max(reach,i+nums[i]);
    }
    return reach >= n-1;
}
```

## Longest Common Subsequence
Given two strings text1 and text2, return the length of their longest common subsequence.  
A subsequence of a string is a new string generated from the original string with some characters(can be none) deleted without changing the relative order of the remaining characters. (eg, "ace" is a subsequence of "abcde" while "aec" is not). A common subsequence of two strings is a subsequence that is common to both strings.  
If there is no common subsequence, return 0.
### Example
```
Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
```
### Code
```cpp
int longestCommonSubsequence(string text1, string text2) {
    int m = text1.size();
    int n = text2.size();
    vector<vector<int>> dp(m+1,vector<int>(n+1,0));
    for(int i = 1; i<m+1; i++){
        for(int j = 1; j<n+1; j++){
            if(text1[i-1] == text2[j-1]){
                dp[i][j] = dp[i-1][j-1]+1;
            }else{
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
            }
        }
    }
    return dp[m][n];
}
```

## Maximal Square
Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.
### Example
```
Input: 
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
Output: 4
```
### Code
```cpp
int maximalSquare(vector<vector<char>>& matrix) {
    if(matrix.empty()||matrix[0].empty()) return 0;
    int m = matrix.size(), n=matrix[0].size(), res = 0;
    vector<vector<int>> dp(m,vector<int>(n,0));
    for(int i=0; i<m; i++){
        for(int j=0; j<n; j++){
            if(i == 0 || j == 0){
                dp[i][j] = matrix[i][j] - '0';
            }else if(matrix[i][j] == '1'){
                dp[i][j] = min(dp[i-1][j-1],min(dp[i-1][j],dp[i][j-1]))+1;
            }
            res = max(res,dp[i][j]);
        }
    }
    return res*res;
}
```

## First Unique Number
You have a queue of integers, you need to retrieve the first unique integer in the queue.  
Implement the FirstUnique class:  
- FirstUnique(int[] nums) Initializes the object with the numbers in the queue.
- int showFirstUnique() returns the value of the first unique integer of the queue, and returns -1 if there is no such integer.
- void add(int value) insert value to the queue.
### Example
```
Input: 
["FirstUnique","showFirstUnique","add","showFirstUnique","add","showFirstUnique","add","showFirstUnique"]
[[[2,3,5]],[],[5],[],[2],[],[3],[]]
Output: 
[null,2,null,2,null,3,null,-1]

Explanation: 
FirstUnique firstUnique = new FirstUnique([2,3,5]);
firstUnique.showFirstUnique(); // return 2
firstUnique.add(5);            // the queue is now [2,3,5,5]
firstUnique.showFirstUnique(); // return 2
firstUnique.add(2);            // the queue is now [2,3,5,5,2]
firstUnique.showFirstUnique(); // return 3
firstUnique.add(3);            // the queue is now [2,3,5,5,2,3]
firstUnique.showFirstUnique(); // return -1
```
### Code
```cpp
class FirstUnique {
private:
    unordered_map<int,list<int>::iterator> hash_;
    unordered_map<int,int> count_;
    list<int> ls_;
public:
    FirstUnique(vector<int>& nums) {
        for(int num : nums){
            add(num);
        }
    }
    int showFirstUnique() {
        if(ls_.empty()){
            return -1;
        }else{
            return ls_.front();
        }
    }
    void add(int value) {
        if(hash_.find(value) == hash_.end()){
            list<int>::iterator it = ls_.insert(ls_.end(),value);
            hash_[value]=it;
            count_[value] = 0;
        }else if(count_[value] == 0){
            ls_.erase(hash_[value]);
            count_[value] = 1;
        }
    }
};
```

## Binary Tree Maximum Path Sum
Given a non-empty binary tree, find the maximum path sum.  
For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain at least one node and does not need to go through the root.
### Example
```
Input: [-10,9,20,null,null,15,7]

   -10
   / \
  9  20
    /  \
   15   7

Output: 42
```
### Code
```cpp
    int helper(TreeNode* root, int &maxSum){
        if(root == NULL) return 0;
        int left = 0, right =0;
        if(root->left) left = helper(root->left,maxSum);
        if(root->right) right = helper(root->right,maxSum);
        int leftOrRight = max(root->val, root->val+max(left,right));
        maxSum = max(maxSum,max(left+right+root->val, leftOrRight));
        return leftOrRight;
        
    }
    int maxPathSum(TreeNode* root) {
        int maxSum = INT_MIN;
        helper(root, maxSum);
        return maxSum;
    }
```


## Check If a String Is a Valid Sequence from Root to Leaves Path in a Binary Tree
Given a binary tree where each path going from the root to any leaf form a valid sequence, check if a given string is a valid sequence in such binary tree.   
We get the given string from the concatenation of an array of integers arr and the concatenation of all values of the nodes along a path results in a sequence in the given binary tree.
### Example
```
Input: root = [0,1,0,0,1,0,null,null,1,0,0], arr = [0,1,0,1]
Output: true
Explanation: 
The path 0 -> 1 -> 0 -> 1 is a valid sequence (green color in the figure). 
Other valid sequences are: 
0 -> 1 -> 1 -> 0 
0 -> 0 -> 0
```
### Code
```cpp
bool helper(TreeNode* root, vector<int>& arr, int index){
    if(root==NULL) return false;
    if(arr[index] != root->val) return false;
    if(index==arr.size()-1){
        return root->left==NULL && root->right==NULL;
    }
    return helper(root->left, arr, index+1) || helper(root->right, arr, index+1);
}
bool isValidSequence(TreeNode* root, vector<int>& arr) {
    return helper(root,arr,0);
}
```