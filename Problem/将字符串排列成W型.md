# 将字符串排列成W型

```c++
	vector<vector<char>> wxingstr(string s, int n)
	{
		int len = s.length();
		int lay = 0;
		bool zheng = true;
		vector<vector<char>> ans(n, vector<char>(len, ' '));
		for (int i = 0; i < len; i++)
		{
			ans[lay][i] = s[i];
			if (zheng)
				lay++;
			if (!zheng)
				lay--;
			if (lay == 0)
				zheng = true;
			if (lay == n - 1)
				zheng = false;
		}
		return ans;
	}
```

