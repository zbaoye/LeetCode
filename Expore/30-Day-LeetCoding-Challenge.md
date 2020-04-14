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