## 最长公共子序列

```cpp
//============================================================================
// Name        : LongestCommonSubseq.cpp
// Author      : mirsking
// Version     :
// Copyright   :
// Description : Hello World in C++, Ansi-style
//============================================================================

#include <iostream>
#include <string>
#include <vector>
using namespace std;

enum DIRECT{
    LEFT_UP,
    LEFT,
    UP
};

struct Pack{
    int len;
    int direct;
    Pack():len(0), direct(LEFT_UP){}
};

int LongestCommonSubseq(const string& lhs, const string& rhs, string& res)
{
    size_t m = lhs.size();
    size_t n = rhs.size();
    vector<vector<Pack> > dp(m+1, vector<Pack>(n+1));

    for(int i=0; i<m; i++)
    {
        for(int j=0; j<n; j++)
        {
            int len1 = dp[i+1][j].len; //LEFT
            int len2 = dp[i][j+1].len; //UP
            int len3 = dp[i][j].len; //LEFT_UP
            if(lhs[i]==rhs[j])
            {
                len3 += 1;
            }

            int max_len = 0;
            if(len1 >= len2)
            {
                max_len = len1;
                dp[i+1][j+1].direct = LEFT;
            }
            else
            {
                max_len = len2;
                dp[i+1][j+1].direct = UP;
            }

            if(max_len <= len3)
            {
                max_len = len3;
                dp[i+1][j+1].direct = LEFT_UP;
            }

            dp[i+1][j+1].len = max_len;
        }
    }

#ifdef DEBUG
    for(size_t i=0; i<=m; i++)
    {
        for(size_t j=0; j<=n; j++)
        {
            cout << dp[i][j].len;
            switch(dp[i][j].direct)
            {
            case LEFT_UP: cout << "g";break;
            case LEFT: cout << "l";break;
            case UP: cout <<"u"; break;
            }
            cout << " ";
        }
        cout << endl;
    }
#endif

    string tmp;
    int i=m;
    int j=n;
    while(i>0 && j>0)
    {
        switch(dp[i][j].direct)
        {
        case LEFT_UP:
            if(dp[i][j].len == dp[i-1][j-1].len + 1)
                tmp.push_back(lhs[i-1]);
            i--;
            j--;
            break;
        case LEFT:
            j--;
            break;
        case UP:
            i--;
            break;
        }
    }

    for(int i=tmp.size()-1; i>=0; i--)
    {
        res.push_back(tmp[i]);
    }

    return dp[m][n].len;
}


int main() {
    string str1 = "abcdfff";
    string str2 = "ddadbecede";
    string res;
    cout << LongestCommonSubseq(str1, str2, res) << endl;
    cout << res << endl;
    return 0;
}

```
