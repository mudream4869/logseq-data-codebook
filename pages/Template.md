- Use in **coding competition**.
- Template for making coding in codeforces' competition faster.
- ## Starter Template
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