# 二分查找
[刷题链接](https://github.com/CyC2018/CS-Notes/blob/master/notes/Leetcode%20题解%20-%20二分查找.md)

常见 **左闭右闭**区间，初始值为0, nums.length - 1，while用小于等于
```java
public int binarySearch(int[] nums, int key) {
    int l = 0, h = nums.length - 1;
    while (l <= h) {
        int m = l + (h - l) / 2;
        if (nums[m] == key) {
            return m;
        } else if (nums[m] > key) {
            h = m - 1;
        } else {
            l = m + 1;
        }
    }
    return -1;
}
```

变种： 运用**左闭右开**，初始值为0, nums.length，while用小于. 在一个有重复元素的数组中查找 key 的最左位置， 
* h 的赋值表达式为 h = m
* 循环条件为 l < h
* 最后返回 l 而不是 -1

``` java
public int binarySearch(int[] nums, int key) {
    int l = 0, h = nums.length;
    while (l < h) {
        int m = l + (h - l) / 2;
        if (nums[m] >= key) {
            h = m;
        } else {
            l = m + 1;
        }
    }
    return l;
}
```

为了减少+1和-1的麻烦，运用一下模版，固定使用：
* l, h = 0, len(nums)-1
* while l + 1 < h:

``` python
# 找到第一个位置，收缩右边界
        l, h = 0, len(nums)-1
        while l + 1 < h:
            m = l + (h-l)//2
            # 关键点：等于时依然收缩右边界
            if nums[m] >= target:
                h = m
            else:
                l = m
        if nums[l] == target:
            left = l
        elif nums[h] == target:
            left = h
```

## 35 [搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)
给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
```python3
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        # 找到有几个数比target小，用二分模版 闭区间
        start, end = 0, len(nums) - 1
        while start + 1 < end:
            mid = start + (end - start) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                start = mid
            else:
                end = mid
        
        # 注意讨论越界情况
        if target <= nums[start]:
            return start
        elif target <= nums[end]:
            return end
        elif nums[end] < target:
            return end + 1
```
## 34. [在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
可以用mid模版写，记得收缩方向对，最后分类讨论即可
``` python3
class Solution:
    def searchRange(self, nums, target):
        if not nums:
            return [-1, -1]
        # 二分模版
        left, right = -1, -1
        # 找到第一个位置，收缩右边界
        l, h = 0, len(nums)-1
        while l + 1 < h:
            m = l + (h-l)//2
            # 关键点：等于时依然收缩右边界
            if nums[m] >= target:
                h = m
            else:
                l = m
        if nums[l] == target:
            left = l
        elif nums[h] == target:
            left = h

        # 找到最后一个位置，收缩左边界
        l, h = 0, len(nums)-1
        while l + 1 < h:
            m = l + (h-l)//2
            # 关键点：等于时依然收缩左边界
            if nums[m] <= target:
                l = m
            else:
                h = m
        if nums[h] == target:
            right = h
        elif nums[l] == target:
            right = l
        
        return [left, right]
```
也可以用二分查找常用+1或-1的模版，找出第一个位置和最后一个位置，但是寻找的方法有所不同，需要实现两个二分查找。我们将寻找 target 最后一个位置，转换成寻找 target+1 第一个位置，再往前移动一个位置。这样我们只需要实现一个二分查找代码即可。
``` java
public int[] searchRange(int[] nums, int target) {
    int first = findFirst(nums, target);
    int last = findFirst(nums, target + 1) - 1;
    if (first == nums.length || nums[first] != target) {
        return new int[]{-1, -1};
    } else {
        return new int[]{first, Math.max(first, last)};
    }
}

private int findFirst(int[] nums, int target) {
    int l = 0, h = nums.length; // 注意 h 的初始值
    while (l < h) {
        int m = l + (h - l) / 2;
        if (nums[m] >= target) {
            h = m;
        } else {
            l = m + 1;
        }
    }
    return l;
}
```
labuladong 统一解法
``` java
int left_bound(int[] nums, int target) { int left = 0, right = nums.length - 1; while (left <= right) {
int mid = left + (right - left) / 2; if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
right = mid - 1;
} else if (nums[mid] == target) {
// 别返回，锁定左侧边界
right = mid - 1; }
}
// 最后要检查 left 越界的情况
if (left >= nums.length || nums[left] != target)
return -1; return left;
}
int right_bound(int[] nums, int target) { int left = 0, right = nums.length - 1; while (left <= right) {
int mid = left + (right - left) / 2; if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
right = mid - 1;
} else if (nums[mid] == target) {
// 别返回，锁定右侧边界
            left = mid + 1;
        }
}
// 最后要检查 right 越界的情况
if (right < 0 || nums[right] != target)
return -1; return right;
}
```
## [山脉序列中的最大值](https://www.lintcode.com/problem/585)
给 n 个整数的山脉数组，即先增后减的序列，找到山顶（最大值）。
``` python
class Solution:
    """
    @param nums: a mountain sequence which increase firstly and then decrease
    @return: then mountain top
    """
    def mountainSequence(self, nums):
        # 找到最后一个使得 A[mid-1] < mid
        # 使用闭区间二分模版
        start, end = 0, len(nums)-1
        while start + 1 < end:
            mid = start + (end - start)//2
            if nums[mid-1] < nums[mid]:
                end = mid
            else:
                start = mid
        return max(nums[start], nums[end])
```
