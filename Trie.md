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
6. Add bold tag
   ```


    string addBoldTag(string s, const vector<string>& dict) {
    int n = s.size();
    vector<pair<int,int>> intervals;

    // Step 1: find all matches and store intervals
    for (const string& w : dict) {
        size_t pos = s.find(w);
        while (pos != string::npos) {
            intervals.push_back({(int)pos, (int)(pos + w.size() - 1)});
            pos = s.find(w, pos + 1);
        }
    }

    if (intervals.empty()) return s;

    // Step 2: sort intervals
    sort(intervals.begin(), intervals.end());

    // Step 3: merge overlapping/adjacent intervals
    vector<pair<int,int>> merged;
    int start = intervals[0].first, end = intervals[0].second;

    for (int i = 1; i < intervals.size(); i++) {
        if (intervals[i].first <= end + 1) {
            end = max(end, intervals[i].second);
        } else {
            merged.push_back({start, end});
            start = intervals[i].first;
            end = intervals[i].second;
        }
    }
    merged.push_back({start, end});

    // Step 4: build output string
    string result;
    int idx = 0;
    for (auto &p : merged) {
        int l = p.first, r = p.second;
        // add part before bold
        result.append(s.substr(idx, l - idx));
        // add bold part
        result += "<b>" + s.substr(l, r - l + 1) + "</b>";
        idx = r + 1;
    }
    // add remaining part
    if (idx < n) result.append(s.substr(idx));

    return result;
    }


   ```
   ```
     struct TrieNode {
    bool isWord = false;
    unordered_map<char, TrieNode*> children;
     };

     class Trie {
    public:
    TrieNode* root;
    Trie() { root = new TrieNode(); }

    void insert(const string &word) {
        TrieNode* node = root;
        for (char c : word) {
            if (!node->children.count(c))
                node->children[c] = new TrieNode();
            node = node->children[c];
        }
        node->isWord = true;
    }

    // Return farthest match end index starting at position i, or -1 if no match
    int search(const string &s, int i) {
        TrieNode* node = root;
        int far = -1;
        for (int j = i; j < s.size(); j++) {
            if (!node->children.count(s[j])) break;
            node = node->children[s[j]];
            if (node->isWord) far = j;
        }
        return far;
    }
    };

    string addBoldTag(string s, const vector<string>& dict) {
    int n = s.size();
    vector<bool> bold(n, false);

    // Build Trie
    Trie trie;
    for (auto &w : dict) trie.insert(w);

    // Mark bold positions
    for (int i = 0; i < n; i++) {
        int far = trie.search(s, i);
        if (far >= 0) {
            for (int k = i; k <= far; k++) bold[k] = true;
        }
    }

    // Build result with <b> tags
    string result;
    for (int i = 0; i < n; i++) {
        if (bold[i] && (i == 0 || !bold[i-1])) result += "<b>";
        result.push_back(s[i]);
        if (bold[i] && (i == n-1 || !bold[i+1])) result += "</b>";
    }
    return result;
     }
   ```
7. Autocomplete
```
     
struct TrieNode {
    unordered_map<char, TrieNode*> children;
    unordered_map<string, int> counts; // sentence -> frequency
    bool isEnd = false;
};

class AutocompleteSystem {
private:
    TrieNode* root;
    string currentInput;

    // Insert a sentence into the trie
    void insert(const string& sentence, int count) {
        TrieNode* node = root;
        for (char c : sentence) {
            if (!node->children.count(c))
                node->children[c] = new TrieNode();
            node = node->children[c];
            node->counts[sentence] += count; // update frequency
        }
        node->isEnd = true;
    }

    // Get top 3 sentences from a node using min-heap
    vector<string> getTop3(TrieNode* node) {
        auto cmp = [](auto &a, auto &b) {
            return a.first > b.first || (a.first == b.first && a.second < b.second);
        };

        priority_queue<pair<int, string>, vector<pair<int, string>>, decltype(cmp)> pq(cmp);

        for (const auto& entry : node->counts) {
            pq.push({entry.second, entry.first});
            if (pq.size() > 3) pq.pop();
        }

        vector<string> result;
        while (!pq.empty()) {
            result.push_back(pq.top().second);
            pq.pop();
        }

        reverse(result.begin(), result.end());
        return result;
    }

public:
    AutocompleteSystem(vector<string>& sentences, vector<int>& times) {
        root = new TrieNode();
        currentInput = "";
        for (int i = 0; i < sentences.size(); i++) {
            insert(sentences[i], times[i]);
        }
    }

    vector<string> input(char c) {
        if (c == '#') {
            insert(currentInput, 1);
            currentInput = "";
            return {};
        }

        currentInput += c;
        TrieNode* node = root;
        for (char ch : currentInput) {
            if (!node->children.count(ch))
                return {}; // no match
            node = node->children[ch];
        }

        return getTop3(node);
    }
};
```
