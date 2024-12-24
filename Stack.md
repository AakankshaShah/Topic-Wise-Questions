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
```
class MinStack {
public:
    stack<long long > s;
    int minEle;
    MinStack() {}

    void push(int x) {
        if (s.empty()) {
            minEle = x;
            s.push(x);
        } else if (x < minEle) {
            s.push((long long)2 * x - minEle);
            minEle = x;
        } else {
            s.push(x);
        }
    }

    void pop() {
        if (s.empty()) {
            return;
        }

        int t = s.top();
        s.pop();

        if (t < minEle) {
            minEle = 2 * minEle - t;
        }
    }

    int top() {
        if (s.empty()) {
            return -1;  // or throw an exception
        }

        int t = s.top();

        return (t < minEle) ? minEle : t;
    }

    int getMin() {
        if (s.empty()) {
            return -1;  // or throw an exception
        } else {
            return minEle;
        }
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(val);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */

```
```
  class MinStack {
public:
    vector< pair<int,int> > s;
	
    MinStack() { }
    
    void push(int val) {
        if(s.empty())
            s.push_back({val,val});
        else
            s.push_back({val,min(s.back().second,val)});    
    }
    
    void pop() { s.pop_back(); }
    
    int top() { return s.back().first; }
    
    int getMin() { return s.back().second; }
};
```
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
    ```
      public boolean isValid(String s) {
        Stack<Character> st = new Stack<>();

        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(' || s.charAt(i) == '{' || s.charAt(i) == '[')
                st.push(s.charAt(i));

            else if (s.charAt(i) == ')') {
                if (!(st.size() > 0 && st.peek() == '('))
                    return false;
                st.pop();
            } else if (s.charAt(i) == '}') {
                if (!(st.size() > 0 && st.peek() == '{'))
                    return false;
                st.pop();
            } else if (s.charAt(i) == ']') {
                if (!(st.size() > 0 && st.peek() == '['))
                    return false;
                st.pop();
            }
        }

        return st.size() == 0;

    }
    ```
10. Longest Valid Paratheses
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
11. Simplify Path

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
    ```
        public String simplifyPath(String path) {
        Stack<String> s = new Stack<>();
        StringBuilder a = new StringBuilder();
        int n = path.length();
        int i = 0;

        while (i < n) {

            while (i < n && path.charAt(i) == '/') {
                i++;
            }

            while (i < n && path.charAt(i) != '/') {
                a.append(path.charAt(i));
                i++;
            }

            String segment = a.toString();

            if (segment.equals(".")) {

            } else if (segment.equals("..")) {

                if (!s.isEmpty()) {
                    s.pop();
                }
            } else if (!segment.isEmpty()) {

                s.push(segment);
            }

            a.setLength(0);
        }

        StringBuilder ans = new StringBuilder();
        while (!s.isEmpty()) {
            ans.insert(0, "/" + s.pop());
        }

        return ans.length() > 0 ? ans.toString() : "/";
    }
    ```
12. Polish Notation 
   Make use of stoi to convert string to int 
13. Basic Calculator
    ```
      bool isDigit(char c)
    {
    if(c>='0'&&c<='9')
    return true;

    return false;
    }

    int calculate(string s) {
        
        stack<int>st;
        int num=0;
        int res=0;
        int sign=1;

        for(int i=0;i<s.length();i++)
        {
            if(isDigit(s[i]))
            num=num*10+(s[i]-'0');

        
        else if(s[i]=='+')
        {
            res+=(num*sign);
            num=0;
            sign=1;
        }
        else if(s[i]=='-')
        {
            res+=(num*sign);
            num=0;
            sign=-1;
        }
        else if(s[i]=='(')
        {
            st.push(res);
            st.push(sign);
            res=0;
            num=0;
            sign=1;
        }
        else if(s[i]==')')
        {
            res+=(num*sign);
            num=0;
            int stack_sign=st.top();
            st.pop();
            int last_res=st.top();
            st.pop();
            res*=stack_sign;
            res+=last_res;

        }
        }
        res+=(num*sign);
        return res;
        

        
    }
    ```
14. Basic Calculator II 
    ```
      int applyCalc(int a, char c, int b) {
        if (c == '+')
            return a + b;
        if (c == '-')
            return b - a;
        if (c == '*')
            return a * b;
        if (c == '/')
            return b / a;
        return 0;
    }
    int precedence(char c) {
        if (c == '+')
            return 1;
        if (c == '-')
            return 1;
        if (c == '*')
            return 2;
        if (c == '/')
            return 2;
        return 0;
    }
    int calculate(string s) {
        stack<int> operand;
        stack<char> operators;

        bool f = false;
        for (int i = 0; i < s.length(); i++) {

            if (s[i] == ' ') {
                continue;
            }

            else if (s[i] == '(') {
                operators.push(s[i]);
            } else if (s[i] >= '0' && s[i] <= '9') {
                int val = 0;

                while (i < s.length() && s[i] >= '0' && s[i] <= '9') {
                    val = (val * 10) + (s[i] - '0');
                    i++;
                }

                operand.push(val);

                i--;

            } else if (s[i] == ')') {
                while (operators.size() > 0 && operators.top() != '(' &&
                       operand.size() >= 2) {
                    int a = operand.top();
                    operand.pop();
                    int b = operand.top();
                    operand.pop();
                    char c = operators.top();
                    operators.pop();
                    operand.push(applyCalc(a, c, b));
                }
                operators.pop();

            } else if (s[i] == '+' || s[i] == '-' || s[i] == '*' ||
                       s[i] == '/') {
                f = true;

                while (operators.size() > 0 && operand.size() >= 2 &&
                       precedence(operators.top()) >= precedence(s[i])) {
                    int a = operand.top();
                    operand.pop();
                    cout << a;
                    int b = operand.top();
                    operand.pop();
                    cout << b;
                    char c = operators.top();
                    operators.pop();
                    cout << c;
                    operand.push(applyCalc(a, c, b));
                }
                operators.push(s[i]);
            }
        }

        while (operators.size() > 0 && operand.size() >= 2) {
            int a = operand.top();
            operand.pop();

            int b = operand.top();
            operand.pop();

            char c = operators.top();
            operators.pop();

            operand.push(applyCalc(a, c, b));
        }

        return operand.top();
    }
    ```
    ```
       private int applyCalc(int a, char c, int b) {
        switch (c) {
            case '+':
                return a + b;
            case '-':
                return b - a; // Note the order
            case '*':
                return a * b;
            case '/':
                return b / a; // Note the order
            default:
                return 0;
        }
    }

    private int precedence(char c) {
        if (c == '+' || c == '-') {
            return 1;
        }
        if (c == '*' || c == '/') {
            return 2;
        }
        return 0;
    }

    public int calculate(String s) {
        Stack<Integer> operand = new Stack<>();
        Stack<Character> operators = new Stack<>();

        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);

            if (ch == ' ') {
                continue;
            }

            else if (ch == '(') {
                operators.push(ch);
            }

            else if (Character.isDigit(ch)) {
                int val = 0;

                while (i < s.length() && Character.isDigit(s.charAt(i))) {
                    val = (val * 10) + (s.charAt(i) - '0');
                    i++;
                }

                operand.push(val);
                i--;
            }

            else if (ch == ')') {
                while (!operators.isEmpty() && operators.peek() != '(') {
                    int a = operand.pop();
                    int b = operand.pop();
                    char op = operators.pop();
                    operand.push(applyCalc(a, op, b));
                }
                operators.pop();
            }

            else if (ch == '+' || ch == '-' || ch == '*' || ch == '/') {
                while (!operators.isEmpty() && precedence(operators.peek()) >= precedence(ch)) {
                    int a = operand.pop();
                    int b = operand.pop();
                    char op = operators.pop();
                    operand.push(applyCalc(a, op, b));
                }
                operators.push(ch);
            }
        }

        while (!operators.isEmpty()) {
            int a = operand.pop();
            int b = operand.pop();
            char op = operators.pop();
            operand.push(applyCalc(a, op, b));
        }

        return operand.pop();
    }
    ```
15. Basic Calculator III  
    ```
       private:
       void operation(int& res, int& prev, int& num, char& sign) {
        if (sign == '+' || sign == '-') {
            num = (sign == '+') ? num : -num;
            res += prev;
            prev = num;
        } else if (sign == '*' || sign == '/')
            prev = (sign == '*') ? prev * num : prev / num;
        else
            prev = num;
    }

    public:
    int calculate(string s) {
        stack<int> results, prevNums;
        stack<char> signs;

        char sign = '\0';
        int len = s.size(), res, prev, num;
        res = prev = num = 0;

        for (int i = 0; i < len; i++) {
            if (isdigit(s[i]))
                num = 10 * num + (s[i] - '0');

            if (!isdigit(s[i]) || i == len - 1) {
                if (s[i] == '(') {
                    results.push(res);
                    prevNums.push(prev);
                    signs.push(sign);

                    sign = '\0';
                    res = 0;
                } else if (s[i] == ')') {
                    operation(res, prev, num, sign);
                    num = 0;
                    sign = '+';
                    operation(res, prev, num, sign);

                    num = res;
                    sign = signs.top(), prev = prevNums.top(),
                    res = results.top();
                    signs.pop(), prevNums.pop(), results.pop();
                    if (i == len - 1)
                        operation(res, prev, num, sign);
                } else {
                    operation(res, prev, num, sign);
                    sign = s[i];
                    num = 0;
                }
            }
        }

        res += prev;

        return res;
    }
    ```

16. Longest Absolute File path 


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

17. Remove K digits to get max 
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
18. Parsing a boolean expression 
19. Minimum remove to make valid parantheses 
    ```
      string minRemoveToMakeValid(string s) 
    {
        int n=s.length();
        int ob=0;
        int cb=0;

        for(int i=0;i<n;i++)
        {
            
            if(s[i]=='(')
            ob++;
            if(s[i]==')')
            {
                if(ob==0)
                s[i]='#';
                else
                ob--;
            }
        }
        ob=0;
        for(int i=n-1;i>=0;i--)
        {
            
            if(s[i]==')')
            ob++;
            if(s[i]=='(')
            {
                if(ob==0)
                s[i]='#';
                else
                ob--;
            }
        }
        string ans="";
        for(int i=0;i<n;i++)
        {
            if(s[i]!='#')
            ans+=s[i];

        }
        return ans;
    }
    ```
    ```
       public String minRemoveToMakeValid(String s) {
        int n = s.length();
        int ob = 0;
        int cb = 0;
        int i = 0;
        char[] c = s.toCharArray();

        while (i < n) {
            if (s.charAt(i) == '(')
                ob++;
            if (s.charAt(i) == ')') {
                if (ob == 0)
                    c[i] = '#';
                else
                    ob--;
            }
            i++;
        }
        i = n - 1;
        ob = 0;
        while (i >= 0) {
            if (s.charAt(i) == ')')
                ob++;
            if (s.charAt(i) == '(') {
                if (ob == 0)
                    c[i] = '#';
                else
                    ob--;
            }
            i--;
        }
        String ans = "";
        for (i = 0; i < n; i++) {
            if (c[i] != '#')
                ans += c[i];
        }
        return ans;

    }
    ```
20. Min add to make parentheses valid 
    ```
      int minAddToMakeValid(string s) {
        int n = s.length();
        int ob = 0;
        int cb = 0;

        for (int i = 0; i < n; i++) {

            if (s[i] == '(')
                ob++;
            if (s[i] == ')') {
                if (ob == 0)
                    s[i] = '#';
                else
                    ob--;
            }
        }
        ob = 0;
        for (int i = n - 1; i >= 0; i--) {

            if (s[i] == ')')
                ob++;
            if (s[i] == '(') {
                if (ob == 0)
                    s[i] = '#';
                else
                    ob--;
            }
        }
        int ans = 0;
        for (int i = 0; i < n; i++) {
            if (s[i] == '#')
                ans++;
        }
        return ans;
    }
    ```
    ```
       int minAddToMakeValid(string s) {
        int n = s.length();
        int ob = 0;
        int cb = 0;
        int ans=0;

        for (int i = 0; i < n; i++) {

            if (s[i] == '(')
                ob++;
            if (s[i] == ')') {
                if (ob == 0)
                  { s[i] = '#';
                  ans++;
                  }
                else
                    ob--;
            }
        }
        
         return ans+ob;
    }
    ```
21. Check if Parathesis string can be made valid or not
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

22 . Finding the Number of Visible Mountains 

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
23. Implement k stacks single array 
24. Redundant braces 
    ```
      bool checkRedundancy(string& str)
     {
    // create a stack of characters
    stack<char> st;
 
    // Iterate through the given expression
    for (auto& ch : str) {
 
        // if current character is close parenthesis ')'
        if (ch == ')') {
            char top = st.top();
            st.pop();
 
            // If immediate pop have open parenthesis '('
            // duplicate brackets found
            bool flag = true;
 
            while (!st.empty() and top != '(') {
 
                // Check for operators in expression
                if (top == '+' || top == '-' || 
                    top == '*' || top == '/')
                    flag = false;
 
                // Fetch top element of stack
                top = st.top();
                st.pop();
            }
 
            // If operators not found
            if (flag == true)
                return true;
        }
 
        else
            st.push(ch); // push open parenthesis '(',
                  // operators and operands to stack
    }
    return false;
    }
    ```
25. LRU Cache

  ```
LRUCache(int cap)
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

26. Remove duplicate letter 
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
27. Palindrome linked list 
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
28. Decode string 
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
29. Maximum ramp width
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
        //6,0,8,2,1,5 For any element that 8 will be part of left 0 will be better answer that's why
        for (int i = n - 1; i >= 0; i--) {
            while (!st.empty() && A[i] >= A[st.top()]) {
                ans = max(i - st.top(), ans);
                st.pop();
            }
        }

        return ans;
    ```
30. No of atoms
    ```
      typedef unordered_map<string, int> MAP;

    string countOfAtoms(string formula) {
        int n = formula.length();

        stack<MAP> st;
        st.push(MAP());

        int i = 0;

        while (i < n) {
            if (formula[i] == '(') {
                st.push(MAP());
                i++;
            } else if (formula[i] == ')') {
                MAP currMap = st.top();
                st.pop();
                i++;
                string multiplier;
                while (i < formula.length() && isdigit(formula[i])) {
                    multiplier += formula[i];
                    i++;
                }
                if (!multiplier.empty()) {
                    int mult = stoi(multiplier);
                    for (auto& [atom, count] : currMap) {
                        currMap[atom] = count * mult;
                    }
                }

                for (auto& [atom, count] : currMap) {
                    st.top()[atom] += count;
                }
            } else {
                string currAtom;
                currAtom += formula[i];
                i++;
                while (i < formula.length() && islower(formula[i])) {
                    currAtom += formula[i];
                    i++;
                }

                string currCount;
                while (i < formula.length() && isdigit(formula[i])) {
                    currCount += formula[i];
                    i++;
                }

                int count = currCount.empty() ? 1 : stoi(currCount);
                st.top()[currAtom] += count;
            }
        }

        map<string, int> sortedMap(begin(st.top()), end(st.top()));

        string result;
        for (auto& [atom, count] : sortedMap) {
            result += atom;
            if (count > 1) {
                result += to_string(count);
            }
        }

        return result;
    }
    ```
    ```
     \\Approach 2
        int n = formula.length();
        vector<int> mult(n, 1);
        stack<int> factors;
        int multFactor = 1;
        for (int i = n - 1; i >= 0; --i) {
            char ch = formula[i];
            if (ch == ')') {
                string cnt = "";
                int k = i + 1;
                while (k < n && isdigit(formula[k])) {
                    cnt += formula[k++];
                }
                if (cnt == "")
                    cnt = "1";
                int digit = stoi(cnt);
                factors.push(digit);
                multFactor *= digit;
            } else if (ch == '(') {
                int digit = factors.top();
                factors.pop();
                multFactor /= digit;
            } else if (isupper(ch)) {
                mult[i] = multFactor;
            } else
                continue;
        }
        map<string, int> res;
        for (int i = 0; i < n; ++i) {
            char ch = formula[i];
            if (!isupper(ch))
                continue;
            multFactor = mult[i];
            string ele = "";
            ele += ch;
            int j = i + 1;
            while (j < n && islower(formula[j])) {
                ele += formula[j++];
            }
            string cnt = "";
            int k = j;
            while (k < n && isdigit(formula[k])) {
                cnt += formula[k++];
            }
            if (cnt == "")
                cnt = "1";
            int digit = stoi(cnt);
            res[ele] += digit * multFactor;
            i = k - 1;
        }
        string ans = "";
        for (auto& [ele, digit] : res) {
            string cnt = to_string(digit);
            if (cnt == "1")
                cnt = "";
            ans += ele + cnt;
        }
        return ans;
    }
    ```
31. Max score from removing substrings 
    ```
      string removeSubstring(string s, string str, int points, int& ans) {
        stack<char> st;
        int n = s.length();
        for (int i = 0; i < n; i++) {
            if (s[i] == str[1] && !st.empty() && st.top() == str[0]) {
                st.pop();
                ans += points;
            } else {
                st.push(s[i]);
            }
        }
        string ss = "";
        while (!st.empty()) {
            ss += st.top();
            st.pop();
        }
        reverse(ss.begin(), ss.end());
        return ss;
    }

    int maximumGain(string s, int x, int y) {
        int ans = 0;
        if (x >= y) {
            s = removeSubstring(s, "ab", x, ans);
            s = removeSubstring(s, "ba", y, ans);
        } else {
            s = removeSubstring(s, "ba", y, ans);
            s = removeSubstring(s, "ab", x, ans);
        }
        return ans;
    }
    ```
32. Remove outermost parentheses
    ```
      string removeOuterParentheses(string s) {
        string result;
        int balance = 0;

        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '(') {

                if (balance > 0) {
                    result += s[i];
                }
                balance++;
            } else {
                balance--;
                if (balance > 0) {
                    result += s[i];
                }
            }
        }

        return result;
    }
    ```
33. Create max number
    ```
      void merge(vector<int>& ans, vector<int>& v1, vector<int>& v2) {
    int m = v1.size();
    int n = v2.size();
    int i = 0;
    int j = 0;

    while (i < m && j < n) {
        // Case1:
        // ....5 5 5 5 5 7....     .... 5 5 5 5 5 5...
        //     i                        j
        //               tempi                     tempj
        if (v1[i] == v2[j]) {
            int tempi = i;
            int tempj = j;
            while (tempi < m && tempj < n && v1[tempi] == v2[tempj]) {
                tempi++;
                tempj++;
            }
            if (tempj == n) {
                ans.push_back(v1[i]);
                i++;
            } else if (tempi == m) {
                ans.push_back(v2[j]);
                j++;
            } else if (v1[tempi] > v2[tempj]) {
                ans.push_back(v1[i]);
                i++;
            } else {
                ans.push_back(v2[j]);
                j++;
            }
        }
        // Case2 : ....5 6 7 3 4.....     .....2 3 9 8 7....
        //             i                       j
        else if (v1[i] > v2[j]) {
            ans.push_back(v1[i]);
            i++;
        }
        // Case2 : ....2 6 7 3 4.....     .....9 3 9 8 7....
        //             i                       j
        else {
            ans.push_back(v2[j]);
            j++;
        }
    }
    // if ......8 9 7   ...3 5 2 8
    //          i                j
    while (i < m) {
        ans.push_back(v1[i]);
        i++;
    }
    // if ......8 9 7   ...3 5 2 8
    //              i      j
    while (j < n) {
        ans.push_back(v2[j]);
        j++;
    }
     }
    class Solution {
     public:
    vector<int> solve(int k, vector<int>& nums) {
        int n = nums.size();
        if (k > n)
            return {};
        vector<int> ans;
        int rem = n - k;
        ans.push_back(nums[0]);

        for (int i = 1; i < n; i++) {
            while (!ans.empty() && nums[i] > ans.back() && rem > 0) {
                ans.pop_back();
                rem--;
            }
            ans.push_back(nums[i]);
        }
        while (rem--) {
            ans.pop_back();
        }
        return ans;
    }
    vector<int> maxNumber(vector<int>& nums1, vector<int>& nums2, int k) {

        vector<int> ans;
        for (int i = 0; i <= k; i++) {
            vector<int> temp1 = solve(i, nums1);
            vector<int> temp2 = solve(k - i, nums2);
            vector<int> temp;
            merge(temp, temp1, temp2);
            if (temp.size() == k)
                ans = max(ans, temp);
        }
        return ans;
    }
    };
    ```
34. Find permutation
    ```
       // vector<int> ans;
        // for (int i = 0; i < l.length(); i++) {
        //     s.push(i + 1);
        //     if (l[i] == 'I') {
        //         while (!s.empty()) {
        //             ans.push_back(s.top());
        //             s.pop();
        //         }
        //     }
        // }
        // s.push(l.length() + 1);
        // while (!s.empty()) {
        //     ans.push_back(s.top());
        //     s.pop();
        // }
        // return ans;
    ```
    ```
    //Approach 2
     l+= 'I';
        int currPointer = -1;
        int len = l.size();
        vector<int> res;
        
        for(int i=0;i<len;i++){
            if(l[i] == 'I'){
                for(int j=i;j>currPointer;j--){
                    res.push_back(j+1);    
                }
                currPointer = i;
            }
        }
        
        return res;

    ```
35. Asteroid collision
     ```
       vector<int> asteroidCollision(vector<int>& asteroids) {
        stack<int> st;

        for (int& a : asteroids) {

            while (!st.empty() && a < 0 && st.top() > 0) {
                int sum = a + st.top();
                if (sum < 0) {
                    st.pop();
                } else if (sum > 0) {
                    a = 0;
                    break;
                } else {
                    st.pop();
                    a = 0;
                    break;
                }
            }

            if (a != 0)
                st.push(a);
        }

        int s = st.size();

        vector<int> result(s);
        int i = s - 1;
        while (!st.empty()) {
            result[i] = st.top();
            st.pop();
            i--;
        }

        return result;
    }
     ```
36. Max chunks sorted 
    ```
     int maxChunksToSorted(vector<int>& arr) {
        int srt=0,ans=0,n=arr.size();
        for(int i=0;i<n;i++){
            if(srt<arr[i])srt=arr[i];
            if(srt==i)ans++;
        }
        return ans;
        
    }
    We initialize two variables: srt to keep track of the maximum value encountered so far and ans to count the number of valid chunks.
    We iterate through the array using a for loop.
     For each element arr[i], we update srt to be the maximum of srt and arr[i].

    If srt equals the current index i, it means all elements from the start up to i can form a valid chunk. Hence, we increment the chunk count ans.

       Finally, we return ans which represents the maximum number of chunks.
    ```
37. Max chunks sorted II
   ```
     int maxChunksToSorted(vector<int>& arr) {
        stack<int>st;
        for(int i=0;i<arr.size();i++)
        {
            int x=arr[i];
            while(!st.empty()&&st.top()>arr[i])
            {
                x=max(x,st.top());
                st.pop();
            }
            st.push(x);

        }
        return st.size();
        
    }
   ```
38. No of visible people
    ```
       vector<int> canSeePersonsCount(vector<int>& heights) {
        int n = heights.size();
        vector<int> ans(n);
        stack<int> st;
        for (int i = n - 1; i >= 0; --i) {
            while (!st.empty() && heights[i] > st.top()) {
                st.pop();
                ++ans[i];
            }
            if (!st.empty())
                ++ans[i];
            st.push(heights[i]);
        }
        return ans;
    }
    ```
39. Most competitive subsequence
    ```
        vector<int> mostCompetitive(vector<int>& arr, int k) {
        int n = arr.size();
        stack<int> st;
       //Next smaller
       //Prev smaller
   

        for (int i = 0; i < n; i++) {
            int left = n - i;

            while (st.size() > 0 && st.size() + left > k && st.top() > arr[i])
                st.pop();

            if (st.size() < k)
                st.push(arr[i]);
        }

        vector<int> ans;
        while (!st.empty()) {
            ans.push_back(st.top());
            st.pop();
        }
        reverse(ans.begin(), ans.end());

        return ans;
    }
    ```
40. Subarray With Elements Greater Than Varying Threshold
      ```
         int validSubarraySize(vector<int>& a, int threshold) {
        int n = a.size();

        vector<int> stk;
        vector<int> nextS(n, -1), prevS(n, -1);
        for (int i = 0; i < n; i++) {
            while (!stk.empty() && a[i] < a[stk.back()]) {
                nextS[stk.back()] = i;
                stk.pop_back();
            }
            stk.push_back(i);
        }
        stk.clear();
        for (int i = n - 1; i >= 0; i--) {
            while (!stk.empty() && a[i] < a[stk.back()]) {
                prevS[stk.back()] = i;
                stk.pop_back();
            }
            stk.push_back(i);
        }
        for(int i = 0; i < n; i++) {
            int left = prevS[i];                           
            int right = nextS[i] == -1 ? n : nextS[i];     
            
            int len = right - left - 1;                     
            
            if (a[i] > (threshold / ((double) len)))
                return len;
        }
         return -1;
    }
      ```
41. Min deletions to make string balanced
    ```
      int minimumDeletions(string s) {
        int n     = s.length();
        int count = 0;

        stack<char> st;

        for(int i = 0; i < n; i++) {
            if(!st.empty() && s[i] == 'a' && st.top() == 'b') { //'ba'
                st.pop();
                count++;
            } else {
                st.push(s[i]);
            }
        }

        return count;
    }
    ```
42. Clumsy factorial 
    ```
     int clumsy(int n) {

        stack<int> s;
        s.push(n--);
        int idx = 0;

        while (n > 0) {
            if (idx % 4 == 0) {
                s.top() *= n;
            } else if (idx % 4 == 1) {
                s.top() /= n;
            } else if (idx % 4 == 2) {
                s.push(n);
            } else {
                s.push(-n);
            }
            idx++;
            n--;
        }

        int result = 0;
        while (!s.empty()) {
            result += s.top();
            s.pop();
        }
        return result;
    }
    ```
    ```
      int clumsy(int N) {
    if (N == 1)
      return 1;
    if (N == 2)
      return 2;
    if (N == 3)
      return 6;
    if (N == 4)
      return 7;
    if (N % 4 == 1)
      return N + 2;
    if (N % 4 == 2)
      return N + 2;
    if (N % 4 == 3)
      return N - 1;
    return N + 1;
      }
    ```
43. Minimum deletions to make array beautiful 
   ```
       int minDeletion(vector<int>& nums) {
       stack<int> st;
        int del=0;
        for(int i=0;i<nums.size();i++){
            if(st.empty()){
                st.push(nums[i]);
            }
            else if (st.top()==nums[i] && (st.size()-1)%2==0){
                del++;
                st.pop();
                st.push(nums[i]);
            }
            else st.push(nums[i]);
        }
        if((nums.size()-del) %2 ) return del+1;
        return del;
    }
   ```
44. Max stack 
```
//TLE
   void push(int x) {
        s1.push(x);
        if (s2.empty() || s2.top() <= x)
            s2.push(x);
    }

    int pop() {
        int x = s1.top();
        s1.pop();
        if (x == peekMax())
            s2.pop();
        return x;
    }

    int top() { return s1.top(); }

    int peekMax() { return s2.top(); }

    int popMax() {
        int x = peekMax();
        stack<int> tmp;
        while (top() != x)
            tmp.push(pop());
        pop();
        while (!tmp.empty()) {
            push(tmp.top());
            tmp.pop();
        }

        return x;
    }
```
45. 132 Pattern 
    ```
       bool find132pattern(vector<int>& nums) {
        stack<int> s;
        int third = INT_MIN;//1
         for (int i = nums.size() - 1; i >= 0; i--) {
            if (nums[i] < third) return true;
            while (!s.empty() && s.top() < nums[i]) {
                third = s.top();//2
                s.pop();
            }
            s.push(nums[i]);//3
        }
        return false;

        
    }
    ```

## Extras 

Prefix , postfix & infix conversion
