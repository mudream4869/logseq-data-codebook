- Use in **coding competition**.
- Template for making coding in competitions faster.
- ## Starter Template
  collapsed:: true
	- ```cpp
	  #include <bits/stdc++.h>
	  
	  using namespace std;
	  
	  int main() {
	      ios::sync_with_stdio(false);
	      cin.tie(nullptr);
	      int T;
	      cin >> T;
	      for (int lt = 1; lt <= T; ++lt) {
	          // TODO
	      }
	      return 0;
	  }
	  ```
- ## Secure Random Generator
  collapsed:: true
	- ```cpp
	  // gen [0, upper)*n
	  // usage:
	  //  mt19937_64 rng(chrono::steady_clock::now().time_since_epoch().count());
	  //  auto v1 = genRand(rng, 100000, 10000000);
	  //  auto v2 = genRand(rng, 100000, 12343212);
	  vector<int64_t> genRand(mt19937_64& rng, size_t n, int64_t upper) {
	      uniform_int_distribution<int64_t> valDistr(0, upper - 1);
	      uniform_int_distribution<int64_t> rangDistr(0, n - 1);
	  
	      set<int64_t> vals;
	      while (vals.size() < n) {
	          vals.insert(valDistr(rng));
	      }
	  
	      vector<int64_t> ret(vals.begin(), vals.end());
	      for (size_t i = 0; i < n; ++i) {
	          swap(ret[i], ret[rangDistr(rng)]);
	      }
	  
	      return vector<int64_t>(vals.begin(), vals.end());
	  }
	  ```
- ## [KMP](((631ea11b-05e1-4a96-bb44-d609c67a35fe)))
  collapsed:: true
	- ```cpp
	  // Morris Pratt: Find all mathing positions
	  template <typename T>
	  vector<int> mpMatching(const vector<T>& t, const vector<T>& p) {
	      vector<int> borderFunc(p.size(), -1);
	      for (int i = 1, j = borderFunc[0]; i < p.size(); ++i) {
	          while (j >= 0 && p[j + 1] != p[i]) {
	              j = borderFunc[j];
	          }
	  
	          if (p[j + 1] == p[i]) {
	              ++j;
	          }
	  
	          borderFunc[i] = j;
	      }
	  
	      vector<int> posMatching;
	      for (int i = 0, j = -1; i < t.size(); ++i) {
	          while (j >= 0 && p[j + 1] != t[i]) {
	              j = borderFunc[j];
	          }
	  
	          if (p[j + 1] == t[i]) {
	              ++j;
	          }
	  
	          if (j == p.size() - 1) {
	              posMatching.push_back(i - p.size() + 1);
	              j = borderFunc[j];
	          }
	      }
	      return posMatching;
	  }
	  
	  ```
- ## Rolling Hash
  collapsed:: true
	- Hash string $(s_0, s_1, ... s_{n-1})$ to $(\sum_{i = 0}^{n-1} s_ix^{n-i-1}) \mod p$
	- ```cpp
	  template<typename T>
	  class RollingHash {
	  public:
	      RollingHash(const std::vector<T>& s, int64_t x, int64_t p):
	              p_(p), hash_(s.size() + 1), xPow_(s.size() + 1) {
	          hash_[0] = 0;
	          for (int i = 0; i < s.size(); ++i) {
	              hash_[i + 1] = hash_[i]*x % p;
	              hash_[i + 1] += s[i];
	              hash_[i + 1] %= p;
	          }
	          
	          xPow_[0] = 1;
	          for (int i = 1; i < xPow_.size(); ++i) {
	              xPow_[i] = (xPow_[i-1] * x) % p;
	          }
	      }
	      
	      int64_t get(int i, int j) const {
	          // return s[i, j) hash
	          // hash_[j] - hash_[i]*xPow[j-i]
	          int64_t v = hash_[i] * xPow_[j - i] % p_;
	          v = hash_[j] + (p_ - v);
	          return v % p_;
	      }
	  
	  private:
	      int64_t p_;
	      vector<int64_t> hash_;
	      vector<int64_t> xPow_;
	  };
	  ```
- ## Range Modify Query (RMQ)
  collapsed:: true
	- ### zkw tree
		- Source: [統計的力量](https://www.slideshare.net/DanielChou/ss-7792670)
		- ```cpp
		  // modify in single point, sum a range
		  struct ZKWTree {
		      vector<int64_t> data;
		      size_t base;
		  
		      ZKWTree(size_t n) {
		          base = 1 << __lg(n + 5) + 1;
		          data = vector<int64_t>(base << 1, 0);
		      }
		  
		      // x in [0, n)
		      void update(size_t x, int64_t v) {
		          ++x;  // 0-base to 1-base
		          x += base;
		          data[x] = v;
		          for (x >>= 1; x; x >>= 1) {
		              data[x] = data[x << 1] + data[(x << 1) | 1];
		          }
		      }
		  
		      // [l, r]
		      // l, r in [0, n)
		      int64_t query(size_t l, size_t r) const {
		          ++l, ++r;  // 0-base to 1-base
		          int64_t ans = 0;
		          for (l += base - 1, r += base + 1; l ^ r ^ 1; l >>= 1, r >>= 1) {
		              if (~l & 1) {
		                  ans += data[l ^ 1];
		              }
		              if (r & 1) {
		                  ans += data[r ^ 1];
		              }
		          }
		          return ans;
		      }
		  };
		  ```
	- ### Dynamic Segment Tree
		- Range Modify and Range Query
		- Dynamic create nodes
		- ```cpp
		  class SegmentTree {
		  private:
		      struct Node;
		  
		      struct Node {
		          Node(int l, int r): l(l), r(r) {
		          }
		  
		          void makeLeft() {
		              if (left) {
		                  return;
		              }
		              int m = (l + r) >> 1;
		              left = new Node(l, m);
		          }
		  
		          void makeRight() {
		              if (right) {
		                  return;
		              }
		              int m = (l + r) >> 1;
		              right = new Node(m, r);
		          }
		  
		          void update() {
		              int v = 0;
		              if (left) {
		                  v = max(v, left->maxSumDelta);
		              }
		              if (right) {
		                  v = max(v, right->maxSumDelta);
		              }
		              maxSumDelta = v + delta;
		          }
		  
		          Node* left = nullptr;
		          Node* right = nullptr;
		  
		          // [l, r)
		          int l;
		          int r;
		  
		          // delta of the whole range
		          int delta = 0;
		  
		          // max of sum of delta 
		          int maxSumDelta = 0;
		      };
		  
		  public:
		      // create [0, R)
		      SegmentTree(int R): root(new Node(0, R)) {
		      }
		  
		      SegmentTree(const SegmentTree&) = delete;
		      SegmentTree& operator=(const SegmentTree&) = delete;
		  
		      ~SegmentTree() {
		          if (!root) {
		              return;
		          }
		          nodeDelete(root);
		      }
		  
		      // add v to [l, r)
		      void add(int l, int r, int v) {
		          nodeAdd(l, r, v, root);
		      }
		  
		      // return max in [l, r)
		      int get(int l, int r) const {
		          return nodeGet(l, r, root);
		      }
		  
		  private:
		      Node* root;
		  
		      void nodeDelete(Node* node) {
		          if (node->right) {
		              nodeDelete(node->right);
		          }
		          if (node->left) {
		              nodeDelete(node->left);
		          }
		          delete node;
		      }
		  
		      void nodeAdd(int l, int r, int v, Node* node) {
		          // assert:  node->l <= l < r <= node->r
		  
		          if (node->l == l && node->r == r) {
		              node->delta += v;
		              node->update();
		              // cout << "add: [" << l << "," << r << "): " << v << endl;
		              return;
		          }
		  
		          int m = (node->l + node->r) >> 1;
		          if (r <= m) {
		              node->makeLeft();
		              nodeAdd(l, r, v, node->left);
		              node->update();
		              return;
		          } else if (m <= l) {
		              node->makeRight();
		              nodeAdd(l, r, v, node->right);
		              node->update();
		              return;
		          }
		          
		          // l < m && m < r
		          node->makeLeft();
		          node->makeRight();
		          nodeAdd(l, m, v, node->left);
		          nodeAdd(m, r, v, node->right);
		          node->update();
		      }
		  
		      int nodeGet(int l, int r, Node* node) const {
		          // assert:  node->l <= l < r <= node->r
		          if (!node) {
		              return 0;
		          }
		  
		          if (node->l == l && node->r == r) {
		              // cout << "get: [" << l << "," << r << "): " << node->maxSumDelta << endl;
		              return node->maxSumDelta;
		          }
		  
		          int m = (node->l + node->r) >> 1;
		          if (r <= m) {
		              return node->delta + nodeGet(l, r, node->left);
		          } else if (m <= l) {
		              return node->delta + nodeGet(l, r, node->right);
		          }
		  
		          return node->delta + max(nodeGet(l, m, node->left), nodeGet(m, r, node->right));
		      }
		  };
		  ```
- ## Disjoint Set
  collapsed:: true
	- ```cpp
	  struct DisjointSet {
	      vector<int> root_;
	      vector<int> size_;
	  
	      DisjointSet(int n) : root_(n), size_(n, 1) {
	          iota(root_.begin(), root_.end(), 0);
	      }
	  
	      void join(int a, int b) {
	          a = root(a);
	          b = root(b);
	          if (a == b) {
	              return;
	          }
	  
	          int large = a;
	          if (size_[a] > size_[b]) {
	              large = a;
	          } else {
	              large = b;
	          }
	          int small = a ^ b ^ large;
	  
	          size_[large] += size_[small];
	          root_[small] = large;
	      }
	  
	      int order(int a) {
	          return size_[root(a)];
	      }
	  
	      // should not be dfs, since stack also cost memory!
	      int root(int a) {
	          while (root_[a] != a) {
	              root_[a] = root_[root_[a]];
	              a = root_[a];
	          }
	          return a;
	      }
	  };
	  ```