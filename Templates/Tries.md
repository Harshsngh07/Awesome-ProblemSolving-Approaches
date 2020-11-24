If you have never heard of Trie, then here is an article and a bunch of references to help you.
Let's jump into the topic.

##### What is Trie?

Trie is a tree-like data structure wherein all the nodes stores alphabetical values (most commonly).

Trie is an ordered tree data structure in which every traversal down the branch retrieves you a string or word.

Here is a formal definition from Wikipedia,
A trie also called a digital tree or prefix tree, is a kind of search tree—an ordered tree data structure used to store a dynamic set or associative array where the keys are usually strings.
Fun Fact: Trie name comes from name retrieval.

Representation:

Template 1 ( Representation )
```
class TrieNode{
public:
    bool end;
    TrieNode* children[27];
    TrieNode(){
        end = false;
        memset(children, NULL, sizeof(children));
    }
};
```

It contains an empty root node and references to all other nodes.

The number of references depends on the total number of values possible.
Example: If the trie represents strings that contain only lowercase letters. Then the number of references needs to be 26 since there are 26 different alphabets from 'a' to 'z'.
If trie represents bits it has only two references. (0 and 1)

To be more clear, What does a node contains?
It contains two parts.

The array of references to other nodes.
Boolean value - leaf (To check whether the string ends at that node or not)
Note here leaf is not to check whether it is leaf node or not.
Let's take an example and build a trie.

List = { "apple", "app", "abide", "ball", "bat" }
Note: All the string in the list contains lowercase letters from 'a' to 'z'.
image

Status: apple is added to the trie.

Here the first node at the top is root and it is empty. 
Notes: 
Now root has a ref. to node 'a' and node 'a' has reference to 'p' and so on. All other  reference are still NULL.
image


Status: app added to trie which already contains apple.
From the root, we have to check whether the reference node exists or not.
If exists, we have to move to the node or create a new node.

Fun Fact: All the descendants of a node have a common prefix of the string associated with that node
Correction in the image: 'e' node in apple also have leaf value true.

image

Status: abide is added. It shares prefix 'a' with apple and app.
image

Status: ball is added.
Since 'b' reference is not present in root. we create it.
image

Status: bat added.
Final image of the trie after inserting strings in the list.
We have to  traverse the trie to search a string.
I hope it made some sense.
It really takes a day to produce the images. A upvote would be encouraging. Thanks.

Template for build trie and search a string in a trie:

	TrieNode* root = new TrieNode();        /* Root Creation */
	
    void insert(string word) {              /* Insert word into the trie */
        TrieNode* node = root;
        for(char c : word){
            if(!node -> children[c - 'a']){
                node -> children[c - 'a'] = new TrieNode();
            }
            node = node -> children[c - 'a'];
        }
        node -> end = true;
    }
    
    
    bool search(string word) {             /* Search word into the trie */
        TrieNode* node = root;
        for(char c : word){
            if(!node -> children[c - 'a']){
                return false;
            }
            node = node -> children[c - 'a'];
        }
        return node -> end;
    }
    

 <hr/>
 
 Problem 1 Implement Magic Dictionary
Solved using basic templates.
```
class Trie{
public:
    Trie* children[26];
    bool end;
    Trie(){
        memset(children, NULL, sizeof(children));
        end = false;
    }
};
class MagicDictionary {
private:
    Trie* root = new Trie();
public:
    /** Initialize your data structure here. */
    MagicDictionary() {
        
    }
    void insert(string s){
        Trie* node = root;
        for(char c : s){
            if(!node -> children[c - 'a']){
                node -> children[c - 'a'] = new Trie();
            }
            node  = node -> children[c - 'a'];
        }
        node -> end = true;
    }
    /** Build a dictionary through a list of words */
    void buildDict(vector<string> dict) {
        for(int i = 0; i < dict.size(); i++){
            insert(dict[i]);
        }
    }
    bool found(string s){
        Trie* node = root;
        for(char c : s){
            if(!node -> children[c - 'a']){
                return false;
            }
            node = node -> children[c - 'a'];
        }
        return node -> end;
    }
    /** Returns if there is any word in the trie that equals to the given word after modifying exactly one character */
    bool search(string word) {
        for(int i = 0; i < word.length(); i++){
            char c = word[i];
            for(char ch = 'a'; ch <= 'z'; ch++){
                if(ch == c) continue;
                word[i] = ch;
                if(found(word)){
                    return true;
                }
            }
            word[i] = c;
        }
        return false;
    }
};
```

Problem 2: Maximum xor of two numbers in an array
Bitwise Trie. Only two reference.
```
class Trie{
public: 
    Trie* children[2] = {};
    bool leaf;
    Trie(){
        leaf = false;
    }
};
class Solution {
private:
    Trie* root = new Trie();
public:
    void buildTree(vector<int> & nums){
        for(int num : nums){
            Trie* node = root;
            for(int i = 31; i >= 0; i--){
                int bitOp = (num >> i) & 1;
                if(!node -> children[bitOp]){
                    node -> children[bitOp] = new Trie();
                }
                node = node -> children[bitOp];
            }
            node -> leaf = true;
        }
    }
    
    int findMaximumXOR(vector<int>& nums) {
        buildTree(nums);
        int maxRes = 0, res = 0;
        for(int i = 0; i < nums.size(); i++){
            int num = nums[i];
            Trie* node = root;
            int index;
            int res = 0;
            for(int k = 31; k >= 0; k--){
                int bitOp = (num >> k) & 1;
                if(bitOp) index = 0;
                else index = 1;
                if(node -> children[index]){
                    res <<= 1;
                    res |= 1;
                    node = node -> children[index];
                }
                else{
                    res <<= 1;
                    res |= 0;
                    node = node -> children[index ? 0 : 1];
                }
            }
            maxRes = max(maxRes, res);
        }
        return maxRes;
    }
};
```
Problem 3: Word Search 2
DFS + Trie
```
class Trie{
public:
    Trie* children[26];
    bool leaf;
    Trie(){
        memset(children, NULL, sizeof(children));
        leaf = false;
    }
};
class Solution {
private:
    Trie* root = new Trie();
    int row = 0, col = 0;
public:
    void buildTrie(vector<string> & words){
        for(string word : words){
            Trie* node = root;
            for(char c : word){
                if(!node -> children[c - 'a'])
                    node -> children[c - 'a'] = new Trie();
                node = node -> children[c - 'a'];
            }
            node -> leaf = true;
        }
    }
    void dfs(vector<vector<char>> & board, Trie* node, string build, vector<string> & res, int i, int j)
    {
        if(i < 0 || j < 0 || i >= row || j >= col)
            return;
        if(board[i][j] == 'F'){
            return;
        }
        if(!node -> children[board[i][j] - 'a']){
            return;
        }
                
                build = build + board[i][j];
                node = node -> children[board[i][j] - 'a'];
                if(node -> leaf){
                    node -> leaf = false;
                    res.push_back(build);
                }
                char temp = board[i][j];
                board[i][j] = 'F';
                dfs(board, node, build, res, i + 1, j);
                dfs(board, node, build, res, i - 1, j);
                dfs(board, node, build, res, i, j + 1);
                dfs(board, node, build, res, i, j - 1);
                board[i][j] = temp; 

                

    }
        
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        buildTrie(words);
        vector<string> res;
        row = board.size(), col = board[0].size();
        for(int i = 0; i < row; i++){
            for(int j = 0; j < col; j++)
            {
                dfs(board, root, "", res, i, j);
            }
        }
        
        return res;
    }
};
```
##### Advantages of using Trie:

It is similar to Hashmap but there is some tradeoff between them in terms of time and space.

We can search a string in O(m) in Trie (m - is the length of the string to be searched) but it needs O(m) time for evaluating the hash function and constant time for retrieving.

There is no key collisions. A key collision is the hash function mapping of different keys to the same position in a hash table.
There is no need for hash functions.
It is used in auto-complete functions in search engines.

##### Time Complexity Analysis:

It takes O(mn) time in the worst case to create trie where m is the longest string and n is the total number of strings. Time depends on the number of strings that the trie contains and how long are they.

It takes O(m) time to perform insert, search and delete operation where m is the length of the word.

Space Complexity Analysis:

O(kN) where k is the number of references (including null) and N is the number of nodes in trie.   