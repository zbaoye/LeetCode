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