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