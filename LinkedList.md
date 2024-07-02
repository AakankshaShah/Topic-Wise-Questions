1. Add two numbers
   ```
    ListNode* temp=dummy;
        int carry=0;
        while(l1!=NULL || l2!=NULL || carry){
            int sum=0;
            if(l1!=NULL){
                sum+=l1->val;
                l1=l1->next;
            }
            if(l2!=NULL){
                sum+=l2->val;
                l2=l2->next;
            }
            sum+=carry;
            carry=sum/10;
            ListNode* newnode=new ListNode(sum%10);
            temp->next=newnode;
            temp=temp->next;
        }
        return dummy->next;
   ```
2. Remove nth node from end (K-N+1) two pointer approach
   ```
     ListNode* fast = head;
        ListNode* slow = head;

        for (int i = 0; i < n; i++) {
            fast = fast->next;
        }

        if (fast == NULL) {
            head = head->next;
            return head;
        }

        while (fast->next != NULL) {
            fast = fast->next;
            slow = slow->next;
        }
        slow->next = slow->next->next;
        return head;
   ```
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
   ```
    ListNode* dummy = new ListNode();
        ListNode* temp = dummy;

        while (list1 != NULL && list2 != NULL) {
            if (list1->val <= list2->val) {
                dummy->next = list1;

                list1 = list1->next;

            } else {
                dummy->next = list2;

                list2 = list2->next;
            }
            dummy = dummy->next;
        }

        if (list1 != NULL) {
            dummy->next = list1;
        }

        if (list2 != NULL) {
            dummy->next = list2;
        }

        return temp->next;
   ```
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
10. Linked list cycle fast & slow pointers
    ```
      ListNode *fast = head;
        ListNode *slow = head;
        
		// till fast and fast-> next not reaches NULL
		// we will increment fast by 2 step and slow by 1 step
        while(fast != NULL && fast ->next != NULL)
        {
            fast = fast->next->next;
            slow = slow->next;
            
			
			// At the point if fast and slow are at same address
			// this means linked list has a cycle in it.
            if(fast == slow)
                return true;
        }
        
		// if traversal reaches to NULL this means no cycle.
        return false;
    ```
11. Reorder list

     ```
      ListNode* reverse(ListNode* head) {
        ListNode* prev = NULL;
        ListNode* curr = head;
        ListNode* nxt = NULL;

        while (curr) {
            nxt = curr->next;
            curr->next = prev;
            prev = curr;
            curr = nxt;
        }
        return prev;
    }
    void reorderList(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head->next;

        while (fast and fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }

        // step 2 - reverse the second half and split the List into two.
        ListNode* second = reverse(slow->next); // independent list second
        slow->next = NULL;
        ListNode* first = head; // independent list first

        // step 3 - merging the two list
        // second list can be shorter when LL size is odd
        while (second) {
            ListNode* temp1 = first->next;
            ListNode* temp2 = second->next;
            first->next = second;
            second->next = temp1;
            first = temp1;
            second = temp2;
        }
     ```
12. Double a number shows as linked list
     ```
        istNode* prev=NULL;
        if(head->val>=5)
        head= new ListNode(0,head);
        prev=head;
        ListNode* curr=head;
        while(curr!=NULL)
        {
            if(curr->val>=5)
            {
                prev->val++;
                curr->val=curr->val*2-10;
            }
            else
            {
                curr->val=curr->val*2;
            }
            prev=curr;
            curr=curr->next;
        }
        return head;
     ```
13. Delete duplicates
    ```
       var deleteDuplicates = function (head) {
    let current = head;
    while (current != null && current.next != null) {

        if (current.val == current.next.val) {
            current.next = current.next.next;
        } else {
            current = current.next;
        }
    }

    return head;
    }
    ```
14. Rotate List 
     ```
       ListNode* rotateRight(ListNode* head, int k) {
        if (head == NULL || head->next == NULL || k == 0)
            return head;
        int len = 1;
        ListNode* temp = head;
        while (temp->next != NULL) {
            len++;
            temp = temp->next;
        }
        k = k % len;
        k = len - k;
        temp->next = head;
        while (k-- > 0) {
            temp = temp->next;
        }
        head = temp->next;
        temp->next = NULL;
        return head;
    }
     ```
15. Reverse List II
     ```
          ListNode* reverseBetween(ListNode* head, int left, int right) {
        if (head == NULL || head->next == NULL)
            return head;
        if (left == right)
            return head;

        ListNode* dummy =
            new ListNode(0); 
        dummy->next = head;
        ListNode* prev = dummy;
        for (int i = 1; i < left; i++) {
            prev = prev->next;
        }

        ListNode* curr = prev->next;
        ListNode* next = curr->next;
        // for (int i = 0; i < right - left; i++) {
        //     curr->next = next->next;
        //     next->next = prev->next;
        //     prev->next = next;
        //     next = curr->next;
        // }
        for(int i = 1; i<=right-left; i++) {
            
            ListNode* temp = prev->next; //0
            prev->next = curr->next; //1
            curr->next = curr->next->next; //2
            prev->next->next = temp; //3
            
        }
    
        return dummy->next;
    }
    }
     ```

     ```
        ListNode* reverseBetween(ListNode* head, int left, int right) {
        
        if(!head || !head->next)
            return head;
        
        stack<ListNode*> st;
        ListNode* dummy = new ListNode(0); //to which prev points in the beginning (Example : 1 -> 2)
        dummy->next = head;
        ListNode* prev = dummy;
        
        for(int i = 1; i<=left-1; i++) {
            prev = prev->next;
        }
        
        //put [left, right] elements on stack
        ListNode* curr = prev->next;
        for(int i = left; i<= right; i++) {
            st.push(curr);
            curr = curr->next;
        }
        
        ListNode* storeRightNext = st.top()->next;
        
        //Now, link them
        while(!st.empty()) {
            prev->next = st.top();
            st.pop();
            prev = prev->next;
        }
        
        prev->next = storeRightNext;
        return dummy->next;
    }
     ```
16. Copy List with random Pointer
    ```
       Node* copyRandomList(Node* head) {
        if (!head)
            return NULL;
        map<Node*, Node*> mp;
        Node* curr = head;
        Node* prev = NULL;
        Node* newHead = NULL;
        while (curr) {
            Node* temp = new Node(curr->val);
            mp[curr] = temp;
            if (newHead == NULL) {
                newHead = temp;
                prev = newHead;
            } else {
                prev->next = temp;
                prev = temp;
            }
            curr = curr->next;
        }

        curr = head;
        Node* newCurr = newHead;
        while (curr) {
            if (curr->random == NULL) {
                newCurr->random = NULL;
            } else {
                newCurr->random = mp[curr->random];
            }

            newCurr = newCurr->next;
            curr = curr->next;
        }
        return newHead;
    }
    ```
    ```
    if (!head)
            return NULL;

        Node* curr = head;

        while (curr) {
            Node* currNext = curr->next;
            curr->next = new Node(curr->val);
            curr->next->next = currNext;
            curr = currNext;
        }

        // Deep copy of random pointers
        curr = head;
        while (curr && curr->next) {
            if (curr->random == NULL) {
                curr->next->random = NULL;
            } else {
                curr->next->random = curr->random->next;
            }
            curr = curr->next->next;
        }

        // Deep copy of next pointers and recovering old linked list
        Node* newHead = head->next;
        Node* newCurr = newHead;
        curr = head;
        while (curr && newCurr) {
            curr->next = curr->next ? curr->next->next : NULL;

            newCurr->next = newCurr->next ? newCurr->next->next : NULL;

            curr = curr->next;
            newCurr = newCurr->next;
        }

        return newHead;
    }
    ```
17. Middle of list 

    ```
      ListNode* middleNode(ListNode* head) {
        ListNode* fast=head;
        ListNode* slow=head;

        while(fast&&fast->next)
        {
            fast=fast->next->next;
            slow=slow->next;
        }

        return slow;

        
    }
    ```
18. Linked List Cycle 
    ```
      bool hasCycle(ListNode *head) {
        ListNode *fast = head;
        ListNode *slow = head;
        
		// till fast and fast-> next not reaches NULL
		// we will increment fast by 2 step and slow by 1 step
        while(fast != NULL && fast ->next != NULL)
        {
            fast = fast->next->next;
            slow = slow->next;
            
			
			// At the point if fast and slow are at same address
			// this means linked list has a cycle in it.
            if(fast == slow)
                return true;
        }
        
		// if traversal reaches to NULL this means no cycle.
        return false;
        
    }
    ```
19. Linked List Cycle II

     ```
      ListNode *detectCycle(ListNode *head) {
        ListNode*fast=head;
        ListNode*slow=head;
        while(fast!=NULL && fast->next!=NULL){
            slow=slow->next;
            fast=fast->next->next;
            if(fast==slow) break;
        }
        if(fast==NULL || fast->next==NULL) return NULL;
        ListNode*temp=head;
        while(temp!=slow){
            temp=temp->next;
            slow=slow->next;
            if(temp==slow) break;
        }
        return temp;
        
    }
      
     ```
20. Sort List
    ```
      ListNode* sortList(ListNode* head) {
        
       
        if(head == NULL || head ->next == NULL)
            return head;
        
        
        ListNode *temp = NULL;
        ListNode *slow = head;
        ListNode *fast = head;
        
        
        while(fast !=  NULL && fast -> next != NULL)
        {
            temp = slow;
            slow = slow->next;          //slow increment by 1
            fast = fast ->next ->next;  //fast incremented by 2
            
        }   
        temp -> next = NULL;            //end of first left half
        
        ListNode* l1 = sortList(head);    //left half recursive call
        ListNode* l2 = sortList(slow);    //right half recursive call
        
        return mergelist(l1, l2);         //mergelist Function call
            
    }
        
    ListNode* mergelist(ListNode *l1, ListNode *l2)
    {
        ListNode *ptr = new ListNode(0);
        ListNode *curr = ptr;
        
        while(l1 != NULL && l2 != NULL)
        {
            if(l1->val <= l2->val)
            {
                curr -> next = l1;
                l1 = l1 -> next;
            }
            else
            {
                curr -> next = l2;
                l2 = l2 -> next;
            }
        
        curr = curr ->next;
        
        }
        
        //for unqual length linked list
        
        if(l1 != NULL)
        {
            curr -> next = l1;
            l1 = l1->next;
        }
        
        if(l2 != NULL)
        {
            curr -> next = l2;
            l2 = l2 ->next;
        }
        
        return ptr->next;
    }
    ```
21. LRU Cache

     ```
       class node {
    public:
        int key;
        int val;
        node* next;
        node* prev;
        node(int _key, int _val) {
            key = _key;
            val = _val;
        }
    };
    unordered_map<int, node*> m;
    int capa;
    node* head = new node(-1, -1);
    node* tail = new node(-1, -1);
    LRUCache(int capacity) {
        capa = capacity;
        head->next = tail;
        tail->prev = head;
    }
    void addNode(node* newnode) { // Add after -1;
        // Add in start
        node* temp = head->next;
        newnode->next = temp;
        newnode->prev = head;
        head->next = newnode;
        temp->prev = newnode;
    }

    void deleteNode(node* delnode) { // Delete from end
        node* tem = delnode->next;
        node* temp = delnode->prev;
        temp->next = tem;
        tem->prev = temp;
    }

    int get(int key) {

        if (m.find(key) != m.end()) {
            node* res = m[key];
            int result = res->val;
            m.erase(key);
            deleteNode(res);
            addNode(res);
            m[key] = head->next;
            return result;

        } else
            return -1;
    }

    void put(int key, int value) {
        if (m.find(key) != m.end()) {
            node* resn = m[key];
            m.erase(key);
            deleteNode(resn);
        }
        if (m.size() == capa) {
            m.erase(tail->prev->key);
            deleteNode(tail->prev);
        }
        addNode(new node(key, value));
        m[key] = head->next;
    }
    };
     ```
