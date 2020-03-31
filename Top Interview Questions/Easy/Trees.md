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