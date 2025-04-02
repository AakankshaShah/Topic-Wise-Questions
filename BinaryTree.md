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
   ```
     public Node connect(Node root) {
        if(root==null)
        return null;
        Node curr=root;
        while(curr.left!=null)
        {
            Node temp=curr;
            while(curr!=null)
            {
                curr.left.next=curr.right;
                curr.right.next=curr.next==null?null:curr.next.left;
                curr=curr.next;
            }
            curr=temp.left;
        }
        return root;
        
    }
   ```
   ```
    //Part 2
      Node* connect(Node* root) {
        if(!root) return root;
        queue<Node *> q;
        q.push(root);

        while(!q.empty()){
            int size = q.size();
            Node *prev = nullptr;

            for(int i=0;i<size;i++){
                Node *t = q.front();
                q.pop();
                if(!t) break;
                t->next = prev;
                prev = t;

                if(t->right) q.push(t->right);
                if(t->left) q.push(t->left);
            }
        }
        return root;
    }
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
13. Increasing order BST
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
14. Range sum BT
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
     ```
       int sum = 0;

    void solve(TreeNode root, int low, int high) {
        if (root == null)
            return;

        if (root.val >= low && root.val <= high)
            sum += root.val;

        if (root.val >= low)
            solve(root.left, low, high);

        if (root.val <= high)
            solve(root.right, low, high);
    }

    public int rangeSumBST(TreeNode root, int low, int high) {
        solve(root, low, high);
        return sum;

    }
     ```

15. Height of tree after substree removal queries**
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
16. Avg substree 

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
17. Distance between two nodes
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

18. Delete node & return forest 
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

19. Complete binary tree

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

20. Valid Binary Tree
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

21. Vertical order of BT
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
    ```
     vector<vector<int>> verticalOrder(TreeNode* root) {
        vector<vector<int>> result;
        if (root == nullptr)
            return result;
        queue<pair<TreeNode*, int>> que;
        que.push(make_pair(root, 0));
        map<int, vector<int>> mp;
        while (!que.empty()) {
            int size = que.size();
            for (int i = 0; i < size; i++) {
                auto node = que.front();
                que.pop();
                mp[node.second].push_back(node.first->val);
                if (node.first->left != nullptr) {
                    que.push(make_pair(node.first->left, node.second - 1));
                }
                if (node.first->right != nullptr) {
                    que.push(make_pair(node.first->right, node.second + 1));
                }
            }
        }
        for (auto it = mp.begin(); it != mp.end(); it++) {
            result.push_back(it->second);
        }
        return result;
    }
    ```
    ```
       public List<List<Integer>> verticalOrder(TreeNode root) {
         List<List<Integer>>  ans=new ArrayList<>();
        if (root == null)
            return ans;

       Queue<Pair<TreeNode, Integer>> q = new LinkedList<>();
        Map<Integer, List<Integer>> mp = new TreeMap<>();
        q.offer(new Pair<>(root, 0)); 
         while (!q.isEmpty()) {
            Pair<TreeNode, Integer> f = q.poll();
            TreeNode node = f.getKey();
            int level = f.getValue();

     
            mp.computeIfAbsent(level, k -> new ArrayList<>()).add(node.val);

            if (node.left != null)
                q.offer(new Pair<>(node.left, level - 1));  
            if (node.right != null)
                q.offer(new Pair<>(node.right, level + 1)); 
        }

       
        for (List<Integer> values : mp.values()) {
            ans.add(values);
        }

        return ans;
        
    }
    ```
22. Binary Tree inorder traversal
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
23. Diameter of a binary tree
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
24. Height of binary tree
     ```
        int height(TreeNode * node) {
            if (node == null)
                return 0;
            int lh = height(node->left);
            int rh = height(node->right);
            return 1 + max(lh, rh);
        }

     ```
25. Evaluate boolean binary tree
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
26. Balanced binary tree
    ```
      int isBT(TreeNode* root)
    {
        if(root==NULL)
        return 0;


        int lh= isBT(root->left);
        int rh= isBT(root->right);

        if(lh==-1||rh==-1)
        return -1;

        if(abs(lh-rh)>1)
        return -1;

        return 1+max(lh,rh);
      
        
        
    }
    
    bool isBalanced(TreeNode* root) 
    {

     if(isBT(root)==-1)
     return false;

     return true;



        
    }
    ```
27. Maximum path sum
    ```
       int ans;
    int sum(TreeNode* root, int& ans) {
        if (root == NULL)
            return 0;
        int lh = max(0,sum(root->left, ans));
        int rh = max(0,sum(root->right, ans));
        ans = max(ans, root->val + lh + rh);
        return root->val + max(lh, rh);
    }
    int maxPathSum(TreeNode* root) {
        ans = INT_MIN;
        sum(root, ans);
        return ans;
    }
    }
    ```
28. Maximum Width 
    ```
      int widthOfBinaryTree(TreeNode* root) {
        if (root == NULL)
            return 0;
        int ans = 0;
        queue<pair<TreeNode*, long long>> q;
        q.push({root, 0});
        while (!q.empty()) {
            int size = q.size();
            int mmin = q.front().second;
            int first, last;
            for (int i = 0; i < size; i++) {
                int curr_id = q.front().second - mmin;
                TreeNode* n = q.front().first;
                q.pop();
                if (i == 0)
                    first = curr_id;
                if (i == size - 1)
                    last = curr_id;
                if (n->left)
                    q.push({n->left, (long long)curr_id * 2 + 1});
                if (n->right)
                    q.push({n->right, (long long)curr_id * 2 + 2});
            }
            ans = max(ans, last - first + 1);
        }
        return ans;
    }
    ```
29. Distribute coins in a bt
    ```
      int solve(TreeNode* node) {
        if (node == NULL)
            return 0;
        int lh = solve(node->left);
        int rh = solve(node->right);
        moves += abs(lh) + abs(rh);
        return lh + rh + node->val - 1;
    }
    int distributeCoins(TreeNode* root) {
        moves = 0;
        solve(root);
        return moves;
    }
    ```
30. Binary tree paths https://www.youtube.com/watch?v=gSFcPOPyq-Y
    ```
       string convert(vector<int>& path) {
        string ans = "";
        int n = path.size();
        for (int i = 0; i < n - 1; i++) {
            ans += to_string(path[i]);
            ans.push_back('-');
            ans.push_back('>');
        }
        ans += to_string(path[n - 1]);
        return ans;
    }

    void solve(TreeNode* root, vector<string>& path, vector<int>& ans) {
        if (root == NULL) {
            return;
        }
        if (root->left == NULL && root->right == NULL) {
            ans.push_back(root->val);
            path.push_back(convert(ans));
            ans.pop_back();
        }
        ans.push_back(root->val);
        solve(root->left, path, ans);
        solve(root->right, path, ans);
        ans.pop_back();
    }

    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> path;
        vector<int> ans;
        solve(root, path, ans);
        return path;
    }
    ```
31. Binary Tree zig zag traversal 
    ```
       vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        queue<TreeNode*> q;
        if (root == NULL)
            return ans;
        q.push(root);

        int c = 0;
        while (q.size() > 0) {
            int s = q.size();
            vector<int> v;
            while (s--) {
                TreeNode* node = q.front();
                q.pop();
                v.push_back(node->val);
                if (node->left != NULL)
                    q.push(node->left);
                if (node->right != NULL)
                    q.push(node->right);
            }
            if (c == 1) {
                reverse(v.begin(), v.end());
            }
            c = c ^ 1;
            ans.push_back(v);
        }
        return ans;
    }
    ```
32. Serialize  deserailize
    ```
      string serialize(TreeNode* root) {
        if (!root) {
            return "";
        }

        string s = "";
        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            TreeNode* curNode = q.front();
            q.pop();

            if (curNode == nullptr) {
                s += "#,";
            } else {
                s += to_string(curNode->val) + ",";
                q.push(curNode->left);
                q.push(curNode->right);
            }
        }

        return s;
    }

    TreeNode* deserialize(string data) {
        if (data.empty()) {
            return nullptr;
        }

        stringstream s(data);
        string str;
        getline(s, str, ',');
        TreeNode* root = new TreeNode(stoi(str));
        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            TreeNode* node = q.front();
            q.pop();

            getline(s, str, ',');
            if (str != "#") {
                TreeNode* leftNode = new TreeNode(stoi(str));
                node->left = leftNode;
                q.push(leftNode);
            }

            getline(s, str, ',');
            if (str != "#") {
                TreeNode* rightNode = new TreeNode(stoi(str));
                node->right = rightNode;
                q.push(rightNode);
            }
        }

        return root;
    }
    ```
33. Path Sum 
    ```
      bool hasPathSum(TreeNode* root, int targetSum) {
        if (root == NULL)
            return false;
        stack<TreeNode*> path;
        stack<int> sum;
        path.push(root);
        sum.push(root->val);
        while (!path.empty()) {
            TreeNode* p = path.top();
            int v = sum.top();
            sum.pop();
            path.pop();
            if (p->left == NULL && p->right == NULL && v == targetSum)
                return true;
            if (p->left) {
                path.push(p->left);
                sum.push(p->left->val + v);
            }
            if (p->right) {
                path.push(p->right);
                sum.push(p->right->val + v);
            }
        }
        return false;
    }
    ```
   ```
/From any node
      bool pathSumFromNode(TreeNode* node, int currentSum, int targetSum) {
    if (!node) return false;

    // Add the current node's value to the current sum
    currentSum += node->value;

    // If the current sum equals the target sum, we've found a valid path
    if (currentSum == targetSum) return true;

    // Recur for left and right children
    return pathSumFromNode(node->left, currentSum, targetSum) || pathSumFromNode(node->right, currentSum, targetSum);
}

// Function for Condition (ii): Check if any path in the tree sums to the target
bool hasPathSumAnywhere(TreeNode* node, int targetSum) {
    if (!node) return false;

    // Check paths starting from the current node
    if (pathSumFromNode(node, 0, targetSum)) return true;

    // Recur for left and right subtrees
    return hasPathSumAnywhere(node->left, targetSum) || hasPathSumAnywhere(node->right, targetSum);
}

   ```
34. Bottom value of binary tree
    ```
      int findBottomLeftValue(TreeNode* root) {
        queue<TreeNode*> levelQue;
        levelQue.push(root);
        int leftNodeVal = root->val;
        while(!levelQue.empty()){
            int currLevelSize = levelQue.size();
            int orgSize = currLevelSize;
            while(currLevelSize-->0){
                TreeNode * currNode = levelQue.front();
                levelQue.pop();
                if(orgSize-1 == currLevelSize)
                    leftNodeVal = currNode->val;
                if(currNode->left != NULL)levelQue.push(currNode->left);
                if(currNode->right != NULL)levelQue.push(currNode->right);
            }
        }   
        return leftNodeVal;
        
    }
    ```
35. Minimum Absolute Difference in BST
   ```
     class Solution {
public:
    
    int minDiff = INT_MAX;
    
    void inOrder(TreeNode* root, TreeNode* &prev) {
        
        if(root == NULL)
            return;
        
        inOrder(root->left, prev);
        
        if(prev != NULL) {
            minDiff = min(minDiff, root->val - prev->val);
        }
        
        prev = root;
        
        inOrder(root->right, prev);
        
    }
    
    int getMinimumDifference(TreeNode* root) {
        TreeNode* prev = NULL;
        inOrder(root, prev);
        return minDiff;
    }
};
   ```
36. Graph valid tree
    ```
     bool validTree(int n, vector<vector<int>>& edges) {
        if (edges.size() == 0 and n == 1) {
            return true;
        }
        vector<vector<int>> v(n);
        for (int i = 0; i < edges.size(); i++) {
            v[edges[i][0]].push_back(edges[i][1]);
            v[edges[i][1]].push_back(edges[i][0]);
        }
        vector<bool> visited(n, false);
        queue<int> q;
        q.push(0);
        visited[0] = true;
        vector<int> parent(n, -1);
        while (!q.empty()) {
            int temp = q.front();
            q.pop();
            for (auto item : v[temp]) {
                if (item == parent[temp]) {
                    continue;
                }
                if (visited[item]) {
                    return false;
                }
                parent[item] = temp;
                visited[item] = true;
                q.push(item);
            }
        }

        for (int i = 0; i < visited.size(); i++) {
            cout << i << endl;
            if (!visited[i])
                return false;
        }

        return true;
    }
    ```
37. Deepest leave sum
   ```
      int deepestLeavesSum(TreeNode* root) {
        if (root == nullptr) {
            return 0;
        }
        queue<TreeNode*> queue;
        int sum = 0, depth;
        queue.push(root);
        while (!queue.empty()) {
            TreeNode* temp;
            depth = queue.size();
            sum = 0;
            for (int i = 0; i < depth; i++) {
                temp = queue.front();
                queue.pop();
                if (temp->left != nullptr) {
                    queue.push(temp->left);
                }
                if (temp->right != nullptr) {
                    queue.push(temp->right);
                }
                sum += temp->val;
            }
        }
        return sum;
    }
   ```

38. Flatten a linked list 
    ```
    //Reverse post order
     public:
    TreeNode* prev;
    void rpost(TreeNode* root) {
        if (root == NULL)
            return;
        rpost(root->right);
        rpost(root->left);
        root->right = prev;
        root->left = NULL;
        prev = root;
    }
    void flatten(TreeNode* root) {

        prev = NULL;

        rpost(root);
    }
    ```
39. Construct binary tree from inorder & post-order
    ```
     TreeNode* solve(vector<int>& inorder, vector<int>& postorder, int inStart,
                      int inEnd, int postStart, int postEnd) {
        if (inStart > inEnd)
            return NULL;

        TreeNode* root = new TreeNode(postorder[postEnd]);
        int root_candidate = root->val;
        int i = inStart;

   
        for (; i <= inEnd; i++) {
            if (inorder[i] == root_candidate) {
                break;
            }
        }
        int leftSize = i - inStart;
        int rightSize = inEnd - i;

        root->left = solve(inorder, postorder, inStart, i - 1, postStart,
                             postStart + leftSize - 1);
        root->right = solve(inorder, postorder, i + 1, inEnd,
                              postEnd - rightSize, postEnd - 1);

        return root;
    }
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {

        int istart = 0;
        int iend = inorder.size() - 1;
        int pstart = 0;
        int pend = postorder.size() - 1;
        return solve(inorder, postorder, istart, iend, pstart, pend);
    }
    ```
40. LCA II
    ```
      bool findX = false;
    bool findY = false;
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        TreeNode* res = findTargetNode(root, p, q);
        if (findX and findY)
            return res;
        return nullptr;
    }
    TreeNode* findTargetNode(TreeNode* curr, TreeNode* p, TreeNode* q) {
        if (curr == nullptr)
            return nullptr;
        TreeNode* left = findTargetNode(curr->left, p, q);
        TreeNode* right = findTargetNode(curr->right, p, q);
        if (curr == p) {
            findX = true;
            return curr;
        }
        if (curr == q) {
            findY = true;
            return curr;
        }
        return left == nullptr ? right : right == nullptr ? left : curr;
    }
41. LCA III
    ```
         bool findNode(Node* root, Node* q) {
        if (!root)
            return false;

        return (root == q) || findNode(root->left, q) ||
               findNode(root->right, q);
    }

     public:
    Node* lowestCommonAncestor(Node* p, Node* q) {
        if (!p || !q)
            return q;
        if (p == q)
            return q;

        if (findNode(p, q))
            return p;
        else if (findNode(q, p))
            return q;
        else
            return lowestCommonAncestor(p->parent, q->parent);
    }
    ```
    ```
      oolean findNode(Node root, Node q) {
        if (root==null)
            return false;

        return (root == q) || findNode(root.left, q) ||
               findNode(root.right, q);
    }
    public Node lowestCommonAncestor(Node p, Node q) {
        
        if (p==null || q==null)
            return null;
        if (p == q)
            return q;

        if (findNode(p, q))
            return p;
        else if (findNode(q, p))
            return q;
        else
            return lowestCommonAncestor(p.parent, q.parent);
    }
    ```
42. Sum root to leaf numbers
    ```
      lass Solution {
     public:
    int total = 0;
    void findSum(TreeNode* root, int s) {
        if (root == NULL)
            return;
        if (root->left == NULL && root->right==NULL)
            total += ((s * 10) + root->val);
        s = s * 10 + root->val;
        findSum(root->left, s);
        findSum(root->right, s);
    }
    int sumNumbers(TreeNode* root) {
        findSum(root, 0);
        return total;
    }
    };
    ```
43. Right & left view of BT
     ```
       vector<int> res;
    void recursion(TreeNode* root, int level) {
        if (root == NULL)
            return;
        if (level == res.size())
            res.push_back(root->val);
        recursion(root->right, level + 1);
        recursion(root->left, level + 1);
    }
    vector<int> rightSideView(TreeNode* root) {

        recursion(root, 0);
        return res;
    }
     ```
     ```
        public List<Integer> rightSideView(TreeNode root) {
        Queue<TreeNode> q = new LinkedList<>();
        List<Integer> ans = new ArrayList<>();
        int last = 0;
        if (root == null)
            return ans;
        q.offer(root);

        while (!q.isEmpty()) {
            int s = q.size();
            last = 0;
            while (s > 0) {
                TreeNode n = q.poll();
                if (last == 0)
                    ans.add(n.val);

                if (n.right != null)
                    q.offer(n.right);
                if (n.left != null)
                    q.offer(n.left);
                s--;
                last++;

            }

        }
        return ans;

    }
     ```
44. Inorder successor in a bst 
     ```
       TreeNode* ans;
    void findT(TreeNode* root, TreeNode* p) {
        if (p == NULL || root == NULL)
            return;
        if (root->val <= p->val) {
            inorderSuccessor(root->right, p);
        }

        else if (root->val > p->val) {
            ans = root;
            inorderSuccessor(root->left, p);
        }
        return ;

   
    }
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
        if (root == NULL || p == NULL)
            return NULL;
        findT(root, p);
        return ans;
    }
     ```
45. Max binary tree
    ```
       TreeNode* buildT(vector<int>& nums, int s, int e) {
        if (s>e)
            return NULL;
        int mr = -1;
        int pos;
        for (int i = s; i <= e; i++) {
            if (nums[i] > mr) {
                pos = i;
                mr = nums[i];
            }
        }
        TreeNode* root = new TreeNode(mr);
        root->left = buildT(nums, s, pos - 1);
        root->right = buildT(nums, pos + 1, e);
        return root;
    }

    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        int n = nums.size();
        return buildT(nums, 0, n - 1);
    }
    ```
46. Lonley nodes 
    ```
       vector<int> getLonelyNodes(TreeNode* root) {

        vector<int> ans;
        if (root == NULL)
            return ans;
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) {
            TreeNode* curr = q.front();
            q.pop();
            if (curr->left && curr->right == NULL) {
                ans.push_back(curr->left->val);
            }
            if (curr->right && curr->left == NULL) {
                ans.push_back(curr->right->val);
            }
            if (curr->left)
                q.push(curr->left);
            if (curr->right)
                q.push(curr->right);
        }
        return ans;
    }
    ```
47. Count number of good nodes
    ```
      vector<int> ans;
    void solve(TreeNode* root, int maxr) {
        if (root == NULL)
            return;
        if (root->val >= maxr) {
            ans.push_back(root->val);
            maxr = root->val;
        }
        solve(root->left, maxr);
        solve(root->right, maxr);
    }
    int goodNodes(TreeNode* root) {

        int maxr = root->val;
        solve(root, maxr);
        return ans.size();
    }
    ```
48. Max product of splitted array 
    ```
       long M = 1e9 + 7;
    long totalSum = 0, maxP = 0;

    int findTotalSum(TreeNode* root) {
        if (!root)
            return 0;

        int leftSubtreeSum = findTotalSum(root->left);
        int rightSubtreeSum = findTotalSum(root->right);
        int sum = root->val + leftSubtreeSum + rightSubtreeSum;

        maxP = max(maxP, (totalSum - sum) * sum);

        return sum;
    }

    int maxProduct(TreeNode* root) {
        if (!root)
            return 0;

        maxP = 0;

        totalSum = findTotalSum(root);

        findTotalSum(root);

        return maxP % M;
    }
    ```
49. Closest binary tree value 
    ```
      nt closestValue(TreeNode* root, double target) {
        int closest = root->val;

        while (root != nullptr) {
            if (abs(root->val - target) == abs(closest - target)) {
                closest = min(root->val, closest);
            }

            if (abs(root->val - target) < abs(closest - target)) {
              
                    closest = root->val;
            }

            if (target < root->val) {
                root = root->left;
            } else {
                root = root->right;
            }
        }

        return closest;
    }
    ```
50. Largest BST subtree
    ```
      int ans = 0;

    struct BSTInfo {
        int size;
        int minVal;
        int maxVal;
        bool isBST;
    };

    BSTInfo solve(TreeNode* root) {
        if (!root) {
            return {0, INT_MAX, INT_MIN, true}; // Base case
        }

        BSTInfo left = solve(root->left);
        BSTInfo right = solve(root->right);

        if (left.isBST && right.isBST && root->val > left.maxVal &&
            root->val < right.minVal) {
            int size = left.size + right.size + 1;
            ans = max(ans, size);
            return {size, min(root->val, left.minVal),
                    max(root->val, right.maxVal), true};
        } else {
            return {0, 0, 0, false};
        }
    }

    int largestBSTSubtree(TreeNode* root) {
        solve(root);
        return ans;
    }
    ```
51. Two sum in a bst 
    ```
      bool TwoSum(int k, vector<int>& A) {
        int l = 0, h = A.size() - 1, sum;
        while (l < h) {
            sum = A[l] + A[h];
            if (sum == k)
                return 1;
            else if (sum < k)
                l++;
            else
                h--;
        }
        return 0;
    }
    void Traverse(TreeNode* a,vector<int>& A)
    {
        if(!a) return;

        Traverse(a->left,A);
        A.push_back(a->val);
        Traverse(a->right,A);
    }
    ```
52. Max substree of same colour 
    ```
      int ans = INT_MIN;

    pair<int, bool> dfs(int u, unordered_map<int, vector<int>>& adj,
                        vector<int>& colors) {
        int size = 1;
        bool sameColor = true;

        for (int v : adj[u]) {
            auto [subtreeSize, isSameColor] = dfs(v, adj, colors);
            size += subtreeSize;
            if (!isSameColor || colors[u] != colors[v]) {
                sameColor = false;
            }
        }

        if (sameColor) {
            ans = max(ans, size);
        }

        return {size, sameColor};
    }

    int maximumSubtreeSize(vector<vector<int>>& edges, vector<int>& colors) {
        int n = colors.size();
        unordered_map<int, vector<int>> adj;

        for (auto& e : edges) {
            adj[e[0]].push_back(e[1]);
        }

        dfs(0, adj, colors);
        return ans;
    }
    ```
53. Binary Search tree iterator 
    ```
      class BSTIterator {
    public:
    stack<TreeNode*> s;
    BSTIterator(TreeNode* root) { pushAll(root); }

    int next() {
        TreeNode* tmp = s.top();
        s.pop();
        pushAll(tmp->right);
        return tmp->val;
    }

    bool hasNext() { return !s.empty(); }
    void pushAll(TreeNode* node) {
        for(;node!=NULL;s.push(node),node=node->left);
    }
    };

    ```
54. Kth Smallest element in BST
    ```
     vector<int> ans;
    void inorder(TreeNode* root) {
        if (root == NULL)
            return;
        inorder(root->left);
        ans.push_back(root->val);
        inorder(root->right);
    }

    int kthSmallest(TreeNode* root, int k) {
        inorder(root);
        return ans[k - 1];
    }
    ```
    ```
       void solve(TreeNode* root, int &cnt, int &ans, int k){
        if(root == NULL)    return;
        //left, root, right 
        solve(root->left, cnt, ans, k);
        cnt++;
        if(cnt == k){
            ans = root->val;
            return;
        }
        solve(root->right, cnt, ans, k);
    }
    int kthSmallest(TreeNode* root, int k) {
        
        int cnt = 0;        
        int ans;
        solve(root, cnt, ans, k);
        return ans;
    }
    ```
55. Binary tree coloring games
     ```
        int lc, rc;
    int countNode(TreeNode* root, int x) {
        if (!root)
            return 0;
        int l = countNode(root->left, x);
        int r = countNode(root->right, x);
        if (root->val == x) {
            lc = l;
            rc = r;
        }
        return l + r + 1;
    }
    bool btreeGameWinningMove(TreeNode* root, int n, int x) {
        countNode(root, x);
        int parentSize = n - (lc + rc + 1);
        if (lc > n / 2 || rc > n / 2 || parentSize > n / 2) {
            return true;
        }
        return false;
    }
     ```
56. Binary tree longest consecutive subsequence Part 1 & 2
    ```
        int longestConsecutive(TreeNode* root) {
        int res=0;
        helper(root, res, 1);
        return res;
        
    }
    void helper(TreeNode* node, int& res, int len) {
        if(node==nullptr) return;

        res=max(res, len);

        if(node->left) {
            if(node->left->val==node->val+1) helper(node->left, res, len+1);
            else helper(node->left, res, 1);
        }

        if(node->right) {
            if(node->right->val==node->val+1) helper(node->right, res, len+1);
            else helper(node->right, res, 1);
        }
    }
    ```
    ```
      pair<int, int> dfs(TreeNode* node, int& maxLen) {
        if (!node)
            return {0, 0};

        auto [leftInc, leftDec] = dfs(node->left, maxLen);
        auto [rightInc, rightDec] = dfs(node->right, maxLen);

        int inc = 1, dec = 1;

        if (node->left) {
            if (node->left->val == node->val + 1) {
                inc = leftInc + 1;
            } else if (node->left->val == node->val - 1) {
                dec = leftDec + 1;
            }
        }

        if (node->right) {
            if (node->right->val == node->val + 1) {
                inc = max(inc, rightInc + 1);
            } else if (node->right->val == node->val - 1) {
                dec = max(dec, rightDec + 1);
            }
        }
        // -1 to not double count the current node
        maxLen = max(maxLen, inc + dec - 1);

        return {inc, dec};
    }

    int longestConsecutive(TreeNode* root) {
        int maxLen = 0;
        dfs(root, maxLen);
        return maxLen;
    }
    ```
57. Verify preorder serilaization
     ```
       bool isValidSerialization(string preorder) {
        stringstream ss(preorder);
        string curr;
        int nodes = 1;
        while (getline(ss, curr, ',')) {
            nodes--;
            if (nodes < 0) return false;
            if (curr != "#") nodes += 2;
        }
        return nodes == 0;
        
    }
     ```
     ```
        vector<string> nodes = split(preorder, ',');
        stack<string> st;
        for (const string& curr : nodes) {

            while (curr == "#" && !st.empty() && st.top() == "#") {
                st.pop();
                if (st.empty()) {
                    return false;
                }
                st.pop(); // Pop the node that was supposed to be the parent of
                          // the "#"
            }
            st.push(curr); // Push the current node onto the stack
        }

        return st.size() == 1 && st.top() == "#";
    }
    vector<string> split(const string& s, char delimiter) {
        vector<string> result;
        stringstream ss(s);
        string item;

        while (getline(ss, item, delimiter)) {
            result.push_back(item);
        }

        return result;
    }
     ```
     ```
       bool isValidSerialization(string preorder) {
        stringstream ss(preorder);
        string item;

        queue<string> q;
        while (getline(ss, item, ',')) {
            q.push(item);
        }
        string root = q.front();
        q.pop();

        if (root == "#")
            return q.size() == 0;

        return helper(root, q) && q.size() == 0;
    }

    bool helper(string root, queue<string>& q) {

        if (root != "#" && q.size() < 2) {
            return false;
        }

        string left = q.front();
        q.pop();
        string right = q.front();
        q.pop();
        bool flag = true;

        if (left != "#")
            flag = flag && helper(left, q);
        if (right != "#")
            flag = flag && helper(right, q);

        return flag;
    }
     ```
58. Mode in a binary tree
     ```
       int currNum = 0;
    int currStreak = 0;
    int maxStreak = 0;
    vector<int> result;

    void dfs(TreeNode* root) {
        if (!root)
            return;

        dfs(root->left);

        if (root->val == currNum) {
            currStreak++;
        } else {
            currNum = root->val;
            currStreak = 1;
        }

        if (currStreak > maxStreak) {
            result = {};
            maxStreak = currStreak;
        }

        if (currStreak == maxStreak) {
            result.push_back(root->val);
        }

        dfs(root->right);
    }
    vector<int> findMode(TreeNode* root) {
        dfs(root);

        return result;
    }
     ```
59. Boundary of a binary tree
     ```
       vector<int> boundaryOfBinaryTree(TreeNode* root) {
        vector<int> ans;
        if (!root)
            return ans;
        if (!isLeaf(root))
            ans.push_back(root->val);
        LeftTree(root->left, ans);
        Leaf(root, ans);
        vector<int> rightBoundary;
        RightTree(root->right, rightBoundary);
        ans.insert(ans.end(), rightBoundary.rbegin(), rightBoundary.rend());

        return ans;
    }
    bool isLeaf(TreeNode* node) { return !node->left && !node->right; }

    void LeftTree(TreeNode* node, vector<int>& ans) {
        while (node) {
            if (!isLeaf(node))
                ans.push_back(node->val);

            if (node->left)
                node = node->left;
            else
                node = node->right;
        }
    }

    void Leaf(TreeNode* node, vector<int>& ans) {
        if (!node)
            return;
        if (isLeaf(node)) {
            ans.push_back(node->val);
            return;
        }
        // Recursively check the left and right subtrees
        Leaf(node->left, ans);
        Leaf(node->right, ans);
    }

    void RightTree(TreeNode* node, vector<int>& rightBoundary) {
        while (node) {
            if (!isLeaf(node))
                rightBoundary.push_back(node->val);
            // Move to the next right boundary node (prefer right child)
            if (node->right)
                node = node->right;
            else
                node = node->left;
        }
    }
     ```
60. Path Sum 3
     ```
      void dfs(TreeNode* root, long long targetSum, int& count,
             unordered_map<long long, int>& mp, long long sum) {
        if (root == nullptr) {
            return;
        }

        sum += root->val;

        if (sum == targetSum) {
            count++;
        }

        if (mp.find(sum - targetSum) != mp.end()) {
            count += mp[sum - targetSum];
        }

        mp[sum]++;

        dfs(root->left, targetSum, count, mp, sum);
        dfs(root->right, targetSum, count, mp, sum);

        mp[sum]--;
    }
    int pathSum(TreeNode* root, int targetSum) {
        int count = 0;
        unordered_map<long long, int> mp;
        dfs(root, targetSum, count, mp, 0);
        return count;
    }
     ```
61. Step-By-Step Directions From a Binary Tree Node to Another
     ```
       static TreeNode* LCA(TreeNode* root, int x, int y) {
        if (root == NULL || root->val == x || root->val == y)
            return root;
        TreeNode* l = LCA(root->left, x, y);
        TreeNode* r = LCA(root->right, x, y);
        if (l == NULL)
            return r;
        if (r == NULL)
            return l;
        return root;
    }
    static bool dfs(TreeNode* root, int x, string& path, bool rev = 0) {
        if (root == NULL)
            return 0;
        if (root->val == x)
            return 1;

        path += (rev ? 'U' : 'L');
        if (dfs(root->left, x, path, rev))
            return 1;
        path.pop_back();

        path += (rev ? 'U' : 'R');
        if (dfs(root->right, x, path, rev))
            return 1;
        path.pop_back();

        return 0;
    }
    string getDirections(TreeNode* root, int startValue, int destValue) {
        root = LCA(root, startValue, destValue);
        string pathFrom = "", pathTo = "";
        dfs(root, startValue, pathFrom, 1);
        dfs(root, destValue, pathTo);

        return pathFrom + pathTo;
    }
     ```
62. Linked list in a binary tree
     ```
         class Solution {
     public:
     bool solve(TreeNode* root, ListNode* head) {
        if (head == NULL)
            return true; 
        if (root == NULL)
            return false;

 
        if (root->val == head->val) {
            if (solve(root->left, head->next) || solve(root->right, head->next)) {
                return true; 
            }
        }
        
        return false; 
    }

    bool isSubPath(ListNode* head, TreeNode* root) {
        if (root == NULL)
            return false; 
        if (solve(root, head))
            return true;  

        // Recursively check the left and right subtrees
        return isSubPath(head, root->left) || isSubPath(head, root->right);
     }
     };

     ```
63. Longest Univalue Path
     ```
         int maxlen;
    int solve(TreeNode* root) {
        if (root == NULL)
            return 0;
        int lh=solve(root->left);
        int rh=solve(root->right);
        int l=0,r=0;
        if(root->left&&root->left->val==root->val)
        l+=lh+1;
        if(root->right&&root->right->val==root->val)
        r+=rh+1;
        maxlen=max(maxlen,l+r);
        return max(l,r);

    }
    int longestUnivaluePath(TreeNode* root) {
        maxlen = 0;
        solve(root);
        return maxlen;
    }
     ```
64. Flip Equivalent Binary Trees
     ```
          bool f(TreeNode* root1, TreeNode* root2) {
        if (root1 == NULL && root2 != NULL) {
            return false;
        }
        if (root2 == NULL && root1 != NULL) {
            return false;
        }
        if (root1 == NULL && root2 == NULL) {
            return true;
        }

        if (root1->val != root2->val)
            return false;

        bool a = f(root1->left, root2->left);
        bool b = f(root1->right, root2->right);
        bool c = f(root1->left, root2->right);
        bool d = f(root1->right, root2->left);
        return (c && d) || (a && b);
    }
    bool flipEquiv(TreeNode* root1, TreeNode* root2) { return f(root1, root2); }
     ```
65. Complete Binary Tree Inserter
    ```
           class CBTInserter {
     public:
    queue<TreeNode*> q;
    TreeNode *root = nullptr, *Ins = nullptr;
    CBTInserter(TreeNode* a) {
        root = a;
        q.push(a);

        while (1) {
            a = q.front();
            q.pop();
            Ins = a;

            if (a->left)
                q.push(a->left);
            else
                break;
            if (a->right)
                q.push(a->right);
            else
                break;
        }
    }

    int insert(int x) {
        TreeNode *a = new TreeNode(x), *par = Ins;

        if (Ins->left) {
            Ins->right = a;
            Ins = q.front();
            q.pop();
        } else
            Ins->left = a;

        q.push(a);
        return par->val;
    }

    TreeNode* get_root() { return root; }
    };
    ```
66. Construct binary tree from pre & post order
     ```
           TreeNode* build(vector<int>& pre, int preLow, int preHigh,
                    vector<int>& post, int postLow, int postHigh) {
  
        if (preLow > preHigh || postLow > postHigh)
            return NULL;
        TreeNode* root = new TreeNode(pre[preLow]);
        
        if (preLow == preHigh)
            return root;
        int i = postLow;
      
        while (i <= postHigh) {
            if (post[i] == pre[preLow + 1])
                break;
            i++;
        }
        int leftCount = i - postLow;
      
        root->left =
            build(pre, preLow + 1, preLow + leftCount + 1, post, postLow, i);
        root->right =
            build(pre, preLow + leftCount + 2, preHigh, post, i + 1, postHigh);
        return root;
    }
    TreeNode* constructFromPrePost(vector<int>& preorder,
                                   vector<int>& postorder) {
        int n = preorder.size();
        return build(preorder, 0, n - 1, postorder, 0, n - 1);
    }
     ```
67. Maximum Level Sum of a Binary Tree
    ```
       int maxLevelSum(TreeNode* root) {
        int res;
        int ans = -1e9;
        int level = 0;
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) {
            int siz = q.size();
            level++;
            long currsum = 0;
            for (int i = 0; i < siz; i++) {
                TreeNode* temp = q.front();
                q.pop();
                currsum += temp->val;
                if (temp->left != NULL) {
                    q.push(temp->left);
                }
                if (temp->right != NULL) {
                    q.push(temp->right);
                }
            }
            if (currsum > ans) {
                res = level;
                ans = currsum;
            }
        }
        return res;
    }
    ```
68. Find Nearest Right Node in Binary Tree
     ```
         TreeNode* findNearestRightNode(TreeNode* root, TreeNode* u) {
        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            int size = q.size();

            for (int i = 0; i < size; i++) {
                auto node = q.front();
                q.pop();

                if (node->val == u->val) {
                    return i == size - 1 ? NULL : q.front();
                }

                if (node->left)
                    q.push(node->left);
                if (node->right)
                    q.push(node->right);
            }
        }

        return NULL;
    }
     ```
69. Create Binary Tree From Descriptions
     ```
         TreeNode* createBinaryTree(vector<vector<int>>& descriptions) {
        unordered_map<int, TreeNode*> mp; // To store node pointers
        unordered_set<int> childNodes;    // To track which nodes are children

        for (int i = 0; i < descriptions.size(); i++) {
            int p = descriptions[i][0];  // Parent node value
            int c = descriptions[i][1];  // Child node value
            int l = descriptions[i][2];  // Is it a left child? (1 for left, 0 for right)

            // Create parent node if not already present
            if (mp.find(p) == mp.end()) {
                mp[p] = new TreeNode(p);
            }

            // Create child node if not already present
            if (mp.find(c) == mp.end()) {
                mp[c] = new TreeNode(c);
            }

            // Link parent and child based on 'l' (left/right)
            if (l) {
                mp[p]->left = mp[c];
            } else {
                mp[p]->right = mp[c];
            }

            // Mark the child node
            childNodes.insert(c);
        }

        // Find the root node (a node that is not a child of any other node)
        for (const auto& desc : descriptions) {
            int parentVal = desc[0];
            if (childNodes.find(parentVal) == childNodes.end()) {
                return mp[parentVal];
            }
        }

        return nullptr;  // Shouldn't reach here if the input is valid
    }
     ```
70. Max profitable path in a tree
     ```
        void AliceDfs(int node, int par, int dis, vector<int>& parent,
                  vector<int>& distance, vector<int> adj[]) {
        parent[node] = par;
        distance[node] = dis;
        for (auto it : adj[node]) {
            if (it != par)
                AliceDfs(it, node, dis + 1, parent, distance, adj);
        }
    }
    void BobDfs(int node, int dis, vector<int>& amount, vector<int>& distance,
                vector<int>& parent, vector<int> adj[]) {
        if (dis < distance[node])
            amount[node] = 0;
        if (dis == distance[node])
            amount[node] /= 2;
        if (node == 0)
            return;
        BobDfs(parent[node], dis + 1, amount, distance, parent, adj);
    }
    int CollectAmount(int node, vector<int>& amount, vector<int>& visited,
                      vector<int> adj[]) {
        visited[node] = 1;
        int res = amount[node];
        int maxi = INT_MIN;
        for (auto it : adj[node]) {
            if (visited[it] == 0)
                maxi = max(maxi, CollectAmount(it, amount, visited, adj));
        }
        if (maxi == INT_MIN)
            return res;
        return res + maxi;
    }
    int mostProfitablePath(vector<vector<int>>& edges, int bob,
                           vector<int>& amount) {
        int n = edges.size() + 1;
        vector<int> adj[n];
        vector<int> parent(n, 0);
        vector<int> visited(n, 0);
        vector<int> distance(n, 0);
        for (auto it : edges) {
            adj[it[0]].push_back(it[1]);
            adj[it[1]].push_back(it[0]);
        }
        AliceDfs(0, 0, 0, parent, distance, adj);
        BobDfs(bob, 0, amount, distance, parent, adj);
        return CollectAmount(0, amount, visited, adj);
    }
     ```
     ```
         void AliceDfs(int node, int par, int dis, int[] parent,
            int[] distance, List<Integer>[] adj) {
        parent[node] = par;
        distance[node] = dis;
        for (int it : adj[node]) {
            if (it != par)
                AliceDfs(it, node, dis + 1, parent, distance, adj);
        }
    }

    void BobDfs(int node, int dis, int[] amount,
            int[] distance, int[] parent, List<Integer>[] adj) {
        if (dis < distance[node])
            amount[node] = 0;
        if (dis == distance[node])
            amount[node] /= 2;
        if (node == 0)
            return;
        BobDfs(parent[node], dis + 1, amount, distance, parent, adj);
    }

    int CollectAmount(int node, int[] amount, int[] visited,
            List<Integer>[] adj) {
        visited[node] = 1;
        int res = amount[node];
        int maxi =  Integer.MIN_VALUE;
        for (int it : adj[node]) {
            if (visited[it] == 0)
                maxi = Math.max(maxi, CollectAmount(it, amount, visited, adj));

        }
        if (maxi ==  Integer.MIN_VALUE)
            return res;
        return res + maxi;
    }

    public int mostProfitablePath(int[][] edges, int bob, int[] amount) {
        int n = edges.length + 1;
        List<Integer>[] adj = new ArrayList[n];
        for (int i = 0; i < n; i++) {
            adj[i] = new ArrayList<>();
        }
        int[] parent = new int[n];
        int[] visited = new int[n];
        int[] distance = new int[n];
       for (int[] edge : edges) {
            adj[edge[0]].add(edge[1]);
            adj[edge[1]].add(edge[0]);
        }
        AliceDfs(0, 0, 0, parent, distance, adj);
        BobDfs(bob, 0, amount, distance, parent, adj);
        return CollectAmount(0, amount, visited, adj);

    }
     ```
71. Convert Binary Search Tree to Sorted Doubly Linked List
     ```
      Node* treeToDoublyList(Node* root) {

        if (!root) {
            return NULL;
        }

        vector<Node*> sortedNodes;
        inorderTraverse(root, sortedNodes);

        Node* prev = sortedNodes.back();

        for (int index = 0; index < sortedNodes.size(); index++) {
            sortedNodes[index]->left = prev;
            sortedNodes[index]->right = index < sortedNodes.size() - 1
                                            ? sortedNodes[index + 1]
                                            : sortedNodes[0];
            prev = sortedNodes[index];
        }

        return sortedNodes[0];
    }

    void inorderTraverse(Node* root, vector<Node*>& sortedNodes) {
        if (!root) {
            return;
        }

        inorderTraverse(root->left, sortedNodes);
        sortedNodes.push_back(root);
        inorderTraverse(root->right, sortedNodes);
    }
     ```
72. Convert sorted list to BST
     ```
          public TreeNode sortedListToBST(ListNode head) {
        if (head == null)
            return null;
        if (head.next == null)
            return new TreeNode(head.val);
        ListNode slow = head;
        ListNode fast = head;
        ListNode prev = null;
        while (fast != null && fast.next != null) {
            prev = slow;
            slow = slow.next;
            fast = fast.next.next;
        }
        if (prev != null) {
            prev.next = null;
        }
        TreeNode root = new TreeNode(slow.val);
        root.left = sortedListToBST(head);
        root.right = sortedListToBST(slow.next);
        return root;

    }
      ```
      ```
            private ListNode current;

    public TreeNode sortedListToBST(ListNode head) {
        if (head == null)
            return null;

        int size = getSize(head);
        current = head;

        return buildTree(0, size - 1);
    }

    private int getSize(ListNode head) {
        int size = 0;
        while (head != null) {
            size++;
            head = head.next;
        }
        return size;
    }

    private TreeNode buildTree(int start, int end) {
        if (start > end)
            return null;

        int mid = start + (end - start) / 2;

        TreeNode left = buildTree(start, mid - 1);

        TreeNode root = new TreeNode(current.val);
        current = current.next;

        TreeNode right = buildTree(mid + 1, end);

        root.left = left;
        root.right = right;

        return root;
    }
      ```
73. Construct binary tree from string 
     ```
        public TreeNode str2tree(String s) {
        Stack<TreeNode> stack = new Stack<>();
        for(int i = 0, j = i; i < s.length(); i++, j = i){
            char c = s.charAt(i);
            if(c == ')')    stack.pop();
            else if(c >= '0' && c <= '9' || c == '-'){
                while(i + 1 < s.length() && s.charAt(i + 1) >= '0' && s.charAt(i + 1) <= '9') i++;
                TreeNode currentNode = new TreeNode(Integer.valueOf(s.substring(j, i + 1)));
                if(!stack.isEmpty()){
                    TreeNode parent = stack.peek();
                    if(parent.left != null)    parent.right = currentNode;
                    else parent.left = currentNode;
                }
                stack.push(currentNode);
            }
        }
        return stack.isEmpty() ? null : stack.peek();
    }
     ```
74. Binary Tree Pruning
      ```
          bool checkOne(TreeNode* root) {
        if(!root)
            return false;
        
        if(root->val == 1)
            return true;
        
        return checkOne(root->left) || checkOne(root->right);
    }
    
    TreeNode* pruneTree(TreeNode* root) {
        if(!root)
            return NULL;
        
        pruneTree(root->left);
        pruneTree(root->right);
        
        if(!checkOne(root->left))  root->left = NULL;
        if(!checkOne(root->right)) root->right = NULL;
        
        
        if(!root->left && !root->right && root->val == 0)
            return NULL;
        return root;
    }
      ```
      ```
          TreeNode* pruneTree(TreeNode* root) {
        if (root == NULL) {
            return NULL;
        }
        root->left = pruneTree(root->left);
        root->right = pruneTree(root->right);
        if (root->val == 0 && root->left == NULL && root->right == NULL) {
            return NULL;
        }

        return root;
    }
      ```
75. Delete node in a BST
     ```
         class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null)
            return null;

        if (key < root.val) {
            root.left = deleteNode(root.left, key);
            return root;
        }

        else if (key > root.val) {
            root.right = deleteNode(root.right, key);
            return root;
        }

        else {
            if (root.left == null) {
                return root.right;
            } else if (root.right == null) {
                return root.left;
            } else {
                TreeNode min = root.right;
                while (min.left != null) {
                    min = min.left;
                }

                root.val = min.val;
                root.right = deleteNode(root.right, min.val);
                return root;

            }
        }
    }
    }
     ```
76. Smallest Subtree with all the Deepest Nodes
   ```
       TreeNode* subtreeWithAllDeepest(TreeNode* root) {
        return findDeepest(root).second;
    }
    pair<int, TreeNode*> findDeepest(TreeNode* root) {
        if (root == nullptr) {
            return {0, nullptr};
        }

        auto left = findDeepest(root->left);
        auto right = findDeepest(root->right);

        if (left.first == right.first) {

            return {left.first + 1, root};
        } else if (left.first > right.first) {

            return {left.first + 1, left.second};
        } else {

            return {right.first + 1, right.second};
        }
    }
   ```
77. Balance a Binary search Tree
     ```
         vector<TreeNode*> sortedArr;
    TreeNode* balanceBST(TreeNode* root) {
        inorderTraverse(root);
        return sortedArrayToBST(0, sortedArr.size() - 1);
        
    }
    void inorderTraverse(TreeNode* root) {
        if (root == NULL) return;
        inorderTraverse(root->left);
        sortedArr.push_back(root);
        inorderTraverse(root->right);
    }
    TreeNode* sortedArrayToBST(int start, int end) {
        if (start > end) return NULL;
        int mid = (start + end) / 2;
        TreeNode* root = sortedArr[mid];
        root->left = sortedArrayToBST(start, mid - 1);
        root->right = sortedArrayToBST(mid + 1, end);
        return root;
    }
     ```
78. Amount of Time for Binary Tree to Be Infected
     ```
         unordered_map<int, vector<int>> graph;
    int amountOfTime(TreeNode* root, int start) {
        constructGraph(root);
        queue<int> q;
        q.push(start);

        unordered_set<int> visited;

        int minutesPassed = -1;

        while (!q.empty()) {
            ++minutesPassed;
            for (int levelSize = q.size(); levelSize > 0; --levelSize) {
                int currentNode = q.front();
                q.pop();
                visited.insert(currentNode);
                for (int adjacentNode : graph[currentNode]) {
                    if (!visited.count(adjacentNode)) {
                        q.push(adjacentNode);
                    }
                }
            }
        }

        return minutesPassed;
    }
    void constructGraph(TreeNode* root) {
        if (!root)
            return;

        if (root->left) {
            graph[root->val].push_back(root->left->val);
            graph[root->left->val].push_back(root->val);
        }

        if (root->right) {
            graph[root->val].push_back(root->right->val);
            graph[root->right->val].push_back(root->val);
        }

        constructGraph(root->left);
        constructGraph(root->right);
    }
     ```
79. Trim BST
     ```
        TreeNode* trimBST(TreeNode* root, int L, int R) {
        if (!root) return root;
        if (root->val >= L && root->val <= R) {
            root->left = trimBST(root->left, L, R);
            root->right = trimBST(root->right, L, R);
            return root;
        }
        if (root->val < L)
            return trimBST(root->right, L, R);
        return trimBST(root->left, L, R);
        
    }
     ```
80. Construct Quad Tree
      ```
            bool isAllSame(vector<vector<int>>& grid, int x, int y, int n) {
        int val = grid[x][y];

        for (int i = x; i < x + n; i++) {
            for (int j = y; j < y + n; j++) {
                if (grid[i][j] != val)
                    return false;
            }
        }

        return true;
    }
    Node* solve(vector<vector<int>>& grid, int x, int y, int n) {
        if (isAllSame(grid, x, y, n)) {
            return new Node(grid[x][y], true);
        } else {
            Node* root = new Node(1, false);

            root->topLeft = solve(grid, x, y, n / 2);
            root->topRight = solve(grid, x, y + n / 2, n / 2);
            root->bottomLeft = solve(grid, x + n / 2, y, n / 2);
            root->bottomRight = solve(grid, x + n / 2, y + n / 2, n / 2);

            return root;
        }
    }
    Node* construct(vector<vector<int>>& grid) {
        return solve(grid, 0, 0, grid.size());
    }
      ```
81. split BST
      ```
              vector<TreeNode*> splitBST(TreeNode* root, int V) {
        vector<TreeNode *> res(2, NULL);
        if(root == NULL) return res;
        
        if(root->val > V){
            res[1] = root;
            auto res1 = splitBST(root->left, V);
            root->left = res1[1];
            res[0]=res1[0];
        }else{
            res[0] = root;
            auto res1 = splitBST(root->right, V);
            root->right = res1[0];
            res[1] = res1[1];
        }
        
        return res;
        
       }
      ```
82. LCA IV
     ```
           TreeNode* lowestCommonAncestor(TreeNode* root, vector<TreeNode*> &nodes) {
        unordered_set<TreeNode*> targets(nodes.begin(), nodes.end());
        auto lca =  dfs(root, targets);
        return lca;
    }

    TreeNode* dfs(TreeNode* node, const unordered_set<TreeNode*> targets) {
        if (node == nullptr) return nullptr;
        if (targets.contains(node)) return node;

        auto leftNode = dfs(node->left, targets);
        auto rightNode = dfs(node->right, targets);
        if (leftNode && rightNode) return node;
        return leftNode ? leftNode : rightNode;
    }
     ```
83. Operations on Tree
    ```
            unordered_map<int, vector<int>> descendents;
    vector<vector<int>> Node;
    int n;
    LockingTree(vector<int>& parent) {
        n = parent.size();
        Node.resize(n, vector<int>(2, -1));
        Node[0][0] = -1;
        for (int i = 1; i < n; i++) {
            Node[i][0] = parent[i];
            descendents[parent[i]].push_back(i);
        }
    }

    bool lock(int num, int user) {
        if (Node[num][1] != -1)
            return false;

        Node[num][1] = user;
        return true;
    }

    bool unlock(int num, int user) {
        if (Node[num][1] != user)
            return false;

        Node[num][1] = -1;
        return true;
    }
    void checkDescendents(int num, bool& atleastOne) {
        if(descendents.count(num) == 0 || descendents[num].size() == 0)
            return;
        
        for(int& x : descendents[num]) {
            if(Node[x][1] != -1) {
                atleastOne = true;
                return;
            }
            checkDescendents(x, atleastOne);
        }
    }
    bool IsAnyAncestorLocked(int& num) {
        if(num == -1)
            return false; 
        
        return Node[num][1] != -1 || IsAnyAncestorLocked(Node[num][0]);
    }
    
    void unlockDescendents(int num) {
        if(descendents.count(num) == 0 || descendents[num].size() == 0)
            return;
        
        for(int& x : descendents[num]) {
            Node[x][1] = -1;
            unlockDescendents(x);
        }
    }

    bool upgrade(int num, int user) {
        if(Node[num][1] != -1) return false;
        bool atleastOne = false;
        checkDescendents(num, atleastOne);
         if(!atleastOne) return false;
           if(IsAnyAncestorLocked(Node[num][0])) return false;
            unlockDescendents(num);
        Node[num][1] = user;
        return true;
    }
    ```

