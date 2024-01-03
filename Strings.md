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
