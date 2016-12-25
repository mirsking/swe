# 最长递增子序列

## 动态规划，时间复杂度$O(n^2)$

```cpp
int lengthOfLIS(vector<int>& nums) {
    int n = nums.size();
    vector<int> dp(n, 1);
    for(int i=1; i<n; i++)
    {
        for(int j=0; j<i; j++)
        {
            if(nums[i] > nums[j])
                dp[i] = max(dp[i], dp[j]+1);
        }

    }

    int max_len =0;
    for(int i=0; i<n; i++)
        max_len = max(max_len, dp[i]);
    return max_len;
}
```
## $O(nlogn)$
参考博客[经典动态规划问题：最长递增子序列(LIS)](http://www.neozone.me/longest-increasing-subsequence.html)
```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        if(n==0)
            return 0;
        vector<int> ends(n);
        int len = 1;
        ends[0] = nums[0];
        for(int i=1; i<n; i++)
        {
            int val = nums[i];
            if(val > ends[len-1])
            {
                ends[len] = val;
                len++;
            }
            else
            {
                int pos = findInsertPos(ends, len, val);
                ends[pos] = val;

            }

        }
        return len;
    }
private:
    int findInsertPos(const vector<int>& input, int n, int val)
    {
        int l = 0, r = n-1;
        while(l<=r)
        {
            int m = (r + l) / 2;
            int mid = input[m];
            if(mid < val)
                l = m+1;
            else if (mid > val)
                r = m-1;
            else
                return m;
        }
        return l;

    }
};
```
