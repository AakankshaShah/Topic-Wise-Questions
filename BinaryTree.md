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
        int height(TreeNode * node) {
            if (node == null)
                return 0;
            int lh = height(node->left);
            int rh = height(node->right);
            return 1 + max(lh, rh);
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
22. Balanced binary tree
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
23. Maximum path sum
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
24. Maximum Width 
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
25. Distribute coins in a bt
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
26. Binary tree paths https://www.youtube.com/watch?v=gSFcPOPyq-Y
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
27. Binary Tree zig zag traversal 
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
28. Serialize  deserailize
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
29. Path Sum 
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
30. Bottom value of binary tree
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
31. Minimum Absolute Difference in BST
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
32. Graph valid tree
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
33. Deepest leave sum
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
34. Height of Binary Tree After Subtree Removal Queries
   ```
       class Solution {
public:
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
};
   ```
35. Flatten a linked list 
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
36. Construct binary tree from inorder & post-order
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
37. LCA II
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
38. LCA III
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
39. Sum root to leaf numbers
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
40. Right & left view of BT
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
41. Inorder successor in a bst 
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
42. Max binary tree
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
43. Lonley nodes 
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
44. Count number of good nodes
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
45. Max product of splitted array 
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
46. Closest binary tree value 
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
47. Largest BST subtree
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
48. Two sum in a bst 
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
