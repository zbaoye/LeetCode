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