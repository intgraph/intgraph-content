[Metadata]
title: Alphabetic Encoding
date: 2014-09-06 23:56:26 

[Tags]
difficulty: 2.5
categories: dfs, implementation
source: unknown

[Description]
If a=1, b=2, c=3,....z=26. 

Given a string, find all possible codes that string can generate. Give a count as well as print the strings. 

For example:

``` 
Input: 

1123

Output:

aabc   // a = 1, a = 1, b = 2, c = 3 
kbc    // since k is 11, b = 2, c= 3 
alc    // a = 1, l = 12, c = 3 
aaw    // a= 1, a =1, w= 23 
kw     // k = 11, w = 23
```

Thanks to **cheng** who published this problem on the bbs of QQ group 待字闺中(347731858).

[Solution]
```cpp
vector<string> ans;

void encode(const string& code, size_t pos, vector<char>& chars)
{
	if (pos == code.size()) {
		string s;
		for (auto c: chars) {
			s += c;
		}
		ans.push_back(s);
	}
	int v = 0;
	while (true) { // suppose a->1, b->2 ... z->26
		v = v * 10 + (code[pos] - '0');
		if (v == 0 || v > 26) {
			return;
		}
		chars.push_back(static_cast<char>(v));
		encode(code, ++pos, chars);
		chars.pop_back();
	}
}
```

Easy piece, no more explaination. Just be ware of that ``while (true)`` loop which might lead to some tiny mistakes.
