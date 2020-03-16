
## 437. Path Sum III

You are given a binary tree in which each node contains an integer value.
Find the number of paths that sum to a given value.
The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).
The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

### Solution

``` c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int tempSum = 0;
    int count = 0;
    int pathSum(TreeNode* root, int sum) {
        tempSum = sum;
        dfs(root);
        return count;
    }
    void dfs(TreeNode* root){
        if(root==NULL) return;
        dfs2(root,0);
        dfs(root->left);
        dfs(root->right);
    }
    void dfs2(TreeNode* root, int val){
        if(root==NULL) return;
        val+= root->val;
        if(val == tempSum){
            count++;
        }
        dfs2(root->left, val);
        dfs2(root->right, val);
    }
};
```