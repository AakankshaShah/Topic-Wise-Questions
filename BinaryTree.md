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
12 . All nodes at distance k

     ```
       unordered_map<TreeNode*, TreeNode*> parent;
       void addParent(TreeNode* root) {
        if (!root)
            return;

        if (root->left)
            parent[root->left] = root;

        addParent(root->left);

        if (root->right)
            parent[root->right] = root;

        addParent(root->right);
    }

    void collectKDistanceNodes(TreeNode* target, int k, vector<int>& result) {

        queue<TreeNode*> que;
        que.push(target);
        unordered_set<int> visited;
        visited.insert(target->val);

        while (!que.empty()) {

            int n = que.size();
            if (k == 0)
                break;

            while (n--) {
                TreeNode* curr = que.front();
                que.pop();

                if (curr->left && !visited.count(curr->left->val)) {
                    que.push(curr->left);
                    visited.insert(curr->left->val);
                }
                if (curr->right && !visited.count(curr->right->val)) {
                    que.push(curr->right);
                    visited.insert(curr->right->val);
                }

                if (parent.count(curr) && !visited.count(parent[curr]->val)) {
                    que.push(parent[curr]);
                    visited.insert(parent[curr]->val);
                }
            }
            k--;
        }

        while (!que.empty()) {
            TreeNode* temp = que.front();
            que.pop();
            result.push_back(temp->val);
        }
    }
    vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
        vector<int> result;

        addParent(root);

        collectKDistanceNodes(target, k, result);
        return result;
    }
     ```
12. Increasing order BST
     ```
      void inorder(TreeNode* root, queue<TreeNode*>& q) {
        if (!root)
            return;
        inorder(root->left, q);
        q.push(root);
        inorder(root->right, q);
    }
    TreeNode* increasingBST(TreeNode* root) {
        if (!root)
            return nullptr;
        queue<TreeNode*> q;
        inorder(root, q);
        TreeNode* newRoot = q.front();
        TreeNode* node = newRoot;
        q.pop();
        while (!q.empty()) {
            node->left = nullptr;
            node->right = q.front();
            node = node->right;
            q.pop();
        }
        node->left = nullptr;
        node->right = nullptr;
        return newRoot;
    }
     
     ```
13. Range sum BT
     ```
      void traverse(TreeNode* node, int low, int high)
     {
    if(node->val >=low && node->val <= high)
        res+=node->val;

    if(node->left != NULL)
        traverse(node->left, low, high);

    if(node->right != NULL)
        traverse(node->right, low, high);
    }

    int rangeSumBST(TreeNode* root, int low, int high) {
        res = 0;
        traverse(root, low, high);
        return res;
    }
     ```


     ```
     //Make use of BST 
     int dfs(TreeNode* root,int low,int high)
    {
        if(root== NULL)
            return 0;
        int ans = 0;
        if(root->val >=low && root->val <=high)
            ans =  root->val;
        
        int left = 0,right = 0;
        if(low<=root->val)
            left = dfs(root->left,low,high);
        if(high>=root->val)
            right = dfs(root->right,low,high);
        return ans +left+right;
    }
    int rangeSumBST(TreeNode* root, int low, int high) {
        return dfs(root,low,high);
    }
     ```

14. Height of tree after substree removal queries**
    ```
     unordered_map<int, int> lheight;
    unordered_map<int, int> rheight;
    unordered_map<int, int> res;
    int findHeight(TreeNode* root) {
        if (root == NULL)
            return 0;

        int left = findHeight(root->left);
        int right = findHeight(root->right);

        lheight[root->val] = left;
        rheight[root->val] = right;

        return 1 + max(left, right);
    }
    void newHeight(TreeNode* root, int maxi, int level) {
        if (root == NULL)
            return;
        res[root->val] = maxi;
        newHeight(root->left, max(maxi, level + rheight[root->val]), level + 1);
        newHeight(root->right, max(maxi, level + lheight[root->val]),
                  level + 1);
    }
    vector<int> treeQueries(TreeNode* root, vector<int>& queries) {
        int m = queries.size();

        vector<int> answer(m, 0);
        if (root == NULL)
            return answer;
        findHeight(root);
        newHeight(root, 0, 0);

        for (int i = 0; i < m; i++) {
            answer[i] = res[queries[i]];
        }
        return answer;
    }
    ```
15. Avg substree 

    ```
     int countNodes(TreeNode* root) {
        if (!root)
            return 0;
        return 1 + countNodes(root->left) + countNodes(root->right);
    }

    int totalSum(TreeNode* root) {
        if (!root)
            return 0;
        return root->val + totalSum(root->left) + totalSum(root->right);
    }
    int averageOfSubtree(TreeNode* root) {
        if (!root)
            return 0;
        int sum = root->val + totalSum(root->left) + totalSum(root->right);
        int totalNodes = 1 + countNodes(root->left) + countNodes(root->right);

        int avg = floor(sum / totalNodes);
        if (avg == root->val) {
            return 1 + averageOfSubtree(root->left) +
                   averageOfSubtree(root->right);
        }
        return averageOfSubtree(root->left) + averageOfSubtree(root->right);
    }

    ```
16. Distance between two nodes
    ```
     TreeNode* LCA(TreeNode* root, int n1, int n2) {
        // Your code here
        if (root == NULL)
            return root;
        if (root->val == n1 || root->val == n2)
            return root;

        TreeNode* left = LCA(root->left, n1, n2);
        TreeNode* right = LCA(root->right, n1, n2);

        if (left != NULL && right != NULL)
            return root;
        if (left == NULL && right == NULL)
            return NULL;
        if (left != NULL)
            return LCA(root->left, n1, n2);

        return LCA(root->right, n1, n2);
    }

    int findLevel(TreeNode* root, int k, int level) {
        if (root == NULL)
            return -1;
        if (root->val == k)
            return level;

        int left = findLevel(root->left, k, level + 1);
        if (left == -1)
            return findLevel(root->right, k, level + 1);
        return left;
    }
    int findDistance(TreeNode* root, int a, int b) {

        TreeNode* lca = LCA(root, a, b);

        int d1 = findLevel(lca, a, 0);
        int d2 = findLevel(lca, b, 0);

        return d1 + d2;
    }
    ```

17. Delete node & return forest 
    ```
      TreeNode* deleteNodes(TreeNode* root, set<int> st,
                          vector<TreeNode*>& result) {
        if (!root)
            return NULL;
        root->left =
            deleteNodes(root->left, st, result); // left  se deleted nodes
        root->right =
            deleteNodes(root->right, st, result); // right se deleted nodes

        if (st.count(root->val)) { // if I have to delete this root, then put
                                   // root->left and root->right in result
            if (root->left != NULL)
                result.push_back(root->left);
            if (root->right != NULL)
                result.push_back(root->right);
            return NULL;
        } else
            return root;
    }
    vector<TreeNode*> delNodes(TreeNode* root, vector<int>& to_delete) {
        set<int> st;
        for (int i : to_delete)
            st.insert(i);

        vector<TreeNode*> result;
        deleteNodes(root, st, result); // <-- it will not consider root

        // So, check here if root is to be deleted or not
        if (!st.count(root->val)) {
            result.push_back(root);
        }
        return result;
    }
    ```

18. Complete binary tree

    ```
     queue<TreeNode*> que;

        que.push(root);

        bool past = false; // kya aapne past me NULL node dekha hai ?

        while (!que.empty()) {
            TreeNode* node = que.front();
            que.pop();

            if (node == NULL) {
                past = true;
            } else {
                if (past == true) {
                    return false;
                }

                que.push(node->left);
                que.push(node->right);
            }
        }

        return true;
    ```

19. Valid Binary Tree
    ```
     bool util(TreeNode* root, long min, long max) {
        if (root == NULL)
            return true;
        if (root->val <= min || root->val >= max)
            return false;
        return util(root->left, min, root->val) &&
               util(root->right, root->val, max);
    }

    bool isValidBST(TreeNode* root) { return util(root, LONG_MIN, LONG_MAX); }
    ```

17. Vertical order of BT
    ```
     map < int, map < int, multiset < int > >> nodes;
        queue < pair < TreeNode*, pair < int, int >>> todo;
        todo.push({root, {0,0}});
        
        while(!todo.empty()){
            auto p = todo.front();
            todo.pop();
            TreeNode* node = p.first;
            int x = p.second.first, y = p.second.second;
            nodes[x][y].insert(node->val);
            
            if(node->left)
                todo.push({node->left,{x-1, y+1}});
            
            if(node->right)
                todo.push({node->right, {x+1, y+1}});
        }
        vector < vector < int > > ans;
        
        for(auto p : nodes){
            vector < int > col;
            for(auto q : p.second){
                col.insert(col.end(), q.second.begin(), q.second.end());
            }
            ans.push_back(col);
        }
        return ans;
    ```
18. Binary Tree inorder traversal
    ```
     vector<int> ans;
    void inorder(TreeNode* root) {
        if (root == NULL)
            return;

        inorder(root->left);
        ans.push_back(root->val);
        inorder(root->right);
    }
    vector<int> inorderTraversal(TreeNode* root) {
        inorder(root);
        return ans;
    }
    ```
19. Diameter of a binary tree
    ```
       int findMax(TreeNode* node, int& maxi) {
        if (node == NULL) {
            return 0;
        }
        int lh = findMax(node->left, maxi);
        int rh = findMax(node->right, maxi);
        maxi = max(maxi, lh + rh);
        return 1 + max(lh, rh);
    }
    int diameterOfBinaryTree(TreeNode* root) {
        int maxi = 0;
        findMax(root, maxi);
        return maxi;
    }
    ```
20. Height of binary tree
 ```
int height(TreeNode* node)
{ if(node==null)
return 0;
int lh=height(node->left);
int rh=height(node->right);
return 1+max(lh,rh);
}
```
21. Evaluate boolean binary tree
    ```
        bool evaluateTree(TreeNode* root) {
        if(root->left==NULL||root->right==NULL)
        return root->val;


        if (root->val == 2)
            return evaluateTree(root->left) | evaluateTree(root->right);
        else
            return evaluateTree(root->left) & evaluateTree(root->right);
    }
    ```
