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
5. LCA  of Binary Tree
   ```
      if (root == NULL || p == root || q == root)
            return root;

        TreeNode* lh = lowestCommonAncestor(root->left, p, q);
        TreeNode* rh = lowestCommonAncestor(root->right, p, q);

        if (lh == NULL)
            return rh;

        else if (rh == NULL)
            return lh;

        else
            return root;
   ```
6. LCA of Binary Search Tree

   ```
     if (root == NULL)
            return NULL;

        if (root->val < p->val && root->val < q->val)
            return lowestCommonAncestor(root->right, p, q);

        else if (root->val > p->val && root->val > q->val)
            return lowestCommonAncestor(root->left, p, q);

        return root;
   ```
7. House robber 3 https://www.youtube.com/watch?v=FYho45iq68Y
      ```
        unordered_map<TreeNode*, int> map;

        int rob(TreeNode* root) {
        // Uncomment the following lines if you want to use the recursive approach
        // pair<int, int> ans = solve(root);
        // return max(ans.first, ans.second);

        if (root == nullptr) return 0;
        if (map.find(root) != map.end()) return map[root];

        int val = 0;
        if (root->left != nullptr) val += rob(root->left->left) + rob(root->left->right);
        if (root->right != nullptr) val += rob(root->right->left) + rob(root->right->right);

        map[root] = max(root->val + val, rob(root->left) + rob(root->right));
        return map[root];
       }
      ```
8. Find leaves of a binary tree
   ```
    map<int, vector<int>> m;

    int findHeight(TreeNode* node) {
        if (node == NULL)
            return 0;

        int lh = findHeight(node->left);
        int rh = findHeight(node->right);

        int h = 1 + max(lh, rh);
        m[h].push_back(node->val);
        return h;
    }

    vector<vector<int>> findLeaves(TreeNode* root) {

        findHeight(root);

        vector<vector<int>> res;
        for (auto it = m.begin(); it != m.end(); it++) {
            res.push_back(it->second);
        }

        return res;
    }
   ```
9.Find duplicate subtree

   ``` 
    string DFS(TreeNode* root, unordered_map<string, int>& mp,
               vector<TreeNode*>& res) {
        if (root == NULL)
            return "NULL";

        string s = to_string(root->val) + "," + DFS(root->left, mp, res) + "," +
                   DFS(root->right, mp, res);

        if (mp[s] == 1)
            res.push_back(root);

        mp[s]++;

        return s;
    }
    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        unordered_map<string, int> mp;

        vector<TreeNode*> res;

        DFS(root, mp, res);

        return res;
    }
   ```

10. Sum of left leaves

    ```
     bool isLeaf(TreeNode* node) {
        if (node == NULL)
            return false;
        if (node->left == NULL && node->right == NULL)
            return true;
        return false;
    }

    int leftLeavesSum(TreeNode* root) {

        int res = 0;

        if (root != NULL) {

            if (isLeaf(root->left))
                res += root->left->val;
            else
                res += leftLeavesSum(root->left);

            res += leftLeavesSum(root->right);
        }

        // return result
        return res;
    }

    ```
11. Symmetric tree
     
    ```
      bool check(TreeNode* l, TreeNode* r) {
        if(l == NULL && r == NULL)
            return true;
        if(l == NULL || r == NULL)
            return false;
        
        if(l->val == r->val && check(l->left, r->right) && check(l->right, r->left))
            return true;
        
        return false;
    }
    ```

