# 最长回文子串(Longest Palindrome Substring)

## 参考博客
[思路讲的很好](http://www.jianshu.com/p/799bc53d4e3d)
[Leetcode图画的很好](http://articles.leetcode.com/longest-palindromic-substring-part-ii/)

## 算法思路
1. 在原数组两断以及字符之间添加特殊字符如'#'。将偶数长度回文子串转换为以'#'为中心的奇数长度回文子串。
2. 定义一个回文半径数组RL，RL[i]表示以第i个字符为对称轴的回文串的回文半径（回文串中最右位置的字符与对称轴字符的距离叫做回文半径）。
3. 保存当前回文探测到的最右端字符的位置max_right，以及对应的对称轴位置max_mid.
4. 当探测到第i个字符：
    1) 如果i>max_right，则直接RL[i]=0。
    2) 如果i<=max_right，则根据与i为对称轴的回文串是否与其关于max_mid轴对称的回文串(以mirror_i为对称轴的回文串)重叠，RL[i]取max_right-i（重叠），RL[mirror_i]（不重叠）中的最小值。

    接下来尝试向外扩展回文串。

> 不重叠
![no overlap](http://7xnluw.com1.z0.glb.clouddn.com/Algorithm/palindrome_no_overlap.png)
> 重叠
![overlap](http://7xnluw.com1.z0.glb.clouddn.com/Algorithm/palindrome_overlap.png)

## code
```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        string s1;
        size_t n = s.size();
        s1.resize(2 * n + 1);
        s1[0] = split_;
        for (size_t i = 0; i < n; i++)
        {
            s1[2 * i + 1] = s[i];
            s1[2 * i + 2] = split_;
        }

        vector<int> RL(s1.size(), 0); // 回文半径
        int max_right = 0;
        int max_mid = 0;

        for (int i = 1; i < s1.size(); i++)
        {
            int mirror_i = 2 * max_mid - i;

            // if i is in 0~max_right, then use mirror_i's value or the max_right - i
            if (i <= max_right)
                RL[i] = min(max_right - i, RL[mirror_i]);
            else
                RL[i] = 0;

            // extend the palindome
            while (i+RL[i]+1<s1.size() && i-RL[i]-1>=0 && s1[i + RL[i]+1] == s1[i - RL[i]-1])
                RL[i]++;

            // update max_mid and max_right
            if (i + RL[i] > max_right)
            {
                max_right = i + RL[i];
                max_mid = i;
            }
        }

        int substr_center = 0;
        int substr_len = 0;
        for (int i = 0; i < RL.size(); i++)
        {
            if (RL[i] > substr_len)
            {
                substr_center = i;
                substr_len = RL[i];
            }
        }

        return s.substr((substr_center - substr_len) / 2, substr_len);
    }
private:
    const char split_ = '#';
};
```
