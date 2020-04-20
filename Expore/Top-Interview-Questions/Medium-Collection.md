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

## Longest Substring Without Repeating Characters
Given a string, find the length of the longest substring without repeating characters.
### Example
```
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```
### Code
```cpp
int lengthOfLongestSubstring(string s) {
    int i=0,j=0,ans=0;
    int n =s.size();
    int maxLen = 0;
    unordered_set<char> char_set;
    while(i<n && j<n){
        if(!char_set.count(s[j])){
            char_set.insert(s[j++]);
            maxLen = max(maxLen, j-i);
        }else{
            char_set.erase(char_set.find(s[i++]));
        }
    }
    return maxLen;
}
```

## Longest Palindromic Substring
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.
### Example
```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```
### Code
Star
#### Solution One
*Dynamic Programming*
```cpp
string longestPalindrome(string s) {
    int len = s.size();
    vector<vector<bool> > dp(len,vector<bool>(len,false));
    int max = 0;
    int left = 0;
    string result = s.substr(0,1);
    
    for (int i = 0; i<len; i++){
        dp[i][i] = true;
    }
    for (int i =0; i<len-1; i++){
        dp[i][i+1]  = s[i]==s[i+1];
        if(dp[i][i+1] == true){
            result = s.substr(i,2);
        }
    }
    for (int k=3; k<=len; k++){
        for(int i =0; i<=len-k; i++){
            int j = i+k-1;
            dp[i][j] = dp[i+1][j-1] && s[i]==s[j];
            if(dp[i][j] && (j-i+1)>result.size()){
                result = s.substr(i,k);
            }
        }
    }
    return result;
}
```
#### Solution Two
`TODO`

## Increasing Triplet Subsequence
Given an unsorted array return whether an increasing subsequence of length 3 exists or not in the array.
Formally the function should:
> Return true if there exists i, j, k
> such that arr[i] < arr[j] < arr[k] given 0 ≤ i < j < k ≤ n-1 else return false.
### Example
```
Input: [1,2,3,4,5]
Output: true
```
### Code
```cpp
bool increasingTriplet(vector<int>& nums) {
    int first = INT_MAX;
    int second = INT_MAX;
    for(int i:nums){
        if(i<=first) first = i;
        else if(i<=second) second = i;
        else return true;
    }
    return false;
}
```

## Add Two Numbers
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.
### Example
```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```
### Code
```cpp
ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
    ListNode* head = new ListNode(0);
    ListNode* cur = head;
    ListNode* pre = head;
    int sum = 0;
    while(l1 != NULL || l2 != NULL){
        sum = cur->val + ((l1!=NULL)?l1->val:0) + ((l2!=NULL)?l2->val:0);
        cur -> val = sum%10;
        cur -> next = new ListNode(sum/10);
        pre = cur;
        cur = cur -> next;
        l1 = (l1 == NULL)?NULL:l1->next;
        l2 = (l2 == NULL)?NULL:l2->next;
    }
    if (cur -> val == 0) pre->next = NULL;
    return head;
}
```

## Odd Even Linked List
Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.
### Example
```
Input: 2->1->3->5->6->4->7->NULL
Output: 2->3->6->7->1->5->4->NULL
```
### Code
```cpp
ListNode* oddEvenList(ListNode* head) {
    if (head == NULL) return head;
    ListNode* odd = head; ListNode* even = odd->next; ListNode* even_head = even;
    while(even != NULL && even->next != NULL){
        odd -> next = even ->next;
        odd = odd->next;
        even -> next =odd ->next;
        even = even ->next;
    }
    odd -> next = even_head;
    return head;
}
```

## Intersection of Two Linked Lists
Write a program to find the node at which the intersection of two singly linked lists begins.
### Example
```
Input: intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
Output: Reference of the node with value = 2
Input Explanation: The intersected node's value is 2 (note that this must not be 0 if the two lists intersect).   
From the head of A, it reads as [0,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.
```
### Code
```cpp
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
    ListNode* a = headA;
    ListNode* b = headB;
    if (headA == NULL || headB == NULL) return NULL;
    while (a!=b){
        if(a == NULL) a = headB;
        else a = a->next;
        if(b == NULL) b = headA;
        else b = b->next;
    }
    return a;
}
```

## Binary Tree Inorder Traversal
Given a binary tree, return the inorder traversal of its nodes' values.
### Example
```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,3,2]
```
### Code
```cpp
vector<int> inorderTraversal(TreeNode* root) {
    stack<TreeNode*> s;
    vector<int> result;
    TreeNode* cur =root;
    while(cur != NULL || !s.empty()){
        while(cur != NULL){
            s.push(cur);
            cur = cur->left;
        }
        cur = s.top();
        s.pop();
        result.push_back(cur->val);
        cur = cur ->right;
    }
    return result;
}
```

## Binary Tree Zigzag Level Order Traversal
Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).
### Example
Given binary tree `[3,9,20,null,null,15,7]`,
```
    3
   / \
  9  20
    /  \
   15   7
```
return its zigzag level order traversal as:
```
[
  [3],
  [20,9],
  [15,7]
]
```
### Code
```cpp
vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
    vector<vector<int>> result;
    if(root == NULL) return result;
    deque<TreeNode*> q;
    q.push_back(root);
    bool zig = true;
    TreeNode* node_temp;
    while(!q.empty()){
        int size_q = q.size();
        vector<int> vec_temp;
        while(size_q){
            if(zig){
                node_temp = q.front();
                q.pop_front();
                if(node_temp->left) q.push_back(node_temp->left);
                if(node_temp->right) q.push_back(node_temp->right);
            }else{
                node_temp = q.back();
                q.pop_back();
                if(node_temp->right) q.push_front(node_temp->right);
                if(node_temp->left) q.push_front(node_temp->left);
            }
            vec_temp.push_back(node_temp->val);
            size_q--;
        }
        result.push_back(vec_temp);
        zig = !zig;
    }
    return result;
}
```

## Construct Binary Tree from Preorder and Inorder Traversal
Given preorder and inorder traversal of a tree, construct the binary tree.
### Example
```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```
### Code
```cpp
TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
    if(preorder.empty()||inorder.empty()) return NULL;
    return buildHelper(preorder, 0, preorder.size()-1, inorder, 0, inorder.size()-1);
}

TreeNode* buildHelper(vector<int>& preorder, int pFront, int pEnd, vector<int>& inorder, int iFront, int iEnd){
    if(pFront>pEnd||iFront>iEnd) return NULL;
    TreeNode* root = new TreeNode(preorder[pFront]);
    int index=0;
    while(preorder[pFront] != inorder[iFront+index]) index++;
    
    root -> left = buildHelper(preorder,pFront+1,pFront+index, inorder,iFront,iFront+index-1);
    root -> right = buildHelper(preorder,pFront+index+1,pEnd, inorder,iFront+index+1,iEnd);
    return root;
}
```