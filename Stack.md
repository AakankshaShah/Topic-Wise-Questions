# Stack



## Theory

https://www.youtube.com/playlist?list=PL_z_8CaSLPWdeOezg68SKkeLN4-T_jNHd


## Questions

1. Nearest Greatest Element towards left/right
2. Nearest Smallest  Element towards left/right
3. Stock span
4. Max area under histogram
   ```
    stack<int> s;
        stack<int> s1;
        int len = height.size();
        vector<int> l(len);
        vector<int> r(len);

        for (int i = 0; i < len; i++) {
            while (s.size() > 0 && height[s.top()] >= height[i])
                s.pop();
            if (s.empty())
                l[i] = -1;
            else
                l[i] = s.top();
            s.push(i);
        }

        for (int i = len - 1; i >= 0; i--) {

            while (s1.size() > 0 && height[s1.top()] >= height[i])
                s1.pop();
            if (s1.empty())
                r[i] = len;
            else
                r[i] = s1.top();
            s1.push(i);
        }
        int ans = 0;
        for (int i = 0; i < len; i++) {
            int width = r[i] - l[i] - 1;
            int area = height[i] * width;
            ans = max(ans, area);
        }
        return ans;
    }
   ```
   ```
     stack<int> s;
        int max_area = 0;
        int tp;
        int n = heights.size();
        int i = 0;
        while (i < n) {
            if (s.empty() || heights[s.top()] <= heights[i])
                s.push(i++);

            else {
                tp = s.top();
                s.pop();
                max_area = max(max_area,
                               heights[tp] * (s.empty() ? i : i - s.top() - 1));
            }
        }
        while (s.empty() == false) {
            tp = s.top();
            s.pop();
            max_area =
                max(max_area, heights[tp] * (s.empty() ? i : i - s.top() - 1));
        }

        return max_area;
    }
   ```
5. Max area of the rectangle in a binary matrix
    ```
      int MAH(int arr[], int n) {
        if (n == 1)
            return arr[0];

        stack<int> s;
        int i = 0;
        int res = 0;
        while (i < n) {
            if (s.empty() || arr[s.top()] <= arr[i])
                s.push(i++);

            else {

                int val = s.top();
                s.pop();

                res = max(res, arr[val] * (s.empty() ? i : i - s.top() - 1));
            }
        }
        while (s.size() > 0) {
            int val = s.top();
            s.pop();
            res = max(res, arr[val] * (s.empty() ? i : i - s.top() - 1));
        }
        return res;
    }

    int maximalRectangle(vector<vector<char>>& matrix) {
        int n = matrix.size();
        int m = matrix[0].size();
        int arr[m];
        for (int j = 0; j < m; j++) {
            arr[j] = 0;
        }
        int ans = 0;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (matrix[i][j] == '0')
                    arr[j] = 0;
                else
                    arr[j] = arr[j] + 1;
            }

            ans = max(ans, MAH(arr, m));
        }
        return ans;
    }
    ```
6. Rainwater trapping
    ```
      int i=0;
       int j=n-1;
       int l_max=0;
       int r_max=0;
       int ans=0;
       
       while(i<=j)
       {
           if(r_max<l_max)
           {
               ans+=max(0,r_max-arr[j]);
               r_max=max(r_max,arr[j]);
               j--;
           }
           else
           {
                ans+=max(0,l_max-arr[i]);
               l_max=max(l_max,arr[i]);
               i++;
           }
       }
       return ans;
    ```
    ```
     int n = height.size() - 1;
        int maxL[n+1];
        int maxR[n+1];
        maxL[0] = height[0];
        maxR[n] = height[n];
        int res=0;
        for (int i = 1; i < height.size(); i++) {
            maxL[i] = max(maxL[i - 1], height[i]);
        }
        for (int i = n-1; i >= 0; i--) {
            maxR[i] = max(maxR[i + 1], height[i]);
        }
        for (int i = 1; i < height.size(); i++) {
            res+=max(0,min(maxL[i],maxR[i])-height[i]);
        }
        return res;
    ```
7. Min Stack
8. Celebrity Problem
   ```
    int celebrity(int M[N][N], int n)
    {
    
 
    int i = 0, j = n - 1;
    while (i < j) {
        if (M[j][i] == 1) // j knows i
            j--;
        else 
            i++;
    }
    
    int candidate = i;
 
    
    for (i = 0; i < n; i++) {
        if (i != candidate) {
            if (M[i][candidate] == 0
                || M[candidate][i] == 1)
                return -1;
        }
    }
    
    return candidate;
   }
   ```
9. Valid Parentheses 

    ```
      bool isValid(string s) {
        stack<char> st;

        for (int i = 0; i < s.length(); i++) {
            if (s[i] == '(' || s[i] == '{' || s[i] == '[')
                st.push(s[i]);

            else if (s[i] == ')') {
                if (!(st.size() > 0 && st.top() == '('))
                    return false;
                st.pop();
            } else if (s[i] == '}') {
                if (!(st.size() > 0 && st.top() == '{'))
                    return false;
                st.pop();
            } else if (s[i] == ']') {
                if (!(st.size() > 0 && st.top() == '['))
                    return false;
                st.pop();
            }
        }

        return st.size() == 0;
    }
    ```
10. Longest Valid Paratheses* 
    ```
       int longestValidParentheses(string s) {
        stack<int> st;
        st.push(-1);
        int res = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s[i] == '(')
                st.push(i);
            else {
                if (!st.empty())
                    st.pop();

                if (!st.empty())//Stores the last invalid index
                    res = max(res, i - st.top());

                else
                    st.push(i);
            }
        }
        return res;
    }
    ```
11. Simplify Path* 

    ```
     stack<string> s;
        int len = path.length() - 1;
        int n = len;
        int i=0;

        while (i <= len) {
            if (path[i] == '/')
                i++;
            else {
                string w = "";
                while (path[i] != '/' && i <= len) {
                    w += path[i++];
                }
                if (w == "..") {
                    if (!s.empty())
                        s.pop();
                } else if (w == ".")
                    continue;

                else
                    s.push(w);
            }
        }
        string ans = "";
        if (s.empty())
            return "/";
        while (!s.empty()) {
            ans = +"/" + s.top() + ans;
            s.pop();
        }
        return ans;
    ```
12. Polish Notation https://leetcode.com/problems/evaluate-reverse-polish-notation
   Make use of stoi to convert string to int 
13. Basic Calculator** https://leetcode.com/problems/basic-calculator
14. Basic Calculator II https://leetcode.com/problems/basic-calculator-ii/
15. Longest Absolute File path https://leetcode.com/problems/longest-absolute-file-path/

``` 
public int lengthLongestPath(String input) {
        if (input == null || input.length() == 0) {
            return 0;
        }
        String[] tokens = input.split("\n");
        int[] dirs = new int[tokens.length];
        int max = 0;
        int curr;
        for (int i = 0; i < tokens.length; i ++) {
            String s = tokens[i];
            int count = s.lastIndexOf("\t") + 1;
            s = s.substring(count);
            if (s.indexOf(".") != -1) {
                //file
                int cand = s.length();
                if (count != 0) {
                    cand = dirs[count - 1] + s.length();
                }
                max = Math.max(max, cand);
            }
            else {
                //folder
                dirs[count] = s.length() + 1;
                if (count != 0) {
                    dirs[count] += dirs[count - 1];
                }
            }
        }
        return max;
        
    }
} 
```
```
 string[] tokens = input.split("\n");

     for (int i = 0; i < tokens.length; i ++) 
     {
         int level=tokens[i].lastIndexof("\t")+1;
         int len=tokens[i].length()-level;

         while(s.size()>level+1)
         s.pop();

         if(tokens[i].Indexof(".")!=-1)
         {
             maxL=max(maxL,s.top()+len)

         }
         else
         s.push(s.top()+len);
     }
     return maxL;
```

16. Remove K digits to get max https://leetcode.com/problems/remove-k-digits/
    ```
      int n = num.length();
        if (k >= n)
            return "0";

        stack<char> s; // Can use string also instead of stack

        for (int i = 0; i < n; i++) {
            if (s.empty())
                s.push(num[i]);

            else {
                while (!s.empty() && k > 0 && s.top() > num[i]) {
                    s.pop();
                    k--;
                }
                s.push(num[i]);
            }
        }

        while (k > 0 && s.size() > 0) {
            s.pop();
            k--;
        }

        if (s.size() == 0)
            return "0";

        string ans = "";
        while (!s.empty()) {
            ans += s.top();
            s.pop();
        }
        reverse(ans.begin(), ans.end());
        int i = 0;
        while (ans[i] == '0')
            i++;

        return ans.substr(i).length() > 0 ? ans.substr(i) : "0";
    ```
17. Parsing a boolean expression https://leetcode.com/problems/parsing-a-boolean-expression/
18. Minimum remove to make valid parantheses https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/
19. Check if Parathesis string can be made valid or not** https://leetcode.com/problems/check-if-a-parentheses-string-can-be-valid/
 ```
 int flip=0;
        int op=0;
        int cp=0;

        int n=s.length();

        if(n%2!=0)
        return false;

        for(int i=0;i<n;i++)
        {
            if(locked[i]=='0')
            flip++;
            else
            {
                if(s[i]=='(')
                op++;
                else
                cp++;
            }
           
        if((flip+op)<cp)
        return false; 
        }

        flip=0;
         op=0;
         cp=0;

        for(int i=n-1;i>=0;i--)
        {
            if(locked[i]=='0')
            flip++;

            else
            {
                if(s[i]=='(')
                op++;
                else
                cp++;
            }
          
        if((flip+cp)<op)
        return false;  
        } 

       
        return true;  
```

20 . Finding the Number of Visible Mountains https://leetcode.com/problems/finding-the-number-of-visible-mountains/

```
vector<vector<int>> dis;

        for(int i =0;i<peaks.size();i++)
        {
            dis.push_back({peaks[i][0]-peaks[i][1],peaks[i][0]+peaks[i][1]});
        }

        sort(dis.begin(),dis.end());
        int i=0;
        int size=0;

        while(i<dis.size())
        {
            size++;

            if(i==dis.size()-1)
            return size;

            if(dis[i][0]==dis[i+1][0])
            size--;
int curr=dis[i][1];
            while(i+1<dis.size()&&curr>=dis[i+1][1])
            {
                i++;
            }
            i++;
        }
        return size;

```
21. Implement k stacks single array https://www.geeksforgeeks.org/efficiently-implement-k-stacks-single-array/
22. Redundant braces https://www.geeksforgeeks.org/expression-contains-redundant-bracket-not/
23. LRU Cache

  ``` LRUCache(int cap)
    {
       capa=cap;
       head->next=tail;
       tail->prev=head;
    }
    
    void addN(node* newnode)
    {
        node* temp=head->next;
        newnode->next=temp;
        newnode->prev=head;
        head->next=newnode;
        temp->prev=newnode;
    }
    
    void deleteN(node* delnode )
    {
        node* tem=delnode->next;
        node *temp=delnode->prev;
        temp->next=tem;
        tem->prev=temp;
    }
    
    //Function to return value corresponding to the key.
    int get(int key)
    {
        
        if(m.find(key)!=m.end())
        {
            node* res=m[key];
            int result=res->val;
            m.erase(key);
             deleteN(res);
             addN(res);
             m[key]=head->next;
            return result;
           
        }
       else
       return -1;
       
       
       
       
    }
    
    //Function for storing key-value pair.
    void set(int key, int value)
    {
        // your code here 
        if(m.find(key)!=m.end())
        {
            node* resn=m[key];
            m.erase(key);
            deleteN(resn);
        }
        if(m.size()==capa)
        {
            m.erase(tail->prev->key);
            deleteN(tail->prev);
        }
        addN(new node(key,value));
        m[key]=head->next;
    }

```

24. Remove duplicate letter 
     ```
      vector<int> lastIndex(26, 0);
        for (int i = 0; i < s.length(); i++){
            lastIndex[s[i] - 'a'] = i; 
        }
        
        vector<bool> seen(26, false); 
        stack<char> st;
        
        for (int i = 0; i < s.size(); i++) {
            int curr = s[i] - 'a';
            
            if (seen[curr]){
                 continue;
            }
            
            while(st.size() > 0 && st.top() > s[i] && i < lastIndex[st.top() - 'a']){
                seen[st.top() - 'a'] = false; 
                st.pop();
            }
            st.push(s[i]); 
            seen[curr] = true; 
        }
        
        string res = "";
        while (st.size() > 0){
            res += st.top();
            st.pop();
        }
        reverse(res.begin(), res.end());
        return res;
        
     ```
25. Palindrome linked list 
    ```
       stack<int> s;
        ListNode* curr = head;
        while (curr != NULL) {
            s.push(curr->val);
            curr = curr->next;
        }
        curr = head;
        while (curr != NULL && curr->val == s.top()) {
            curr = curr->next;
            s.pop();
        }
        return curr == NULL;
    ```
    ```
      class Solution {
    public boolean isPalindrome(ListNode head) {
        ListNode slow = head, fast = head;
        int len = 0;
        for(ListNode curr = head; curr != null; curr = curr.next){
            len++;
        }
        //get hold of mid of linked list
        while(fast != null &&  fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        if(len % 2 != 0)slow = slow.next;
        ListNode secondHlfHead = reverse(slow);
        while(head != null && secondHlfHead != null){
            if(head.val != secondHlfHead.val)return false;
            head = head.next;
            secondHlfHead = secondHlfHead.next;
        }
        return true;
    }
    private ListNode reverse(ListNode head){
        ListNode curr = head, prev = null;
        while(curr != null){
            ListNode nextNode = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nextNode;
        }
        return prev;
    }
    }
    ```
26. Decode string 
     ```
        stack<int> st1;
        stack<char> st2;

        string fans = "";
        int n = s.length();

        for (int i = 0; i < n; i++) {

            if (s[i] >= '0' && s[i] <= '9') {
                int num = 0;
                while (i < n && s[i] >= '0' && s[i] <= '9') {
                    num = num * 10 + (s[i] - '0');
                    i++;
                }

                st1.push(num);
                i--;
            }

            else if (s[i] == ']') {
                string var = "";
                string ans = "";
                while (st2.size() > 0 && st2.top() != '[') {
                    var = st2.top() + var;
                    st2.pop();
                }
                st2.pop();

                for (int j = 1; j <= st1.top(); j++) {
                    ans += var;
                }

                for (int it = 0; it < ans.size(); it++) {
                    st2.push(ans[it]);
                }
                st1.pop();
            } else {
                st2.push(s[i]);
            }
        }
        string rest;
        while (!st2.empty()) {
            char c = st2.top();
            rest.push_back(c);
            st2.pop();
        }
        reverse(rest.begin(), rest.end());

        fans = fans + rest;

        return fans;
     ```
27. Maximum ramp width
    ```
    //Method 1
       int n = A.size();
        vector<int> rMax(n);
        rMax[n - 1] = A[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            rMax[i] = max(rMax[i + 1], A[i]);
        }
        int left = 0, right = 0;
        int ans = 0;
        while (right < n) {
            while (left < right && A[left] > rMax[right]) {
                left++;
            }
            ans = max(ans, right - left);
            right++;
        }
        return ans;
    ```
    ```
    //Method 2
      int n = A.size();
        vector<int> b(n);
        for (int i = 0; i < n; i++) {
            b[i] = i;
        }
        sort(b.begin(), b.end(), [&A](int i, int j) {
            return A[i] < A[j];
        });

        int mn = n;
        int ans = 0;
        for (int i = 0; i < n; i++) {
            ans = max(ans, b[i] - mn);
            mn = min(mn, b[i]);
        }
        return ans;
    ```
    ```
      stack<int> st;
        for (int i = 0; i < n; i++) {
            if (st.empty() || A[i] < A[st.top()]) {
                st.push(i);
            }
        }

        int ans = 0;

        // Best answer would be smaller than current value and farther away
        for (int i = n - 1; i >= 0; i--) {
            while (!st.empty() && A[i] >= A[st.top()]) {
                ans = max(i - st.top(), ans);
                st.pop();
            }
        }

        return ans;
    ```

## Extras 

Prefix , postfix & infix conversion
