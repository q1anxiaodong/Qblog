---
title: 删除有序数组中的k个重复项
date: 2024-03-21 11:16:06
tags: ['leetcode']
categories: ['算法']
---
# 题目描述

[面试经典150题-26](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/description/?envType=study-plan-v2&envId=top-interview-150)

# 解题思路

**快慢指针**。快指针用来遍历原来的数组，慢指针用来记录删除后的数组长度。首先顺序滑动快指针并判断两指针对应的值是否重复：
- 如果重复，继续向右划。重复值都是要删掉的不用管。
- 如果不重复，则将快指针的值记录到慢指针的**下一位**[1]，然后慢指针右移一位。

根据上面判断分支循环遍历，直到快指针划到数组最后一位。

## 可以优化的点
1. 在每次找到非重复值的时候，如果快指针就在慢指针右边一位，那其实慢指针要记录的值已经在对应的位置了，所以不用继续赋值。因此可以在找到非重复值的时候判断快指针`fast`-慢指针`slow`是否大于1，如果等于1，则不用再赋值。
2. 当数组原本长度小于2时，不可能出现重复情况，所以直接返回原数组。

# 题解
大佬们的题解：
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int n = nums.length;
        int j = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] != nums[j]) {
                nums[++j] = nums[i];
            }
        }
        return j + 1;
    }
}
```
- 时间复杂度：O(n)
- 空间复杂度：O(1)

我的题解：
```typescript
function removeDuplicates(nums: number[]): number {
    if (nums.length < 2) {
        return nums.length;
    }

    let i = 0, j = 1;
    for(;j < nums.length; j++) {
        if (nums[i] !== nums[j]) {
            i += 1;
            nums[i] = nums[j]
        }
    }
    return i + 1;
};
```
- 时间复杂度：O(n)
- 空间复杂度：O(1)

# 扩展延伸：删除重复项时保留k个重复数字

[扩展题目](https://leetcode.cn/problems/remove-duplicates-from-sorted-array-ii/description/?envType=study-plan-v2&envId=top-interview-150)

### 思路
原本的题目相当于保留1个重复数据，现在需要保留k个重复数据时，只需要在找到非重复值的时候，将其记录在慢指针的**后k位**就可以(因为整个数组是非严格递增的)，最后在返回数组长度时别忘了**加上k**，这个大家想想就能明白。

### 题解
```typescript
function removeDuplicates(nums: number[]): number {
    if (nums.length < 3) {
        return nums.length;
    }

    let i = 0, j = 2, repeat = 0;
    for(; j < nums.length; j++) {
        if (nums[j] !== nums[i]) {
            nums[i + 2] = nums[j];
            i++;
        }
    }
    return i + 2;
};
```