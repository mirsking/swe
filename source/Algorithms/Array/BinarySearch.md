# 二分查找

## 注意细节
1. 边界问题
2. 数组很大时(left+right)/2越界的问题。

注意：以下代码需要大概8G的内存，内存过小的请勿尝试。
```cpp
int binarySearch(int* array, int length, int val)
{
    int left = 0;
    int right = length;

    while (left < right) // Notice 1: [left, right)
    {
        int mid = left + (right-left)/2; // Notice 4: avoid (left+right)/2 overflow
        int mid_val = *(array + mid);
        if(mid_val == val)
            return mid;

        if (mid_val < val)
            left = mid + 1; //Notice 2: left = next mid
        else
            right = mid; // Notice 3: right = mid
    }

    return -1;
}

int main()
{
    int* nums = new int[INT_MAX];
    for (int i = 0; i < INT_MAX; i++)
        nums[i] = i;
    int index = binarySearch(nums, INT_MAX, INT_MAX - 1);
    cout << index << " " << nums[index] << endl;
    delete[] nums;
    return 0;
}
```
