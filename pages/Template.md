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
	  collapsed:: true
		- Source: [統計的力量](https://www.slideshare.net/DanielChou/ss-7792670)
		- ```cpp
		  // modify in single point, sum a range
		  struct ZKWTree {
		      vector<int> data;
		      int base;
		      ZKWTree(int n) {
		          base = 1 << __lg(n + 5) + 1;
		          data = vector<int>(base << 1, 0);
		      }
		  
		      void update(int x, int v) {
		          ++x;  // 0-base to 1-base
		          for (int i = x + base; i; i >>= 1) {
		              data[i] += v;
		          }
		      }
		  
		      // [l, r]
		      int query(int l, int r) {
		          ++l, ++r;  // 0-base to 1-base
		          int ans = 0;
		          for (l += base - 1, r += base + 1; l ^ r ^ 1; l >>= 1, r >>= 1) {
		              if (~l & 1) ans += data[l ^ 1];
		              if (r & 1) ans += data[r ^ 1];
		          }
		          return ans;
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