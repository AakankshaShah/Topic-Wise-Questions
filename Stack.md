# Stack



## Theory

https://www.youtube.com/playlist?list=PL_z_8CaSLPWdeOezg68SKkeLN4-T_jNHd


## Questions

1. Nearest Greatest Element towards left/right
2. Nearest Smallest  Element towards left/right
3. Stock span
4. Max area under histogram
5. Max area of rectangle in a binary matrix
6. Rainwater trapping
7. Min Stack
8. Celebrity Problem
9. Valid Parentheses https://leetcode.com/problems/valid-parentheses
10. Longest Valid Paratheses* https://leetcode.com/problems/longest-valid-parentheses
11. Simplify Path* https://leetcode.com/problems/simplify-path/
12. Polish Notation https://leetcode.com/problems/evaluate-reverse-polish-notation
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

## Extras 

Prefix , postfix & infix conversion
