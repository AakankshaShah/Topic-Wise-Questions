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
   ```
      public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode head = new ListNode();
        ListNode temp = head;
        int c = 0;

        while (l1 != null || l2 != null || c!=0) {
            int sum = 0;
            if (l1 != null) {
                sum += l1.val;
                l1 = l1.next;
            }
            if (l2 !=null) {
                sum += l2.val;
                l2 = l2.next;
            }
            sum = sum + c;

            ListNode temp2 = new ListNode(sum % 10);
            c = sum / 10;
            temp.next = temp2;
            temp = temp.next;
        }
        return head.next;
        
    }
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
    ```
       ListNode* partition(ListNode* head, int x) {
        ListNode *lesser = NULL;
        ListNode *greater = NULL;
        ListNode *startLesser = NULL;
        ListNode *startGreater = NULL;

        ListNode *prev;

        while(head != NULL){
            prev = head;
            head = head->next;
            prev->next = NULL;

            if(prev->val < x){
                if(lesser == NULL){
                    lesser = prev;
                    startLesser = lesser;
                }
                else{
                    lesser->next = prev;
                    lesser = lesser->next;
                }
            }

            else{
                if(greater == NULL){
                    greater = prev;
                    startGreater = greater;
                }
                else{
                    greater->next = prev;
                    greater = greater->next;
                }
            }

        }

        if(!startLesser) return startGreater;
        if(!startGreater) return startLesser;
        lesser->next = startGreater;

        return startLesser;
        
    }
    ```
10. Reorder list

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
11. Double a number shows as linked list
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
12. Delete duplicates
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
13. Rotate List 
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
14. Reverse List II
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
15. Copy List with random Pointer
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
    ```
     class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) {
            return null;
        }

        Map<Node, Node> mp = new HashMap<>();
        Node current = head;
        while (current != null) {
            mp.put(current, new Node(current.val));
            current = current.next;
        }
        current = head;
        while (current != null) {
            Node copy = mp.get(current);
            copy.next = mp.get(current.next);
            copy.random = mp.get(current.random);
            current = current.next;
        }

        return mp.get(head);

    }
    ```
16. Middle of list 

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
17. Linked List Cycle 
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
18. Linked List Cycle II

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
19. Sort List
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
20. LRU Cache

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
21. Delete Nodes From Linked List Present in Array
    ```
     ListNode* modifiedList(vector<int>& nums, ListNode* head) {
        unordered_set<int> s;
        int n = nums.size();
        for (int i = 0; i < n; i++) {
            s.insert(nums[i]);
        }
        ListNode* ans = head;
        while (ans != NULL && s.find(ans->val) != s.end()) {
            ans = ans->next; // Check for the head node excusively
        }
        head = ans;
        while (head != NULL && head->next != NULL) {
            if (s.find(head->next->val) != s.end()) {
                head->next = head->next->next;
                continue;
            }
            head = head->next;
        }
        return ans;
    }
    ```
22. Plus one 
    ```
        ListNode* rev(ListNode* head) {
        ListNode* prev = NULL;
        ListNode* next;
        ListNode* curr = head;
        while (curr) {
            next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
    ListNode* plusOne(ListNode* head) {

       if (head == NULL)
            return head;

        ListNode* nhead = rev(head);
        ListNode* curr = nhead;
        int carry = 1; // Initial carry is 1 because we are adding one

        while (curr && carry) {
            int sum = curr->val + carry;
            curr->val = sum % 10;
            carry = sum / 10;
            if (carry && curr->next == NULL) {
                curr->next = new ListNode(0); // Add a new node if we still have a carry
            }
            curr = curr->next;
        }

        return rev(nhead);
    }
    ```
23. Linked List components
    ```
      int numComponents(ListNode* head, vector<int>& nums) {
        set<int> st;
        for (auto num : nums)
            st.insert(num);
        ListNode* curr = head;
        int ans = 0, val;
        while (curr) {
            val = curr->val;
            while (curr and st.count(curr->val)) {
                val = curr->val;
                curr = curr->next;
            }
            if (st.count(val))
                ans++;
            if (curr)
                curr = curr->next;
        }
        return ans;
    }
    ```
24. Circular ll (ascending)
    ```
      Node* insert(Node* head, int insertVal) {
        Node* curr = head;
        Node* n = new Node(insertVal);
        if (curr == NULL) {

            n->next = n;
            return n;
        }
        if (insertVal <= head->val) {
            while (curr->next != head)
                curr = curr->next;
            curr->next = n;
            n->next = head;

            return n;
        }

        while (curr->next != head && curr->next->val < insertVal)
            curr->next;
        n->next = curr->next;
        curr->next = n;
        return head;
    }
    ```
25. Descending  3->4->1 , 3->4->2->1
    ```
       Node* insert(Node* head, int insertVal) {
        Node *newNode = new Node(insertVal);
        if (!head) {
            newNode->next = newNode;
            return newNode;
        }

        Node *prevNode = head;
        Node *currNode = head->next;

        while (true) {
            if (insertVal >= prevNode->val && insertVal <= currNode->val) {
                prevNode->next = newNode;
                newNode->next = currNode;
                return head;
            }

            if (prevNode->val > currNode->val && (insertVal <= currNode->val || insertVal >= prevNode->val)) {
                prevNode->next = newNode;
                newNode->next = currNode;
                return head;
            }

            prevNode = currNode;
            currNode = currNode->next;

            if (prevNode == head) {
                prevNode->next = newNode;
                newNode->next = currNode;
                return head;
            }
        }

        return nullptr;
    }
    ```
26. Design Skiplist
    ```
     class Skiplist {
    private:
    struct ListNode {
        int val;
        ListNode* next;
        ListNode* down;
        ListNode(int v, ListNode* n, ListNode* d) : val(v), next(n), down(d) {}
    };

    public:
    ListNode* head;
    int numLevel;
    int numCount;

    Skiplist() : head(new ListNode(-1, nullptr, nullptr)), numLevel(0), numCount(0) {}

    bool search(int target) {
        ListNode* curr = head;
        while (curr != nullptr) {
            while (curr->next != nullptr && curr->next->val < target)
                curr = curr->next;
            if (curr->next != nullptr && curr->next->val == target)
                return true;
            curr = curr->down;
        }
        return false;
    }

    void add(int num) {
        std::stack<ListNode*> st;
        ListNode* current = head;
        while (current != nullptr) {
            while (current->next != nullptr && current->next->val < num) {
                current = current->next;
            }
            st.emplace(current);
            current = current->down;
        }

        bool insert = true;
        ListNode* down = nullptr;

        while (insert && !st.empty()) {
            current = st.top();
            st.pop();
            current->next = new ListNode(num, current->next, down);
            down = current->next;
            insert = (rand() % 2 == 0);
        }

        if (insert) {
            head = new ListNode(-1, nullptr, head);
            numLevel++;
        }
        numCount++;
    }

    bool erase(int num) {
        ListNode* current = head;
        bool found = false;
        while (current != nullptr) {
            while (current->next != nullptr && current->next->val < num)
                current = current->next;
            if (current->next != nullptr && current->next->val == num) {
                found = true;
                ListNode* target = current->next;
                current->next = current->next->next;
                delete target;
            }
            current = current->down;
        }
        return found;
    }

    ```
27. Add two numbers II
    ```
      ListNode* reverse(ListNode* head) {
        ListNode *p, *q;
        p = head, q = NULL;

        while (p != NULL) {
            ListNode* on = p->next; // keep track of original next node
            p->next = q;            // reversing links
            q = p;
            p = on;
        }
        return q;
    }
    ListNode* addTwoNumbers(ListNode* head1, ListNode* head2) {
        ListNode* p = reverse(head1);
        ListNode* q = reverse(head2);

        ListNode *head, *last;
        head = last = new ListNode(-1); // pointers to new list

        int carry = 0;
        while (p != NULL || q != NULL) {
            int d = (p != NULL ? p->val : 0) + (q != NULL ? q->val : 0) + carry;

            ListNode* temp = new ListNode(d % 10);
            last->next = temp;
            last = temp;

            carry = d / 10;

            if (p)
                p = p->next;
            if (q)
                q = q->next;
        }

        if (carry != 0) {
            ListNode* temp = new ListNode(carry);
            last->next = temp;
            last = temp;
        }

        head = head->next; // since head was pointing to dummy node
        head = reverse(head);

        return head;
    }
    ```
28. Next greater element in linked list 
    ```
      ector<int> nextLargerNodes(ListNode* head) {
        vector<int>arr;
        ListNode*temp=head;
        int n=0;
        while(temp){
            arr.push_back(temp->val);
            temp=temp->next;
            n++;
        }
     
        vector<int>ans(n);
        stack<int>st;
        ans[n-1]=0;
        st.push(arr[n-1]);
        for(int i=n-2; i>=0; i--){
            while(st.size()>0 && st.top()<=arr[i]){
                st.pop();
            }
            if(st.size()==0) ans[i]=0;
            else ans[i]=st.top();
            st.push(arr[i]);
        }
        return ans;
        
    }
    ```
29.Intersection of two linked lists

```
      ListNode* getIntersectionNode(ListNode* headA, ListNode* headB) {
        unordered_map<ListNode*, int> mpp;
        for (auto p = headA; p != NULL; p = p->next) {
            mpp[p] = p->val;
        }
        for (auto p = headB; p != NULL; p = p->next) {
            if (mpp.find(p) != mpp.end())
                return p;
        }
        return NULL;
    }
```

```
       int length(ListNode *head){
        int len = 0;
        while(head){
            len++;
            head = head->next;
        }
        return len;
        }
         ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if(!headA || !headB) return NULL;


        int lenA = length(headA), lenB = length(headB);

  
        if(lenA>lenB){
            while(lenA>lenB){
                headA = headA->next;
                lenA--;
            }
        }
        else if(lenA<lenB){
            while(lenA<lenB){
                headB = headB->next;
                lenB--;
            }
        }
        

        while(headA && headB){
            if(headA==headB) return headA;
            headA = headA->next;
            headB = headB->next;
        }
        return NULL;
    }
 ```
30. Delete a node
    ```
      void deleteNode(ListNode* node) {
        node->val=node->next->val;
        node->next=node->next->next;
       
        
    }
    ```
31. Delete N nodes after M nodes in a linked list 
    ```
        ListNode* deleteNodes(ListNode* head, int m, int n) {
        if (!head) {
            return head;
        }

        int mcnt = 0, ncnt = 0;
        ListNode* curr = head;
        ListNode* tmpNode;

        while (curr != nullptr) {
            mcnt += 1;
            if (mcnt == m) {
                tmpNode = curr;
                while (tmpNode && ncnt != n + 1) {
                    ncnt += 1;
                    tmpNode = tmpNode->next;
                }
                curr->next = tmpNode;
                mcnt = 0;
                ncnt = 0;
                curr = curr->next;
            } else {
                curr = curr->next;
            }
        }
        return head;
    }
    ```
32. Remove duplicate nodes from an unsorted list 
    ```
      ListNode* deleteDuplicatesUnsorted(ListNode* head) {
        unordered_map<int, int> mp;

        ListNode* tmp = head;
        while (tmp != nullptr) {
            mp[tmp->val]++;
            tmp = tmp->next;
        }

        ListNode* prev = new ListNode(-1);

        ListNode* curr = prev;
        tmp = head;

        while (tmp != nullptr) {
            if (mp[tmp->val] == 1) {
                curr->next = new ListNode(tmp->val);
                curr = curr->next;
            }

            tmp = tmp->next;
        }

        return prev->next;
    }
    ```
33. Merge nodes in between the zero
    ```
      ListNode* mergeNodes(ListNode* head) {
        ListNode* dummy = new ListNode(0);
        ListNode* current = dummy;
        
        ListNode* temp = head->next;
        int sum = 0;
        
        while (temp != nullptr) {
            if (temp->val != 0) {
                sum += temp->val;
            } else {
                if (sum != 0) {
                    current->next = new ListNode(sum);
                    current = current->next;
                    sum = 0;
                }
            }
            temp = temp->next;
        }
        
        return dummy->next;
        
    }
    ```
34. Flatten a multilevel linked list
    ```
        Node* flatten(Node* head) {
        Node* curr = head;
        Node* tail = head;
        stack<Node*> s;
        while (curr != nullptr) {
            if (curr->child != nullptr) {
                if (curr->next != nullptr) {
                    Node* nx = curr->next;
                    nx->prev = nullptr;
                    s.push(nx);
                }

                // dealing with childrens
                Node* child = curr->child;
                curr->next = child;
                child->prev = curr;
                curr->child = nullptr;
            }
            tail = curr;
            curr = curr->next;
        }

        // now we will try to link the remaining part
        while (!s.empty()) {
            curr = s.top(); // as current become null so we can say it to point
                            // the top most node of stack.
            tail->next = curr;
            curr->prev = tail;
            while (curr != nullptr) {
                tail = curr;
                curr = curr->next;
            }
            s.pop();
        }

        return head;
    }
    ```
35. Odd even linked list
     ```
         ListNode* oddEvenList(ListNode* head) {
         if(!head || !head->next || !head->next->next) return head;
        
        ListNode *odd = head;
        ListNode *even = head->next;
        ListNode *even_start = head->next;
        
        while(odd->next && even->next){
            odd->next = even->next; 
            even->next = odd->next->next;  
            odd = odd->next;
            even = even->next;
        }
        odd->next = even_start;   
        return head; 
        
    }
     ```
36. Remove Zero Sum Consecutive Nodes from Linked List
   ```
       ListNode* removeZeroSumSublists(ListNode* head) {

        ListNode *dummy = new ListNode(0), *cur = dummy;
        dummy->next = head;
        int prefix = 0;
        map<int, ListNode*> m;
        while (cur) {
            prefix += cur->val;
            if (m.count(prefix)) {
                cur = m[prefix]->next;
                int p = prefix + cur->val;
                while (p != prefix) {
                    m.erase(p);
                    cur = cur->next;
                    p += cur->val;
                }
                m[prefix]->next = cur->next;
            } else {
                m[prefix] = cur;
            }
            cur = cur->next;
        }
        return dummy->next;
    }
   ```
