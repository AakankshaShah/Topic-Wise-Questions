1. Reorder Strings https://leetcode.com/problems/reorganize-string/description/

    ```
     typedef pair<int, char> P;
    string reorganizeString(string s) {
        vector<int> count(26, 0);
        int n = s.length();

        for (int i = 0; i < s.length(); i++) {
            count[s[i] - 'a']++;

            if (count[s[i] - 'a'] > (n + 1) / 2)
                return "";
        }

        priority_queue<P, vector<P>> pq;

        for (char i = 'a'; i <= 'z'; i++) {
            if (count[i - 'a'] > 0)
                pq.push({count[i - 'a'], i});
        }
        string result = "";

        while (pq.size() >= 2) {

            auto P1 = pq.top();
            pq.pop();

            auto P2 = pq.top();
            pq.pop();

            result.push_back(P1.second);
            result.push_back(P2.second);

            P1.first--;

            if (P1.first > 0)
                pq.push(P1);

            P2.first--;

            if (P2.first > 0)
                pq.push(P2);
        }

        if (!pq.empty()) {
            result.push_back(pq.top().second);
        }

        return result;

    ```
2. Unique mail address
    ```
      unordered_set<string> st;
        for (string& email : emails) {
            string cleanEmail;
            for (char c : email) {
                if (c == '+' || c == '@')
                    break;
                if (c == '.')
                    continue;
                cleanEmail += c;
            }
            cleanEmail += email.substr(email.find('@'));
            st.insert(cleanEmail);
        }
        return st.size();
    ```
3. Valid word abbreviation
    ```
      bool validWordAbbreviation(string word, string abbr) {
        int l1 = 0;
        int l2 = 0;

        while (l2 < abbr.length() && l1 < word.length()) {
            int num = 0;
            bool start = 0;
            while (isdigit(abbr[l2])) {
                if (!start && abbr[l2] == '0')
                    return false;
                num = num * 10 + abbr[l2] - '0';
                l2++;
                start = 1;
            }

            if (num == 0) {
                if (word[l1] != abbr[l2])
                    return false;
                l1++;
                l2++;
            } else {
                l1 += num;
            }
        }

        return l1 == word.length() && l2 == abbr.length();
    }
    ```
    ```
      public boolean validWordAbbreviation(String word, String abbr) {
        int l1 = 0;
        int l2 = 0;

        while (l2 < abbr.length() && l1 < word.length()) {
            int num = 0;
            boolean start = false;
            while (l2 < abbr.length() && Character.isDigit(abbr.charAt(l2))) {
                if (!start && abbr.charAt(l2) == '0')
                    return false;
                num = num * 10 + abbr.charAt(l2) - '0';
                l2++;
                start = true;
            }

            if (num == 0) {
                if (word.charAt(l1) != abbr.charAt(l2))
                    return false;
                l1++;
                l2++;
            } else {
                l1 += num;
            }
        }

        return l1 == word.length() && l2 == abbr.length();

    }
    ```
4. Valid Palindrome 2
    ```
       bool isPalindrome(string s,int l,int h){
        while(l<h){
            if(s[l]!=s[h])  return false;
            l++;h--;
        }
        return true;
    }
    bool validPalindrome(string s) {
        int l=0,h=s.size()-1;
        while(l<h){
            if(s[l]==s[h])  l++,h--;
            else{
                return isPalindrome(s,l+1,h) || isPalindrome(s,l,h-1);
            }
        }
        return true;
    }
    ```
    ```
       boolean isPalindrome(String s, int l, int h) {
        while (l < h) {
            if (s.charAt(l) != s.charAt(h))
                return false;
            l++;
            h--;
        }
        return true;
    }

    public boolean validPalindrome(String s) {
        int l = 0, h = s.length() - 1;
        while (l < h) {
            if (s.charAt(l) == s.charAt(h)) {
                l++;
                h--;
            } else {
                return isPalindrome(s, l + 1, h) || isPalindrome(s, l, h - 1);
            }
        }
        return true;

    }
    ```
5. Custom order sorting 
    ```
       string customSortString(string order, string s) {
        unordered_map<char, int> freq;
        for (char letter : s) {
            freq[letter]++;
        }
        string result = "";
        for (char letter : order) {
            while (freq[letter]-- > 0) {
                result += letter;
            }
        }
        for (auto& [letter, count] : freq) {
            while (count-- > 0) {
                result += letter;
            }
        }
        return result;
    }
    ```
6. Add Strings
     ```
        public String addStrings(String num1, String num2) {
        int a = num1.length() - 1;
        int b = num2.length() - 1;
        int c = 0;
        StringBuilder s = new StringBuilder();
        while (a >= 0 || b >= 0 || c != 0) {
            int sum = c;
            if (a >= 0) {
                sum += num1.charAt(a) - '0';
                a--;

            }
            if (b >= 0) {
                sum += num2.charAt(b) - '0';
                b--;

            }

            c = sum / 10;
            s.append(sum % 10);

        }
        return s.reverse().toString();

    }
     ```
7. Merge String alternatively
    ```
        public String mergeAlternately(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();
        StringBuilder result = new StringBuilder();

        for (int i = 0; i < Math.max(m, n); i++) {
            if (i < m) {
                result.append(word1.charAt(i));
            }
            if (i < n) {
                result.append(word2.charAt(i));
            }
        }

        return result.toString();

    }
    ```
8. String compression
     ```
        int compress(vector<char>& chars) {
        int write = 0;
        int read = 0;

        while (read < chars.size()) {
            char currentChar = chars[read];
            int count = 0;

            while (read < chars.size() && chars[read] == currentChar) {
                read++;
                count++;
            }

            chars[write++] = currentChar;

            if (count > 1) {
                for (char c : to_string(count)) {
                    chars[write++] = c;
                }
            }
        }

        return write;
    }
     ```
9. Add bold tag in String 
     ```
         string addBoldTag(string str, vector<string>& words) {
        int n = str.size();
        vector<vector<int>> intervals;
      

        for (auto& word : words) {
            size_t pos = str.find(word);
            while (pos != string::npos) {
                int start = pos, end = pos + word.size();
                intervals.pb({start, end});
                pos = str.find(word, pos + 1);
            }
        }
        if (intervals.empty())
            return str;
        sort(intervals.begin(), intervals.end(),
             [&](const vector<int>& a, const vector<int>& b) {
                 return a[0] < b[0];
             });
        int start = intervals[0][0], end = intervals[0][1];
        map<int, int> tagInsertionStore;
        for (int i = 1; i < intervals.size(); i++) {
            if (start <= intervals[i][0] && intervals[i][0] <= end) {
                end = max(end, intervals[i][1]);
            } else {

                tagInsertionStore[start] = 0;
                tagInsertionStore[end] = 1;
                start = intervals[i][0], end = intervals[i][1];
            }
        }
        tagInsertionStore[start] = 0; // 0 indicates opening tag
        tagInsertionStore[end] = 1;
        string res = "";
        string opening = "<b>";
        string closing = "</b>";
        for (int i = 0; i < n; i++) {
            if (tagInsertionStore.count(i)) {
                if (!tagInsertionStore[i])
                    res += opening;
                else
                    res += closing;
                res.pb(str[i]);
            } else
                res.pb(str[i]);
        }

        if (tagInsertionStore.count(n))
            res += closing; 

        return res;
    }
     ```
     
   
  
