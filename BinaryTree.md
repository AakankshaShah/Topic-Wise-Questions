# Binary Tree

## Theory 

## Questions 

1. Sorted list to BST 

  ```
    if (head == nullptr)
        return nullptr;

    if (head->next == nullptr)
        return new TreeNode(head->val);

    ListNode* slow = head;
    ListNode* fast = head;
    ListNode* slow_prev = nullptr;

    while (fast != nullptr && fast->next != nullptr) {
        slow_prev = slow;
        slow = slow->next;
        fast = fast->next->next;
    }

    // slow points to mid
    if (slow_prev != nullptr)
        slow_prev->next = nullptr;

    TreeNode* root = new TreeNode(slow->val);
    root->left = sortedListToBST(head);
    root->right = sortedListToBST(slow->next);

    return root;
 ```

2. Minimum depth of a binary tree
    ```
     if(!root)
            return 0;
        
        if(!root->left && !root->right)
            return 1;
        
        queue<TreeNode*> que;
        que.push(root);
        
        int depth = 1;
        
        while(!que.empty()) {
            
            int n = que.size();
            
            while(n--) {
                TreeNode* temp = que.front();
                que.pop();
                
                if(!temp->left && !temp->right)
                    return depth;
                
                if(temp->left)
                    que.push(temp->left);
                if(temp->right)
                    que.push(temp->right);
            }
            
            depth++;
            
        }
        
        return -1;
    ```

    ```
     //DFS
       if(!root)
            return 0;
        
        if(root->left == NULL && root->right == NULL)
            return 1;
        
        int left  = root->left?minDepth(root->left):INT_MAX;
        int right = root->right?minDepth(root->right):INT_MAX;
        
        return 1 + min(left, right);

    ```
3. Populating next right pointer in each node
     
   ```
     if(root==NULL){
            return NULL;
        }
        Node* current=root;
        while(current->left!=NULL){
            
            Node* temp=current;
            while(current!=NULL){
                
                current->left->next=current->right;
                current->right->next=current->next==NULL?NULL: current->next->left;
                current=current->next;
                
            }
            current=temp->left;
        }
        return root;
   ```
4. Invert Binary tree
    ```
      void swap(TreeNode* curr) {
        if (!curr)
            return;
        // Swap the child pointers
        TreeNode* temp;
        temp = curr->left;
        curr->left = curr->right;
        curr->right = temp;

        swap(curr->left);
        swap(curr->right);
    }
    TreeNode* invertTree(TreeNode* root) {
        swap(root);
        return root;
    }
    ```

