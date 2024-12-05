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
