# 数据结构完全学习指南（C++ 实现精简版）

> 一份以 C++ 代码为核心、解释精炼的数据结构文档。适合快速上手和复习，所有实现均基于 C++11/14。

---

## 目录

- [第一部分：入门篇](#第一部分入门篇)
  - [1. 数组](#1-数组)
  - [2. 链表](#2-链表)
  - [3. 栈](#3-栈)
  - [4. 队列](#4-队列)
  - [5. 哈希表](#5-哈希表)
- [第二部分：进阶篇](#第二部分进阶篇)
  - [6. 二叉树与BST](#6-二叉树与bst)
  - [7. AVL树](#7-avl树)
  - [8. 红黑树（概念）](#8-红黑树概念)
  - [9. B树/B+树（概念）](#9-b树b树概念)
  - [10. 堆](#10-堆)
  - [11. 图](#11-图)
  - [12. 并查集](#12-并查集)
  - [13. 字典树（Trie）](#13-字典树trie)
  - [14. 线段树](#14-线段树)
  - [15. 树状数组](#15-树状数组)
  - [16. 跳表](#16-跳表)
- [第三部分：总结与学习建议](#第三部分总结与学习建议)

---

## 第一部分：入门篇

### 1. 数组

动态数组（`std::vector`）的底层原理：连续内存，支持随机访问，尾部插入均摊 O(1)。

**手动实现简单动态数组**（仅演示核心）：

```cpp
#include <iostream>
#include <cstring>
class DynamicArray {
private:
    int* data;
    int capacity;
    int size;
    void resize() {
        capacity = capacity * 2 + 1;
        int* newData = new int[capacity];
        memcpy(newData, data, size * sizeof(int));
        delete[] data;
        data = newData;
    }
public:
    DynamicArray() : data(nullptr), capacity(0), size(0) {}
    ~DynamicArray() { delete[] data; }
    void push_back(int val) {
        if (size == capacity) resize();
        data[size++] = val;
    }
    int& operator[](int index) { return data[index]; }
    int getSize() const { return size; }
};
```

**常用操作**：随机访问 O(1)，插入/删除中间 O(n)。

---

### 2. 链表

**单向链表节点**：

```cpp
struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};
```

**基本操作（迭代）**：

```cpp
// 反转链表
ListNode* reverse(ListNode* head) {
    ListNode* prev = nullptr;
    ListNode* cur = head;
    while (cur) {
        ListNode* nxt = cur->next;
        cur->next = prev;
        prev = cur;
        cur = nxt;
    }
    return prev;
}

// 在头部插入
ListNode* insertHead(ListNode* head, int val) {
    ListNode* node = new ListNode(val);
    node->next = head;
    return node;
}

// 删除指定值（第一次出现）
ListNode* deleteNode(ListNode* head, int val) {
    if (!head) return nullptr;
    if (head->val == val) { ListNode* tmp = head->next; delete head; return tmp; }
    ListNode* cur = head;
    while (cur->next && cur->next->val != val) cur = cur->next;
    if (cur->next) { ListNode* del = cur->next; cur->next = del->next; delete del; }
    return head;
}
```

**双向链表**（`std::list`）可双向遍历，插入删除更灵活。

---

### 3. 栈

后进先出（LIFO）。可用数组或链表实现。

**顺序栈（基于动态数组）**：

```cpp
#include <vector>
template<typename T>
class Stack {
    std::vector<T> data;
public:
    void push(const T& val) { data.push_back(val); }
    void pop() { if (!empty()) data.pop_back(); }
    T& top() { return data.back(); }
    bool empty() const { return data.empty(); }
};
```

**链表栈**（以头为栈顶）：

```cpp
template<typename T>
class LinkedStack {
    struct Node { T val; Node* next; Node(T v, Node* n=nullptr):val(v),next(n){} };
    Node* head = nullptr;
public:
    ~LinkedStack() { while(head){ Node* tmp=head; head=head->next; delete tmp; } }
    void push(T v) { head = new Node(v, head); }
    void pop() { if(head){ Node* tmp=head; head=head->next; delete tmp; } }
    T& top() { return head->val; }
    bool empty() const { return head==nullptr; }
};
```

**应用**：括号匹配、表达式求值、DFS。

---

### 4. 队列

先进先出（FIFO）。可用循环数组或链表。

**循环队列（数组）**：

```cpp
template<typename T>
class CircularQueue {
    T* data;
    int capacity, front, rear, count; // rear指向下一个插入位置
public:
    CircularQueue(int cap) : capacity(cap), front(0), rear(0), count(0) {
        data = new T[capacity];
    }
    ~CircularQueue() { delete[] data; }
    bool enqueue(T val) {
        if (count == capacity) return false;
        data[rear] = val;
        rear = (rear + 1) % capacity;
        count++;
        return true;
    }
    bool dequeue(T& val) {
        if (count == 0) return false;
        val = data[front];
        front = (front + 1) % capacity;
        count--;
        return true;
    }
    bool empty() const { return count == 0; }
};
```

**链式队列**（带头尾指针）入队 O(1)，出队 O(1)。

---

### 5. 哈希表

使用链地址法实现（简化版，仅 int 键）：

```cpp
#include <list>
#include <vector>
class HashMap {
    std::vector<std::list<std::pair<int,int>>> buckets;
    int size;
    int hash(int key) const { return key % buckets.size(); }
public:
    HashMap(int n = 101) : buckets(n), size(0) {}
    void insert(int key, int val) {
        int idx = hash(key);
        for (auto& p : buckets[idx]) {
            if (p.first == key) { p.second = val; return; }
        }
        buckets[idx].push_back({key, val});
        size++;
        // 简单负载因子检查（未实现扩容）
    }
    int* get(int key) {
        int idx = hash(key);
        for (auto& p : buckets[idx]) if (p.first == key) return &p.second;
        return nullptr;
    }
    bool remove(int key) {
        int idx = hash(key);
        auto& bucket = buckets[idx];
        for (auto it = bucket.begin(); it != bucket.end(); ++it) {
            if (it->first == key) { bucket.erase(it); size--; return true; }
        }
        return false;
    }
};
```

**冲突解决**：链地址法（拉链）。开放地址法（线性探测）可自行实现。

---

## 第二部分：进阶篇

### 6. 二叉树与BST

**二叉树节点**：

```cpp
struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```

**二叉搜索树（BST）插入与查找**：

```cpp
TreeNode* insertBST(TreeNode* root, int val) {
    if (!root) return new TreeNode(val);
    if (val < root->val) root->left = insertBST(root->left, val);
    else if (val > root->val) root->right = insertBST(root->right, val);
    return root;
}

TreeNode* searchBST(TreeNode* root, int val) {
    if (!root || root->val == val) return root;
    if (val < root->val) return searchBST(root->left, val);
    return searchBST(root->right, val);
}

// 删除（返回新根）
TreeNode* deleteBST(TreeNode* root, int val) {
    if (!root) return nullptr;
    if (val < root->val) root->left = deleteBST(root->left, val);
    else if (val > root->val) root->right = deleteBST(root->right, val);
    else {
        if (!root->left) { TreeNode* tmp = root->right; delete root; return tmp; }
        if (!root->right) { TreeNode* tmp = root->left; delete root; return tmp; }
        TreeNode* minNode = root->right;
        while (minNode->left) minNode = minNode->left;
        root->val = minNode->val;
        root->right = deleteBST(root->right, minNode->val);
    }
    return root;
}
```

**中序遍历**（递归）得到有序序列。

---

### 7. AVL树

平衡因子 = 左高 - 右高（绝对值≤1）。

**节点定义**（含高度）：

```cpp
struct AVLNode {
    int val, height;
    AVLNode *left, *right;
    AVLNode(int x) : val(x), height(1), left(nullptr), right(nullptr) {}
};

int height(AVLNode* n) { return n ? n->height : 0; }
int getBalance(AVLNode* n) { return n ? height(n->left) - height(n->right) : 0; }
void updateHeight(AVLNode* n) { if(n) n->height = 1 + std::max(height(n->left), height(n->right)); }

// 右旋
AVLNode* rotateRight(AVLNode* y) {
    AVLNode* x = y->left;
    AVLNode* T2 = x->right;
    x->right = y;
    y->left = T2;
    updateHeight(y);
    updateHeight(x);
    return x;
}
// 左旋（对称）
AVLNode* rotateLeft(AVLNode* x) {
    AVLNode* y = x->right;
    AVLNode* T2 = y->left;
    y->left = x;
    x->right = T2;
    updateHeight(x);
    updateHeight(y);
    return y;
}

AVLNode* insertAVL(AVLNode* root, int val) {
    if (!root) return new AVLNode(val);
    if (val < root->val) root->left = insertAVL(root->left, val);
    else if (val > root->val) root->right = insertAVL(root->right, val);
    else return root; // 不允许重复

    updateHeight(root);
    int balance = getBalance(root);
    // LL
    if (balance > 1 && val < root->left->val) return rotateRight(root);
    // RR
    if (balance < -1 && val > root->right->val) return rotateLeft(root);
    // LR
    if (balance > 1 && val > root->left->val) {
        root->left = rotateLeft(root->left);
        return rotateRight(root);
    }
    // RL
    if (balance < -1 && val < root->right->val) {
        root->right = rotateRight(root->right);
        return rotateLeft(root);
    }
    return root;
}
```

删除操作类似，需处理平衡。

---

### 8. 红黑树（概念）

红黑树是自平衡BST，满足五条性质，保证最长路径≤2倍最短路径。C++标准库 `std::map` / `std::set` 通常基于红黑树实现。实现复杂，此处略。

**关键点**：插入/删除后通过变色和旋转（最多 O(log n) 次）恢复平衡。

---

### 9. B树/B+树（概念）

多路搜索树，用于磁盘存储。B树每个节点可存多个键，B+树数据仅在叶子，内部只存索引，叶子链表连接。

**B树节点结构**（示意）：

```cpp
template<int M>
struct BTreeNode {
    int keys[M];          // 实际数量 < M
    BTreeNode* children[M+1];
    int count;
    bool leaf;
};
```

插入/删除需分裂/合并，代码较长，不展开。

---

### 10. 堆

用数组实现完全二叉堆。最大堆（父≥子）。

```cpp
#include <vector>
class MaxHeap {
    std::vector<int> heap;
    void siftUp(int i) {
        while (i > 0 && heap[i] > heap[(i-1)/2]) {
            std::swap(heap[i], heap[(i-1)/2]);
            i = (i-1)/2;
        }
    }
    void siftDown(int i) {
        int n = heap.size();
        while (true) {
            int left = 2*i + 1, right = 2*i + 2, largest = i;
            if (left < n && heap[left] > heap[largest]) largest = left;
            if (right < n && heap[right] > heap[largest]) largest = right;
            if (largest == i) break;
            std::swap(heap[i], heap[largest]);
            i = largest;
        }
    }
public:
    void push(int val) {
        heap.push_back(val);
        siftUp(heap.size()-1);
    }
    int pop() {
        int top = heap[0];
        heap[0] = heap.back();
        heap.pop_back();
        if (!heap.empty()) siftDown(0);
        return top;
    }
    int top() const { return heap[0]; }
    bool empty() const { return heap.empty(); }
    // 建堆 O(n)
    void buildFromArray(const std::vector<int>& arr) {
        heap = arr;
        for (int i = (heap.size()/2)-1; i >= 0; --i) siftDown(i);
    }
};
```

**优先队列**：应用在Dijkstra、TopK等。

---

### 11. 图

**邻接表表示**（加权有向图）：

```cpp
#include <vector>
#include <list>
#include <utility>
struct Edge { int to, weight; };
class Graph {
    int V;
    std::vector<std::list<Edge>> adj;
public:
    Graph(int v) : V(v), adj(v) {}
    void addEdge(int u, int v, int w) { adj[u].push_back({v, w}); }
    // 无向图则 addEdge(v,u,w)
};
```

**DFS（递归）**：

```cpp
void dfs(int u, std::vector<bool>& visited, const std::vector<std::list<Edge>>& adj) {
    visited[u] = true;
    for (const auto& e : adj[u]) if (!visited[e.to]) dfs(e.to, visited, adj);
}
```

**BFS（队列）**：

```cpp
void bfs(int start, const std::vector<std::list<Edge>>& adj) {
    std::vector<bool> visited(adj.size(), false);
    std::queue<int> q;
    visited[start]=true; q.push(start);
    while(!q.empty()){
        int u = q.front(); q.pop();
        for(auto& e: adj[u]) if(!visited[e.to]) { visited[e.to]=true; q.push(e.to); }
    }
}
```

**Dijkstra（优先队列）**：

```cpp
#include <queue>
std::vector<int> dijkstra(const Graph& g, int src) {
    std::vector<int> dist(g.V, INT_MAX);
    dist[src]=0;
    using P = std::pair<int,int>; // distance, node
    std::priority_queue<P, std::vector<P>, std::greater<P>> pq;
    pq.push({0,src});
    while(!pq.empty()){
        auto [d,u] = pq.top(); pq.pop();
        if(d != dist[u]) continue;
        for(auto& e : g.adj[u]) {
            if(dist[e.to] > dist[u] + e.weight) {
                dist[e.to] = dist[u] + e.weight;
                pq.push({dist[e.to], e.to});
            }
        }
    }
    return dist;
}
```

**Kruskal（并查集）**见下。

---

### 12. 并查集

**路径压缩 + 按秩合并**：

```cpp
class UnionFind {
    std::vector<int> parent, rank;
public:
    UnionFind(int n) {
        parent.resize(n);
        rank.assign(n, 0);
        for(int i=0;i<n;i++) parent[i]=i;
    }
    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);
        return parent[x];
    }
    bool unite(int x, int y) {
        int rx = find(x), ry = find(y);
        if (rx == ry) return false;
        if (rank[rx] < rank[ry]) parent[rx] = ry;
        else if (rank[rx] > rank[ry]) parent[ry] = rx;
        else { parent[ry] = rx; rank[rx]++; }
        return true;
    }
};
```

**Kruskal**：边排序，并用并查集判断环。

---

### 13. 字典树（Trie）

**仅小写字母**：

```cpp
struct TrieNode {
    TrieNode* children[26] = {};
    bool isEnd = false;
};

class Trie {
    TrieNode* root;
public:
    Trie() { root = new TrieNode(); }
    void insert(const std::string& word) {
        TrieNode* cur = root;
        for (char c : word) {
            int idx = c - 'a';
            if (!cur->children[idx]) cur->children[idx] = new TrieNode();
            cur = cur->children[idx];
        }
        cur->isEnd = true;
    }
    bool search(const std::string& word) {
        TrieNode* cur = root;
        for (char c : word) {
            int idx = c - 'a';
            if (!cur->children[idx]) return false;
            cur = cur->children[idx];
        }
        return cur->isEnd;
    }
    bool startsWith(const std::string& prefix) {
        TrieNode* cur = root;
        for (char c : prefix) {
            int idx = c - 'a';
            if (!cur->children[idx]) return false;
            cur = cur->children[idx];
        }
        return true;
    }
};
```

---

### 14. 线段树

**区间求和**（递归）：

```cpp
class SegmentTree {
    int n;
    std::vector<int> tree;
    void build(const std::vector<int>& arr, int node, int l, int r) {
        if (l == r) { tree[node] = arr[l]; return; }
        int mid = (l+r)/2;
        build(arr, node*2, l, mid);
        build(arr, node*2+1, mid+1, r);
        tree[node] = tree[node*2] + tree[node*2+1];
    }
    void update(int idx, int val, int node, int l, int r) {
        if (l == r) { tree[node] = val; return; }
        int mid = (l+r)/2;
        if (idx <= mid) update(idx, val, node*2, l, mid);
        else update(idx, val, node*2+1, mid+1, r);
        tree[node] = tree[node*2] + tree[node*2+1];
    }
    int query(int ql, int qr, int node, int l, int r) {
        if (ql <= l && r <= qr) return tree[node];
        int mid = (l+r)/2;
        int res = 0;
        if (ql <= mid) res += query(ql, qr, node*2, l, mid);
        if (qr > mid) res += query(ql, qr, node*2+1, mid+1, r);
        return res;
    }
public:
    SegmentTree(const std::vector<int>& arr) {
        n = arr.size();
        tree.resize(4*n);
        build(arr, 1, 0, n-1);
    }
    void update(int idx, int val) { update(idx, val, 1, 0, n-1); }
    int query(int l, int r) { return query(l, r, 1, 0, n-1); }
};
```

**懒标记**（区间加）可扩展，此处略。

---

### 15. 树状数组

**单点更新，前缀查询**：

```cpp
class Fenwick {
    int n;
    std::vector<int> bit;
public:
    Fenwick(int n) : n(n), bit(n+1,0) {}
    void add(int idx, int delta) { // 1-based
        for (; idx <= n; idx += idx & -idx) bit[idx] += delta;
    }
    int sum(int idx) { // 前缀和 1..idx
        int res=0;
        for (; idx > 0; idx -= idx & -idx) res += bit[idx];
        return res;
    }
    int rangeSum(int l, int r) { return sum(r) - sum(l-1); }
};
```

**求逆序对**：离散化后，从左到右遍历，`sum(n)-sum(idx)` 累加，然后 `add(idx,1)`。

---

### 16. 跳表

**简易实现**（固定最大层数）：

```cpp
#include <vector>
#include <cstdlib>
#include <climits>
struct SkipNode {
    int val;
    std::vector<SkipNode*> forward;
    SkipNode(int v, int level) : val(v), forward(level, nullptr) {}
};

class SkipList {
    int maxLevel;
    float prob;
    SkipNode* head;
    int level;
public:
    SkipList(int maxLv=16, float p=0.5) : maxLevel(maxLv), prob(p), level(0) {
        head = new SkipNode(INT_MIN, maxLevel);
    }
    ~SkipList() { /* 释放所有节点 */ }

    int randomLevel() {
        int lv = 0;
        while ((float)rand()/RAND_MAX < prob && lv < maxLevel-1) lv++;
        return lv;
    }

    void insert(int val) {
        std::vector<SkipNode*> update(maxLevel, nullptr);
        SkipNode* cur = head;
        for (int i = level; i >= 0; --i) {
            while (cur->forward[i] && cur->forward[i]->val < val) cur = cur->forward[i];
            update[i] = cur;
        }
        cur = cur->forward[0];
        if (cur && cur->val == val) return; // 不允许重复

        int newLevel = randomLevel();
        if (newLevel > level) {
            for (int i = level+1; i <= newLevel; ++i) update[i] = head;
            level = newLevel;
        }
        SkipNode* newNode = new SkipNode(val, newLevel+1);
        for (int i = 0; i <= newLevel; ++i) {
            newNode->forward[i] = update[i]->forward[i];
            update[i]->forward[i] = newNode;
        }
    }

    bool search(int val) {
        SkipNode* cur = head;
        for (int i = level; i >= 0; --i) {
            while (cur->forward[i] && cur->forward[i]->val < val) cur = cur->forward[i];
        }
        cur = cur->forward[0];
        return cur && cur->val == val;
    }
    // 删除类似，略
};
```
