[力扣19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

### 1.遍历两遍

~~~c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if(head==nullptr || n<1){ 
            return head;
        }
        ListNode *curr = head;
        while(curr!=nullptr){ //遍历第一遍
            n--;
            curr = curr->next;
        }
        if(n==0){
            head = head->next;
        }
        if(n<0){
            curr = head;
            n++;
            while(n != 0){   //遍历第二遍
                n++;
                curr = curr->next;
            }
            // n==0 时curr是倒数第K+1个节点
            curr->next = curr->next->next; 
        }
        return head;
    }
};
~~~

##### 时间 ：O(N)

##### 空间 ：O(1)



### 2.遍历一遍

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* first = head;
        ListNode* second = dummyHead;
        for(int i=0; i<n; i++){
            first = first->next;
        }
        while(first){
            first = first->next;
            second = second->next;
        }
        second->next = second->next->next;
        ListNode* ans = dummyHead->next;
        delete dummyHead;
        return ans;
    }
};
```

##### 时间 ：O(N)

##### 空间 ：O(1)