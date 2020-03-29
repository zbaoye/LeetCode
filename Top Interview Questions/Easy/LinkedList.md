## 1. Delete Node in a Linked List
Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.  
Given linked list -- head = [4,5,1,9], which looks like following:
### Example
```
Input: head = [4,5,1,9], node = 5
Output: [4,1,9]
Explanation: You are given the second node with value 5, the linked list should become 4 -> 1 -> 9 after calling your function.
```
### Code
```cpp
void deleteNode(ListNode* node) {
    node->val = node->next->val;
    node->next = node->next->next;
}
```

## 2. Remove Nth Node From End of List
Given a linked list, remove the n-th node from the end of list and return its head.
### Example
Given linked list: 1->2->3->4->5, and n = 2. 
After removing the second node from the end, the linked list becomes 1->2->3->5.
### Code
```cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
    ListNode* newHead = new ListNode(0);
    newHead->next = head;
    ListNode* fast = newHead;
    ListNode* slow = newHead;
    for(int i=0; i<n+1; i++){
        fast = fast->next;
    }
    while(fast != NULL){
        fast = fast->next;
        slow = slow->next;
    }
    slow ->next = slow->next->next;
    return newHead->next;   
}
```

## 3. Reverse Linked List
Reverse a singly linked list.
### Example
```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```
### Code
```cpp
ListNode* reverseList(ListNode* head) {
    if (head == NULL) return head;
    ListNode* newHead = new ListNode(0);
    ListNode* cur = head;
    while(head != NULL){
        head = head->next;
        cur->next = newHead->next;
        newHead->next = cur;
        cur = head;
    }
    return newHead->next;
}
```

## 4. Merge Two Sorted Lists
Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.
### Example
```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```
### Code
```cpp
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    if (l1 == NULL) return l2;
    if (l2 == NULL) return l1;
    
    ListNode *mergedList = new ListNode(0);
    ListNode *firstNode = mergedList;
    while(l1!=NULL && l2!=NULL){
        if(l1->val <= l2->val){
            mergedList -> next = l1;
            l1 = l1->next;
        }else{
            mergedList -> next = l2;
            l2 = l2->next;
        }
        mergedList = mergedList->next;
    }
    if(l1==NULL){
        mergedList ->next = l2;
    }
    if(l2==NULL){
        mergedList ->next = l1;
    }
    return firstNode->next;
}
```

## 5. Palindrome Linked List
Given a singly linked list, determine if it is a palindrome.
### Example
```
Input: 1->2->2->1
Output: true
```
### Code
```cpp
ListNode* reverse(ListNode* head){
    ListNode* pre = NULL;
    ListNode* cur = head;
    while(cur != NULL){
        ListNode* temp = cur ->next;
        cur ->next = pre;
        pre = cur;
        cur = temp;
    }
    return pre;
}
bool isPalindrome(ListNode* head) {
    if(head == NULL) return true;
    ListNode* fast = head;
    ListNode* slow = head;
    while(fast->next!=NULL && fast->next->next != NULL){
        slow = slow->next;
        fast = fast->next->next;
    }
    slow = reverse(slow->next);
    while(slow != NULL){
        if(slow->val != head->val){
            return false;
        }else{
            slow = slow->next;
            head = head->next;
        }
    }
    return true;
}
```

## 6. Linked List Cycle
Given a linked list, determine if it has a cycle in it.  
To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.
### Example
```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```
### Code
```cpp
bool hasCycle(ListNode *head) {
    if(head == NULL || head -> next == NULL) return false;
    
    ListNode* slow = head;
    ListNode* fast = head->next;
    while(fast!=slow){
        if(fast == NULL || fast->next == NULL){
            return false;
        }
        fast = fast -> next ->next;
        slow = slow -> next;
    }
    return true;
}
```