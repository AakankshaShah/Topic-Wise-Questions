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

16. Remove K digits to get max https://leetcode.com/problems/remove-k-digits/
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

24. Remove duplicate letter https://leetcode.com/problems/remove-duplicate-letters/

## Extras 

Prefix , postfix & infix conversion
