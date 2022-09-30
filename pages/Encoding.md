- ## UCS-2
	- Every **character** occupy 2 bytes.
- ## UTF-8
	- A variant-length unicode encoding method.
	- First byte imply bytes length.
	- | Byte Length | Byte 1 | Byte 2 | Byte 3 | Byte 4 | Byte 5 | Byte 6|
	  |-------------|-------|------|------|-----|-----|-----|
	  | 1 (ASCII) | 0xxxxxxx||||||
	  | 2 | 110xxxxx| 10xxxxxx|||||
	  | 3 | 1110xxxx| 10xxxxxx|10xxxxxx||||
	  | 4 | 11110xxx| 10xxxxxx|10xxxxxx|10xxxxxx|||
	  | 5 | 111110xx| 10xxxxxx|10xxxxxx|10xxxxxx|10xxxxxx||
	  | 6 | 1111110x| 10xxxxxx|10xxxxxx|10xxxxxx|10xxxxxx|10xxxxxx|
	- Example:
		- `E8B387` = `11101000 10110011 10000111`
		- Unicode = `1000 1100 1100 0111` = `8CC7`
	- https://leetcode.com/problems/utf-8-validation/
- ## C++: `wchar_t`
	- Ref: [C++與Unicode](https://www.ithome.com.tw/voice/135711)
	- TODO `wchar_t`