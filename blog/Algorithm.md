# 算法学习完全指南：从入门到进阶（C++实现）

> 一份系统性的算法知识图谱，涵盖基础数据结构、经典算法思想、图论、动态规划、字符串处理及高级数据结构，所有示例均使用现代 C++（C++17/20）实现。

---

## 1. 引言

算法是程序员的“内功”，它决定了解决问题的效率与优雅程度。本文旨在为学习者提供一条从零基础到竞赛/面试水平的完整路径，内容组织由浅入深，每类算法均给出核心思路、复杂度分析和可运行的 C++ 代码片段。

---

## 2. 算法复杂度分析

### 2.1 大 O 表示法
- **O(1)**：常数时间，如数组随机访问。
- **O(log n)**：对数时间，如二分查找。
- **O(n)**：线性时间，如简单遍历。
- **O(n log n)**：常见排序算法（归并、快排）。
- **O(n²)**：平方时间，如冒泡、选择。
- **O(2ⁿ)**：指数时间，如穷举子集。

### 2.2 空间复杂度
衡量算法运行所需的额外内存，也使用大 O 表示。

---

## 3. 基础数据结构

### 3.1 数组与链表
- **数组**：连续内存，随机访问 O(1)，插入/删除 O(n)。
- **链表**（单向/双向）：插入/删除 O(1)（已知位置），随机访问 O(n)。

```cpp
// 单向链表节点
struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};
```

### 3.2 栈与队列
- **栈**（LIFO）：`std::stack<int>`。
- **队列**（FIFO）：`std::queue<int>`。
- **双端队列**：`std::deque<int>`。

### 3.3 哈希表
- `std::unordered_map` / `std::unordered_set`，平均 O(1) 查找。

### 3.4 树与二叉树
- 二叉树节点：
```cpp
struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```
- 二叉搜索树（BST）、AVL、红黑树（`std::map` / `std::set` 基于红黑树）。

### 3.5 堆（优先队列）
- `std::priority_queue` 默认最大堆，可用于堆排序、Dijkstra。

---

## 4. 排序算法

### 4.1 冒泡排序（O(n²)）
```cpp
void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n-1; ++i)
        for (int j = 0; j < n-i-1; ++j)
            if (arr[j] > arr[j+1]) swap(arr[j], arr[j+1]);
}
```

### 4.2 选择排序（O(n²)）
```cpp
void selectionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n-1; ++i) {
        int minIdx = i;
        for (int j = i+1; j < n; ++j)
            if (arr[j] < arr[minIdx]) minIdx = j;
        swap(arr[i], arr[minIdx]);
    }
}
```

### 4.3 插入排序（O(n²)，但接近有序时 O(n)）
```cpp
void insertionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 1; i < n; ++i) {
        int key = arr[i], j = i-1;
        while (j >= 0 && arr[j] > key) { arr[j+1] = arr[j]; --j; }
        arr[j+1] = key;
    }
}
```

### 4.4 希尔排序（O(n log² n)）
```cpp
void shellSort(vector<int>& arr) {
    int n = arr.size();
    for (int gap = n/2; gap > 0; gap /= 2)
        for (int i = gap; i < n; ++i) {
            int temp = arr[i], j;
            for (j = i; j >= gap && arr[j-gap] > temp; j -= gap)
                arr[j] = arr[j-gap];
            arr[j] = temp;
        }
}
```

### 4.5 归并排序（O(n log n)，稳定）
```cpp
void merge(vector<int>& arr, int l, int m, int r) {
    vector<int> left(arr.begin()+l, arr.begin()+m+1);
    vector<int> right(arr.begin()+m+1, arr.begin()+r+1);
    int i=0, j=0, k=l;
    while (i<left.size() && j<right.size())
        arr[k++] = (left[i] <= right[j]) ? left[i++] : right[j++];
    while (i<left.size()) arr[k++] = left[i++];
    while (j<right.size()) arr[k++] = right[j++];
}
void mergeSort(vector<int>& arr, int l, int r) {
    if (l >= r) return;
    int m = l + (r-l)/2;
    mergeSort(arr, l, m);
    mergeSort(arr, m+1, r);
    merge(arr, l, m, r);
}
```

### 4.6 快速排序（O(n log n) 平均，不稳定）
```cpp
int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j < high; ++j)
        if (arr[j] < pivot) swap(arr[++i], arr[j]);
    swap(arr[i+1], arr[high]);
    return i+1;
}
void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi-1);
        quickSort(arr, pi+1, high);
    }
}
```

### 4.7 堆排序（O(n log n)）
```cpp
void heapify(vector<int>& arr, int n, int i) {
    int largest = i, l = 2*i+1, r = 2*i+2;
    if (l<n && arr[l]>arr[largest]) largest = l;
    if (r<n && arr[r]>arr[largest]) largest = r;
    if (largest != i) { swap(arr[i], arr[largest]); heapify(arr, n, largest); }
}
void heapSort(vector<int>& arr) {
    int n = arr.size();
    for (int i=n/2-1; i>=0; --i) heapify(arr, n, i);
    for (int i=n-1; i>0; --i) { swap(arr[0], arr[i]); heapify(arr, i, 0); }
}
```

### 4.8 计数排序（O(n+k)，稳定，适合整数）
```cpp
void countingSort(vector<int>& arr) {
    int maxVal = *max_element(arr.begin(), arr.end());
    vector<int> count(maxVal+1, 0), output(arr.size());
    for (int x : arr) count[x]++;
    for (int i=1; i<=maxVal; ++i) count[i] += count[i-1];
    for (int i=arr.size()-1; i>=0; --i) {
        output[count[arr[i]]-1] = arr[i];
        count[arr[i]]--;
    }
    arr = output;
}
```

### 4.9 基数排序（O(d*(n+k))，d 为数位）
```cpp
// 略，基于计数排序按位处理
```

### 4.10 桶排序（O(n) 平均，适合均匀分布）

---

## 5. 搜索算法

### 5.1 线性搜索（O(n)）
```cpp
int linearSearch(const vector<int>& arr, int target) {
    for (int i=0; i<arr.size(); ++i) if (arr[i]==target) return i;
    return -1;
}
```

### 5.2 二分搜索（O(log n)，有序）
```cpp
int binarySearch(const vector<int>& arr, int target) {
    int l=0, r=arr.size()-1;
    while (l <= r) {
        int mid = l + (r-l)/2;
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) l = mid+1;
        else r = mid-1;
    }
    return -1;
}
```

### 5.3 深度优先搜索（DFS）—— 栈/递归
```cpp
// 图用邻接表
void dfs(int u, vector<vector<int>>& adj, vector<bool>& visited) {
    visited[u] = true;
    cout << u << " ";
    for (int v : adj[u]) if (!visited[v]) dfs(v, adj, visited);
}
```

### 5.4 广度优先搜索（BFS）—— 队列
```cpp
void bfs(int start, vector<vector<int>>& adj) {
    vector<bool> visited(adj.size(), false);
    queue<int> q;
    q.push(start); visited[start] = true;
    while (!q.empty()) {
        int u = q.front(); q.pop();
        cout << u << " ";
        for (int v : adj[u]) if (!visited[v]) { visited[v]=true; q.push(v); }
    }
}
```

---

## 6. 图论算法

### 6.1 最短路径
#### Dijkstra（非负权，O(E log V)）
```cpp
vector<int> dijkstra(int src, vector<vector<pair<int,int>>>& adj) {
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    vector<int> dist(adj.size(), INT_MAX);
    dist[src] = 0; pq.push({0, src});
    while (!pq.empty()) {
        auto [d, u] = pq.top(); pq.pop();
        if (d > dist[u]) continue;
        for (auto [v, w] : adj[u]) {
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                pq.push({dist[v], v});
            }
        }
    }
    return dist;
}
```

#### Bellman-Ford（可负权，O(VE)）
```cpp
vector<int> bellmanFord(int src, vector<tuple<int,int,int>>& edges, int V) {
    vector<int> dist(V, INT_MAX); dist[src] = 0;
    for (int i=0; i<V-1; ++i) {
        for (auto [u,v,w] : edges) {
            if (dist[u] != INT_MAX && dist[u] + w < dist[v])
                dist[v] = dist[u] + w;
        }
    }
    // 检测负环
    for (auto [u,v,w] : edges) {
        if (dist[u] != INT_MAX && dist[u] + w < dist[v])
            return {}; // 负环
    }
    return dist;
}
```

#### Floyd-Warshall（全源，O(V³)）
```cpp
void floydWarshall(vector<vector<int>>& dist) {
    int V = dist.size();
    for (int k=0; k<V; ++k)
        for (int i=0; i<V; ++i)
            for (int j=0; j<V; ++j)
                if (dist[i][k] != INT_MAX && dist[k][j] != INT_MAX)
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
}
```

### 6.2 最小生成树
#### Prim（O(E log V)）
```cpp
int prim(vector<vector<pair<int,int>>>& adj) {
    int V = adj.size(), cost = 0;
    vector<int> key(V, INT_MAX);
    vector<bool> inMST(V, false);
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    key[0] = 0; pq.push({0,0});
    while (!pq.empty()) {
        auto [w, u] = pq.top(); pq.pop();
        if (inMST[u]) continue;
        inMST[u] = true;
        cost += w;
        for (auto [v, wt] : adj[u]) {
            if (!inMST[v] && wt < key[v]) {
                key[v] = wt;
                pq.push({key[v], v});
            }
        }
    }
    return cost;
}
```

#### Kruskal（O(E log E)）
```cpp
struct DSU { vector<int> p, r; DSU(int n): p(n), r(n,0) { iota(p.begin(), p.end(), 0); }
    int find(int x) { return p[x]==x ? x : p[x]=find(p[x]); }
    bool unite(int a, int b) { a=find(a); b=find(b); if(a==b)return false; if(r[a]<r[b]) swap(a,b); p[b]=a; if(r[a]==r[b]) r[a]++; return true; }
};
int kruskal(vector<tuple<int,int,int>>& edges, int V) {
    sort(edges.begin(), edges.end());
    DSU dsu(V);
    int cost = 0, cnt = 0;
    for (auto [w,u,v] : edges) {
        if (dsu.unite(u,v)) { cost += w; if (++cnt == V-1) break; }
    }
    return cost;
}
```

### 6.3 拓扑排序（DAG）
```cpp
vector<int> topologicalSort(vector<vector<int>>& adj) {
    int V=adj.size();
    vector<int> indeg(V,0), res;
    for (auto& neigh : adj) for (int v : neigh) indeg[v]++;
    queue<int> q;
    for (int i=0; i<V; ++i) if (indeg[i]==0) q.push(i);
    while (!q.empty()) {
        int u = q.front(); q.pop();
        res.push_back(u);
        for (int v : adj[u]) if (--indeg[v] == 0) q.push(v);
    }
    return res.size()==V ? res : vector<int>(); // 有环则返回空
}
```

### 6.4 强连通分量（Kosaraju / Tarjan）
```cpp
// Kosaraju: 两次 DFS，略
```

---

## 7. 动态规划（DP）

### 7.1 0/1 背包
```cpp
int knapsack01(int W, vector<int>& wt, vector<int>& val) {
    int n = wt.size();
    vector<int> dp(W+1, 0);
    for (int i=0; i<n; ++i)
        for (int w=W; w>=wt[i]; --w)
            dp[w] = max(dp[w], dp[w-wt[i]] + val[i]);
    return dp[W];
}
```

### 7.2 完全背包
```cpp
// 内层正序遍历
for (int w=wt[i]; w<=W; ++w) dp[w] = max(dp[w], dp[w-wt[i]]+val[i]);
```

### 7.3 最长公共子序列（LCS）
```cpp
int LCS(string a, string b) {
    int m=a.size(), n=b.size();
    vector<vector<int>> dp(m+1, vector<int>(n+1,0));
    for (int i=1;i<=m;++i)
        for (int j=1;j<=n;++j)
            if (a[i-1]==b[j-1]) dp[i][j]=dp[i-1][j-1]+1;
            else dp[i][j]=max(dp[i-1][j], dp[i][j-1]);
    return dp[m][n];
}
```

### 7.4 最长递增子序列（LIS, O(n log n)）
```cpp
int LIS(vector<int>& arr) {
    vector<int> tail;
    for (int x : arr) {
        auto it = lower_bound(tail.begin(), tail.end(), x);
        if (it == tail.end()) tail.push_back(x);
        else *it = x;
    }
    return tail.size();
}
```

### 7.5 编辑距离（Levenshtein）
```cpp
int editDist(string a, string b) {
    int m=a.size(), n=b.size();
    vector<vector<int>> dp(m+1, vector<int>(n+1));
    for (int i=0;i<=m;++i) dp[i][0]=i;
    for (int j=0;j<=n;++j) dp[0][j]=j;
    for (int i=1;i<=m;++i)
        for (int j=1;j<=n;++j)
            if (a[i-1]==b[j-1]) dp[i][j]=dp[i-1][j-1];
            else dp[i][j]=1+min({dp[i-1][j], dp[i][j-1], dp[i-1][j-1]});
    return dp[m][n];
}
```

### 7.6 区间 DP（矩阵链乘法）
```cpp
int matrixChain(vector<int>& dims) {
    int n=dims.size()-1;
    vector<vector<int>> dp(n, vector<int>(n,0));
    for (int len=2; len<=n; ++len)
        for (int i=0; i+len-1<n; ++i) {
            int j=i+len-1;
            dp[i][j]=INT_MAX;
            for (int k=i; k<j; ++k)
                dp[i][j]=min(dp[i][j], dp[i][k]+dp[k+1][j]+dims[i]*dims[k+1]*dims[j+1]);
        }
    return dp[0][n-1];
}
```

---

## 8. 贪心算法

### 8.1 活动选择（区间调度）
```cpp
int activitySelection(vector<pair<int,int>>& activities) {
    sort(activities.begin(), activities.end(), [](auto& a, auto& b){ return a.second < b.second; });
    int cnt=1, lastEnd=activities[0].second;
    for (int i=1; i<activities.size(); ++i)
        if (activities[i].first >= lastEnd) { cnt++; lastEnd = activities[i].second; }
    return cnt;
}
```

### 8.2 霍夫曼编码（优先队列）
```cpp
struct Node { int freq; char ch; Node *l,*r; Node(int f, char c='\0', Node* L=nullptr, Node* R=nullptr): freq(f), ch(c), l(L), r(R) {} };
struct cmp { bool operator()(Node* a, Node* b) { return a->freq > b->freq; } };
Node* buildHuffman(vector<pair<char,int>>& freq) {
    priority_queue<Node*, vector<Node*>, cmp> pq;
    for (auto [ch, f] : freq) pq.push(new Node(f, ch));
    while (pq.size() > 1) {
        auto a=pq.top(); pq.pop();
        auto b=pq.top(); pq.pop();
        pq.push(new Node(a->freq+b->freq, '\0', a, b));
    }
    return pq.top();
}
```

---

## 9. 分治算法

### 9.1 归并排序、快速排序（已在上文）
### 9.2 最近点对（O(n log n)）
```cpp
// 典型分治，略（涉及按x排序，递归合并，按y筛选）
```

---

## 10. 回溯算法

### 10.1 N 皇后
```cpp
bool isSafe(vector<string>& board, int row, int col, int n) {
    for (int i=0; i<row; ++i) if (board[i][col]=='Q') return false;
    for (int i=row-1, j=col-1; i>=0 && j>=0; --i, --j) if (board[i][j]=='Q') return false;
    for (int i=row-1, j=col+1; i>=0 && j<n; --i, ++j) if (board[i][j]=='Q') return false;
    return true;
}
void solveNQueens(int n, int row, vector<string>& board, vector<vector<string>>& res) {
    if (row == n) { res.push_back(board); return; }
    for (int col=0; col<n; ++col) {
        if (isSafe(board, row, col, n)) {
            board[row][col]='Q';
            solveNQueens(n, row+1, board, res);
            board[row][col]='.';
        }
    }
}
```

### 10.2 子集生成（组合）
```cpp
void subsets(vector<int>& nums, int idx, vector<int>& cur, vector<vector<int>>& res) {
    res.push_back(cur);
    for (int i=idx; i<nums.size(); ++i) { cur.push_back(nums[i]); subsets(nums, i+1, cur, res); cur.pop_back(); }
}
```

### 10.3 全排列
```cpp
void permute(vector<int>& nums, int l, vector<vector<int>>& res) {
    if (l == nums.size()-1) { res.push_back(nums); return; }
    for (int i=l; i<nums.size(); ++i) { swap(nums[l], nums[i]); permute(nums, l+1, res); swap(nums[l], nums[i]); }
}
```

---

## 11. 字符串算法

### 11.1 KMP 算法（O(n+m)）
```cpp
vector<int> buildPi(const string& pat) {
    int m=pat.size();
    vector<int> pi(m,0);
    for (int i=1, j=0; i<m; ++i) {
        while (j>0 && pat[i]!=pat[j]) j=pi[j-1];
        if (pat[i]==pat[j]) ++j;
        pi[i]=j;
    }
    return pi;
}
vector<int> kmpSearch(const string& text, const string& pat) {
    vector<int> pi=buildPi(pat), res;
    for (int i=0, j=0; i<text.size(); ++i) {
        while (j>0 && text[i]!=pat[j]) j=pi[j-1];
        if (text[i]==pat[j]) ++j;
        if (j == pat.size()) { res.push_back(i-j+1); j=pi[j-1]; }
    }
    return res;
}
```

### 11.2 Rabin-Karp（滚动哈希）
```cpp
// 略，使用前缀哈希和模数
```

### 11.3 Trie 树
```cpp
struct TrieNode {
    bool isEnd;
    vector<TrieNode*> children;
    TrieNode() : isEnd(false), children(26, nullptr) {}
};
class Trie {
    TrieNode* root;
public:
    Trie() { root = new TrieNode(); }
    void insert(string word) {
        TrieNode* cur = root;
        for (char c : word) {
            int idx = c-'a';
            if (!cur->children[idx]) cur->children[idx] = new TrieNode();
            cur = cur->children[idx];
        }
        cur->isEnd = true;
    }
    bool search(string word) {
        TrieNode* cur = root;
        for (char c : word) {
            int idx = c-'a';
            if (!cur->children[idx]) return false;
            cur = cur->children[idx];
        }
        return cur->isEnd;
    }
};
```

### 11.4 AC 自动机（多模式匹配）
```cpp
// 较复杂，核心是 Trie + fail 指针，略
```

---

## 12. 数学算法

### 12.1 素数筛（埃氏筛 O(n log log n)，欧拉筛 O(n)）
```cpp
vector<int> sieve(int n) {
    vector<bool> isPrime(n+1, true);
    vector<int> primes;
    isPrime[0]=isPrime[1]=false;
    for (int i=2; i<=n; ++i) {
        if (isPrime[i]) {
            primes.push_back(i);
            for (long long j=1LL*i*i; j<=n; j+=i) isPrime[j]=false;
        }
    }
    return primes;
}
```

### 12.2 欧几里得（GCD）
```cpp
int gcd(int a, int b) { return b ? gcd(b, a%b) : a; }
int lcm(int a, int b) { return a / gcd(a,b) * b; }
```

### 12.3 快速幂（O(log n)）
```cpp
long long modPow(long long a, long long e, long long mod) {
    long long res=1;
    while (e) { if (e&1) res=res*a%mod; a=a*a%mod; e>>=1; }
    return res;
}
```

### 12.4 矩阵快速幂（用于斐波那契等）
```cpp
using Matrix = vector<vector<long long>>;
Matrix multiply(const Matrix& A, const Matrix& B) {
    int n=A.size(), m=B[0].size(), p=B.size();
    Matrix C(n, vector<long long>(m,0));
    for (int i=0;i<n;++i)
        for (int k=0;k<p;++k)
            for (int j=0;j<m;++j)
                C[i][j] = (C[i][j] + A[i][k]*B[k][j]) % MOD;
    return C;
}
Matrix matPow(Matrix base, long long exp) {
    int n=base.size();
    Matrix res(n, vector<long long>(n,0));
    for (int i=0;i<n;++i) res[i][i]=1;
    while (exp) { if (exp&1) res=multiply(res,base); base=multiply(base,base); exp>>=1; }
    return res;
}
```

### 12.5 组合数（预处理阶乘）
```cpp
vector<long long> fact, invFact;
void initComb(int N, long long mod) {
    fact.resize(N+1); invFact.resize(N+1);
    fact[0]=1;
    for (int i=1;i<=N;++i) fact[i]=fact[i-1]*i%mod;
    invFact[N]=modPow(fact[N], mod-2, mod);
    for (int i=N-1;i>=0;--i) invFact[i]=invFact[i+1]*(i+1)%mod;
}
long long nCr(int n, int r, long long mod) {
    if (r<0 || r>n) return 0;
    return fact[n]*invFact[r]%mod*invFact[n-r]%mod;
}
```

---

## 13. 高级数据结构

### 13.1 并查集（DSU）
```cpp
class DSU {
    vector<int> parent, rank;
public:
    DSU(int n) { parent.resize(n); rank.resize(n,0); iota(parent.begin(), parent.end(), 0); }
    int find(int x) { return parent[x]==x ? x : parent[x]=find(parent[x]); }
    bool unite(int a, int b) {
        a=find(a); b=find(b);
        if (a==b) return false;
        if (rank[a]<rank[b]) swap(a,b);
        parent[b]=a;
        if (rank[a]==rank[b]) rank[a]++;
        return true;
    }
};
```

### 13.2 线段树（区间求和/最值）
```cpp
class SegTree {
    int n;
    vector<int> tree;
public:
    SegTree(vector<int>& arr) { n=arr.size(); tree.resize(4*n); build(arr,1,0,n-1); }
    void build(vector<int>& arr, int node, int l, int r) {
        if (l==r) { tree[node]=arr[l]; return; }
        int mid=(l+r)/2;
        build(arr, node*2, l, mid);
        build(arr, node*2+1, mid+1, r);
        tree[node]=tree[node*2]+tree[node*2+1];
    }
    void update(int idx, int val, int node, int l, int r) {
        if (l==r) { tree[node]=val; return; }
        int mid=(l+r)/2;
        if (idx<=mid) update(idx,val,node*2,l,mid);
        else update(idx,val,node*2+1,mid+1,r);
        tree[node]=tree[node*2]+tree[node*2+1];
    }
    int query(int L, int R, int node, int l, int r) {
        if (R<l || r<L) return 0;
        if (L<=l && r<=R) return tree[node];
        int mid=(l+r)/2;
        return query(L,R,node*2,l,mid) + query(L,R,node*2+1,mid+1,r);
    }
};
```

### 13.3 树状数组（Fenwick Tree）
```cpp
class BIT {
    int n; vector<int> bit;
public:
    BIT(int n): n(n), bit(n+1,0) {}
    void add(int idx, int val) { for (++idx; idx<=n; idx += idx&-idx) bit[idx]+=val; }
    int sum(int idx) { int res=0; for (++idx; idx>0; idx -= idx&-idx) res+=bit[idx]; return res; }
    int rangeSum(int l, int r) { return sum(r) - (l?sum(l-1):0); }
};
```

### 13.4 平衡树（这里以 `std::set` / `std::map` 代表）
### 13.5 可持久化线段树（主席树）
```cpp
// 复杂，仅示意结构
struct Node { int l, r, sum; };
vector<Node> tree;
int update(int prev, int l, int r, int pos) { ... } // 每次新建节点
```

---

## 14. 进阶主题

### 14.1 网络流（最大流 Dinic）
```cpp
struct Dinic {
    struct Edge { int to, rev, cap; };
    vector<vector<Edge>> g;
    vector<int> level, it;
    Dinic(int n): g(n), level(n), it(n) {}
    void addEdge(int v, int to, int cap) {
        g[v].push_back({to, (int)g[to].size(), cap});
        g[to].push_back({v, (int)g[v].size()-1, 0});
    }
    bool bfs(int s, int t) {
        fill(level.begin(), level.end(), -1);
        queue<int> q; level[s]=0; q.push(s);
        while(!q.empty()) {
            int v=q.front(); q.pop();
            for (auto& e:g[v]) if(e.cap>0 && level[e.to]<0) {
                level[e.to]=level[v]+1;
                q.push(e.to);
            }
        }
        return level[t]>=0;
    }
    int dfs(int v, int t, int f) {
        if(v==t) return f;
        for(int &i=it[v]; i<g[v].size(); ++i) {
            Edge &e=g[v][i];
            if(e.cap>0 && level[v]<level[e.to]) {
                int d=dfs(e.to, t, min(f, e.cap));
                if(d>0) { e.cap-=d; g[e.to][e.rev].cap+=d; return d; }
            }
        }
        return 0;
    }
    int maxFlow(int s, int t) {
        int flow=0;
        while(bfs(s,t)) {
            fill(it.begin(), it.end(), 0);
            while(int f=dfs(s,t,INT_MAX)) flow+=f;
        }
        return flow;
    }
};
```

### 14.2 二分图匹配（匈牙利算法）
```cpp
bool dfs(int u, vector<vector<int>>& adj, vector<int>& match, vector<bool>& visited) {
    for (int v : adj[u]) {
        if (visited[v]) continue;
        visited[v]=true;
        if (match[v]==-1 || dfs(match[v], adj, match, visited)) {
            match[v]=u; return true;
        }
    }
    return false;
}
int hungarian(vector<vector<int>>& adj, int nLeft, int nRight) {
    vector<int> match(nRight, -1);
    int result=0;
    for (int u=0; u<nLeft; ++u) {
        vector<bool> visited(nRight, false);
        if (dfs(u, adj, match, visited)) result++;
    }
    return result;
}
```

### 14.3 最近公共祖先（LCA，倍增法）
```cpp
class LCA {
    int n, LOG;
    vector<vector<int>> adj, up;
    vector<int> depth;
public:
    LCA(int n, vector<vector<int>>& adj): n(n), adj(adj) {
        LOG = ceil(log2(n)) + 1;
        up.assign(n, vector<int>(LOG));
        depth.assign(n,0);
        dfs(0,0);
    }
    void dfs(int u, int p) {
        up[u][0]=p;
        for (int i=1;i<LOG;++i) up[u][i]=up[ up[u][i-1] ][i-1];
        for (int v:adj[u]) if(v!=p) { depth[v]=depth[u]+1; dfs(v,u); }
    }
    int lca(int a, int b) {
        if(depth[a]<depth[b]) swap(a,b);
        int diff=depth[a]-depth[b];
        for(int i=0; i<LOG; ++i) if(diff & (1<<i)) a=up[a][i];
        if(a==b) return a;
        for(int i=LOG-1;i>=0;--i) if(up[a][i]!=up[b][i]) { a=up[a][i]; b=up[b][i]; }
        return up[a][0];
    }
};
```

### 14.4 树链剖分（重链剖分）
```cpp
// 用于路径更新/查询，配合线段树，代码较长，略
```
