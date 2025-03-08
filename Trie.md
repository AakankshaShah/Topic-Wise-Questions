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
```
class TrieNode {
    TrieNode[] links = new TrieNode[26];

    boolean containsKey(char ch) {
        return links[ch - 'a'] != null;
    }

    void put(char ch, TrieNode node) {
        links[ch - 'a'] = node;
    }

    TrieNode get(char ch) {
        return links[ch - 'a'];
    }
}

class Solution {
    public int countDistinct(String s) {
        int cnt = 0;
        TrieNode root = new TrieNode();

        for (int i = 0; i < s.length(); i++) {
            TrieNode node = root;

            for (int j = i; j < s.length(); j++) {

                if (!node.containsKey(s.charAt(j))) {
                    cnt++;
                    node.put(s.charAt(j), new TrieNode());
                }

                node = node.get(s.charAt(j));
            }
        }

        return cnt;
    }
}

```
2. Word Search 2
     ```
         vector<string> result;
    int r, c;
    vector<pair<int, int>> directions{{-1, 0}, {1, 0}, {0, 1}, {0, -1}};
    struct trieNode {
        bool endOfWord;
        trieNode* children[26];
        string word;
    };
    trieNode* getNode() {
        trieNode* temp = new trieNode();
        temp->endOfWord = false;
        for (int i = 0; i < 26; i++) {
            temp->children[i] = NULL;
        }
        temp->word = "";
        return temp;
    }

    void insert(trieNode* root, string str) {
        trieNode* pCrawl = root;
        for (char ch : str) {
            if (pCrawl->children[ch - 'a'] == NULL) {
                pCrawl->children[ch - 'a'] = getNode();
            }
            pCrawl = pCrawl->children[ch - 'a'];
        }
        pCrawl->endOfWord = true;
        pCrawl->word = str;
    }
    void DFS(vector<vector<char>>& board, int i, int j, trieNode* root) {
        if (i < 0 || i >= r || j < 0 || j >= c || board[i][j] == '$' ||
            root->children[board[i][j] - 'a'] == NULL) {
            return;
        }
        root = root->children[board[i][j] - 'a'];
        if (root->endOfWord == true) {
            result.push_back(root->word);
            root->endOfWord = false;
        }
        char temp = board[i][j];

        board[i][j] = '$';
        for (pair<int, int> p : directions) {
            int new_i = i + p.first;
            int new_j = j + p.second;
            DFS(board, new_i, new_j, root);
        }
        board[i][j] = temp;
    }

    vector<string> findWords(vector<vector<char>>& board,
                             vector<string>& words) {
        r = board.size();
        c = board[0].size();

        trieNode* root = getNode();
        for (string str : words) {
            insert(root, str);
        }

        for (int i = 0; i < r; i++) {
            for (int j = 0; j < c; j++) {
                char ch = board[i][j];
                if (root->children[ch - 'a'] != NULL) {
                    DFS(board, i, j, root);
                }
            }
        }
        return result;
    }
     ```
3.  Design In-Memory File System
     ```
           struct Node {
    unordered_map<string, Node*> dir = {};
    vector<string> pathList = {};
    string content = "";
    };
    class FileSystem {
     private:
     Node* main;

    public:
    FileSystem() { main = new Node; }

    vector<string> ls(string path) {
        Node *temp = file(path);
        if (temp->content.size())
            return {path.substr(path.find_last_of('/') + 1)};
        sort(temp->pathList.begin(), temp->pathList.end());
        return temp->pathList;
    }

    void mkdir(string path) {
        file(path);
    }

    void addContentToFile(string filePath, string content) {
        Node *temp = file(filePath);
        temp->content += content; 
    }

    string readContentFromFile(string filePath) {
         Node *temp = file(filePath);
        return temp->content;
    }
    Node* file(string path) {
        Node* temp = main;
        stringstream ss(path);
        string currPath = "";

        while (getline(ss, currPath, '/')) {

            if (temp->dir[currPath] == nullptr) {
                temp->pathList.push_back(currPath);
                temp->dir[currPath] = new Node;
            }
            temp = temp->dir[currPath];
        }
        return temp;
    }
    };
     ```
4.Find the Length of the Longest Common Prefix
   ```
          struct TrieNode {

        TrieNode* children[10];
    };
    TrieNode* getTrieNode() {
        TrieNode* node = new TrieNode();
        for (int i = 0; i < 10; i++) {
            node->children[i] = nullptr;
        }

        return node;
    }
    void insert(int num, TrieNode* root) {
        TrieNode* crawl = root;
        string numStr = to_string(num);

        for (char ch : numStr) {
            int idx = ch - '0';
            if (!crawl->children[idx]) {
                crawl->children[idx] = getTrieNode();
            }

            crawl = crawl->children[idx];
        }
    }
    int search(int num, TrieNode* root) {
        TrieNode* crawl = root;
        string numStr = to_string(num);
        int length = 0;

        for (char ch : numStr) {
            int idx = ch - '0';
            if (crawl->children[idx]) {
                length++;
                crawl = crawl->children[idx];
            } else {
                break;
            }
        }

        return length;
    }
    int longestCommonPrefix(vector<int>& arr1, vector<int>& arr2) {
        TrieNode* root = getTrieNode();
        for (int num : arr1) {
            insert(num, root);
        }
        int result = 0;
        for (int num : arr2) {
            result = max(result, search(num, root));
        }

        return result;
    }
   ```
5. Map sum pairs
     ```
          private:
    struct Trie {
        int sum = 0;
        unordered_map<char, Trie*> children;
    };

    Trie* root;
    unordered_map<string, int> keymap;

    public:
    MapSum() { root = new Trie(); }

    void insert(string key, int val) {
        int delta = val - keymap[key];
        keymap[key] = val;
        Trie* curr = root;
        for (char c : key) {
            if (!curr->children.count(c)) {
                curr->children[c] = new Trie();
            }
            curr = curr->children[c];
            curr->sum += delta;
        }
    }

    int sum(string prefix) {
        Trie* curr = root;
        for (char ch : prefix) {
            if (!curr->children.count(ch))
                return 0; // Prefix not found
            curr = curr->children[ch];
        }
        return curr->sum;
    } 
     ```
