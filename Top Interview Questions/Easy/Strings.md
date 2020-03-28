## 1. Reverse Integer
Given a 32-bit signed integer, reverse digits of an integer.
### Example
```
Input: 123
Output: 321
```
### Code
```c++
int reverse(int x) {
    int result = 0;
    while(x != 0){
        int pop = x%10;
        x = x/10;
        if(result>INT_MAX/10 || (result == INT_MAX/10 && pop>7)) return 0;
        if(result<INT_MIN/10 || (result == INT_MIN/10 && pop<-8)) return 0;
        result = result*10 + pop;
    }
    return result;
}
```

## 2. First Unique Character in a String
Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.
### Example
```
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.
```
### Code
```c++
int firstUniqChar(string s) {
    int list[26] = {};
    
    for(int i =0; i<s.size(); i++){
        list[s[i] - 'a']++;
    }
    for(int i =0; i<s.size(); i++){
        if(list[s[i]-'a'] == 1){
            return i;
        }
    }
    return -1;
}
```

## 3. Valid Anagram
Given two strings s and t , write a function to determine if t is an anagram of s.
### Example
```
Input: s = "anagram", t = "nagaram"
Output: true
```
### Code
```c++
bool isAnagram(string s, string t) {
    int list[26] = {0};
    for(auto c:s){
        list[c-'a']++;
    }
    for(auto c:t){
        list[c-'a']--;
    }
    for(auto i:list)
        if(i != 0)
            return false;
    return true;
}
```

## 4.  String to Integer (atoi)
Implement atoi which converts a string to an integer.  
The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.  
The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.  
If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.  
If no valid conversion could be performed, a zero value is returned.
### Example
```
Input: "4193 with words"
Output: 4193
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.
```
### Code
```c++
int myAtoi(string str) {
    int startIndex = 0;
    int n = str.size();
    while(startIndex < n && str[startIndex] == ' '){
        startIndex++;
    }
    bool flag = false;
    int neg = 1;
    int result=0;
    for(int i=startIndex; i<n; i++){
        if(str[i] =='-' || str[i]=='+'){
            if(flag) break;
            if(str[i]=='-'){
                neg = -1;   
            }
            flag = true;
        }else if(str[i]>='0'&&str[i]<='9'){
            flag = true;
            int temp = str[i]-'0';
            if(result>INT_MAX/10 || (result==INT_MAX/10 && temp>7)) return INT_MAX;
            if(result<INT_MIN/10 || (result==INT_MIN/10 && temp>8)) return INT_MIN;
            result = result*10 + neg*temp;
        }else{
            break;
        }
    }
    return result;
}
```

## 5. Implement strStr()
Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.
### Example 
```
Input: haystack = "hello", needle = "ll"
Output: 2
```
### Code
```c++
int strStr(string haystack, string needle) {
    if(needle.empty()) return 0;
    int stackSize = haystack.size();
    int needleSize = needle.size();
    for(int i=0; i<stackSize-needleSize+1; i++){
        int j=0;
        for(; j<needleSize; j++){
            if(haystack[i+j] != needle[j]) break;
        }
        if(j == needleSize) return i;
    }
    return -1;
}
```

## 6. Count and Say
The count-and-say sequence is the sequence of integers with the first five terms as following:
### Example
```
1.     1
2.     11
3.     21
4.     1211
5.     111221
```
### Code
```c++
string parse(string input){
    string result="";
    char pre_char = input[0];
    int count = 0;
    for (int i=0; i<input.size(); i++){
        if( input[i] == pre_char){
            count++;
        }else{
            result.push_back(count+'0');
            result.push_back(pre_char);
            pre_char = input[i];
            count=1;
        }
    }
    result.push_back(count+'0');
    result.push_back(pre_char);
    return result;
}
string countAndSay(int n) {
    if(n == 1) return "1";
    string pre = countAndSay(n-1);
    return parse(pre);
}
```

## 7. Longest Common Prefix
Write a function to find the longest common prefix string amongst an array of strings.  
If there is no common prefix, return an empty string "".
### Example
```
Input: ["flower","flow","flight"]
Output: "fl"
```
### Code
```c++
string getComm(string s1, string s2){
    int size = (s1.size()>s2.size())?s2.size():s1.size();
    string result = "";
    for (int i=0; i<size; i++){
        if(s1[i] == s2[i]){
            result.push_back(s1[i]);
        }else{
            return result;
        }
    }
    return result;
}
string longestCommonPrefix(vector<string>& strs) {
    if(strs.empty()) return "";
    int size = strs.size();
    string common = strs[0];
    for (int i=1; i<size; i++){
        common = getComm(common,strs[i]);
        if(common == ""){
            return "";
        }
    }
    return common;
}
```