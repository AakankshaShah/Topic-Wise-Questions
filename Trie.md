1. Number of distinct substrings in a string 
```
class Node {
public:
    Node* links[26] = {nullptr};

    bool containsKey(char ch) { return links[ch - 'a'] != nullptr; }

    void put(char ch, Node* n) { links[ch - 'a'] = n; }

    Node* get(char ch) { return links[ch - 'a']; }
};

class Solution {
public:
    int countDistinct(string s) {
        int cnt = 0;
        Node* root = new Node(); // Trie root node
        int n = s.length();

        // Traverse every possible starting index of the substrings
        for (int i = 0; i < n; i++) {
            Node* currentNode = root;
            for (int j = i; j < n; j++) {
                char ch = s[j];

                if (!currentNode->containsKey(ch)) {
                    cnt++;
                    currentNode->put(ch, new Node());
                }

                currentNode = currentNode->get(ch);
            }
        }

        return cnt;
    }
};

```
