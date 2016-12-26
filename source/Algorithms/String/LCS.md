# 最长公共子串（Longest Common Substring）

```cpp
//============================================================================
// Name        : LongestCommonSubstr.cpp
// Author      : mirsking
// Version     :
// Copyright   :
// Description : Hello World in C++, Ansi-style
//============================================================================

#include <iostream>
#include <string>
#include <vector>
using namespace std;

int longestCommonSubstr(const string& lhs, const string& rhs, vector<string>& res)
{
    vector<vector<int> > dp(lhs.size()+1, vector<int>(rhs.size()+1, 0));
    int max_len = 0;
    vector<int> starts;
    for(size_t i=0; i<lhs.size(); i++)
    {
        for(size_t j=0; j<rhs.size(); j++)
        {
            if(lhs[i]==rhs[j])
            {
                dp[i+1][j+1] = dp[i][j] + 1;
                if(dp[i+1][j+1] > max_len)
                {
                    max_len = dp[i+1][j+1];
                    starts.clear();
                    starts.push_back(i+1-max_len);
                }
                else if(dp[i+1][j+1] == max_len)
                {
                    starts.push_back(i+1-max_len);
                }
            }
            else
            {
                dp[i+1][j+1] = 0;
            }
        }
    }

    for(int i=0; i<starts.size(); i++)
    {
        res.push_back(lhs.substr(starts[i], max_len));
    }

    return max_len;
}

int main() {

    string str1 = "abc";
    string str2 = "dddddabddd";
    vector<string> res;
    cout << longestCommonSubstr(str1, str2, res) << endl;
    for(int i=0; i<res.size(); i++)
    {
        cout << res[i] << endl;
    }
    return 0;
}

```
