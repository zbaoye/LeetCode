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