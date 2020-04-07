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