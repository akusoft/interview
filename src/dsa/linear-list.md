# 线性表

## 顺序表

顺序表分为**无序顺序表**和**有序顺序表**。

### 无序顺序表

### 有序顺序表

#### 去重

时间复杂度：

空间复杂度：

```c
int removeDuplicates(int* nums, int numsSize)
{
    if(numsSize == 0)
    {
        return 0;
    }
    int i = 0, j = 0;
    while(++j < numsSize)
    {
        if(nums[i] != nums[j])
        {
            nums[++i] = nums[j];
        }
    }
    return ++i
}
```

LeetCode：26

## 链表
