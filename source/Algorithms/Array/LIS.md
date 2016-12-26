# 最长递增子序列(LIS)算法

## 动态规划， $O(n^2)$
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

用ends数组存储，序列长度为len的最尾端元素。
1. 当新插入的元素比len-1长度的尾端元素大，则直接更新为ends[len];
2. 当新插入的元素比len-1长度的尾端元素小或等，则在ends中二分，找到哪个len_x对应的ends[len_x] >= val, 并将其更新为val.


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
