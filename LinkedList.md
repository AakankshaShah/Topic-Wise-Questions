1. Add two numbers
2. Remove nth node from end (K-N+1) two pointer approach
3.  Reverse linked list 
     ```
      Node* current = head;
        Node *prev = NULL, *next = NULL;
 
        while (current != NULL) {
            // Store next
            next = current->next;
            // Reverse current node's pointer
            current->next = prev;
            // Move pointers one position ahead.
            prev = current;
            current = next;
        }
        head = prev;

      ```
4. Merge two sorted linked list
5. Merge k sorted linked lists
    ```
    ListNode* mergeTwoSortedLists(ListNode* l1, ListNode* l2) {
        if (!l1)
            return l2;
        if (!l2)
            return l1;

        if (l1->val <= l2->val) {
            l1->next = mergeTwoSortedLists(l1->next, l2);
            return l1;
        } else {
            l2->next = mergeTwoSortedLists(l1, l2->next);
            return l2;
        }

        return NULL;
    }

    ListNode* partitionAndMerge(int start, int end, vector<ListNode*>& lists) {
        if (start == end)
            return lists[start];

        if (start > end)
            return NULL;

        int mid = start + (end - start) / 2;

        ListNode* l1 = partitionAndMerge(start, mid, lists);
        ListNode* l2 = partitionAndMerge(mid + 1, end, lists);

        return mergeTwoSortedLists(l1, l2);
    }
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        int n = lists.size();

        if (n == 0)
            return NULL;

        return partitionAndMerge(0, n - 1, lists);
    }
 
   ```
6. Swap pairs 
    ```
     if (head == NULL || head->next == NULL)
            return head;

        ListNode* prev = head;
        ListNode* curr = head->next;
        head = curr;

        while (true) {
            ListNode* next = curr->next;

            curr->next = prev;

            if (next == NULL || next->next == NULL) {
                prev->next = next;
                break;
            }

            prev->next = next->next;
            prev = next;
            curr = prev->next;
        }

        return head;
     ```
7. Reverse in k groups 
    ```
    ListNode* reverseList(ListNode* head, int k) {
        ListNode* prev = NULL;
        ListNode* cur = head;
        ListNode* next;
        int count=0;
        while (current != NULL && count < k) {
            next = cur->next;
            cur->next = prev;
            prev = cur;
            cur = next;
            count++;
    
        }
        if(next!=null)
         head->next = reverse(next, k); 
        
        return prev;
    }

     ```
8. Reverse in k groups only 
    ```
     ListNode* reverseList(ListNode* head, int k) {
        ListNode* prev = NULL;
        ListNode* cur = head;
        ListNode* next;
        while (k--) {
            next = cur->next;
            cur->next = prev;
            prev = cur;
            cur = next;
        }
        head->next = cur;
        return prev;
    }

   public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        int len = 0;
        ListNode* length = head;
        while (length != NULL) {
            len++;
            length = length->next;
        }
        ListNode* NewList = new ListNode();
        ListNode* temp = NewList;
        ListNode* cur = head;
        while (cur != NULL) {
            if (len >= k) {
                temp->next = reverseList(cur, k);
                len -= k;
                temp = cur;
                cur = cur->next;
            } else {
                break;
            }
        }
        return NewList->next;
    }
    ```
9. Partition list
