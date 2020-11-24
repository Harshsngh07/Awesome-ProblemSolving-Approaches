For more detailed analysis of each them, please check
https://leetcode.com/articles/introduction-to-binary-search/
NOTE: My implementation is slightly different than the ones used in that article, and I personally prefer the third one in general.
### Classic Binary Search:

```class Solution {
    // template 1
    public int search(int[] nums, int target) {
         if (nums == null || nums.length == 0)  return -1;
         int start = 0, end = nums.length - 1;
         while (start <= end) {
             int mid = start + (end - start) / 2;
             if (nums[mid] == target)   return mid;
             else if (nums[mid] < target)   start = mid + 1;
             else   end = mid - 1;
         }
         return -1;
    }
    // template 2
    public int search(int[] nums, int target) {
        if (nums == null || nums.length == 0)  return -1;
        int start = 0, end = nums.length - 1;
        while (start < end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] < target) start = mid + 1;
            else    end = mid;
        }
        if (nums[start] == target)   return start;
        return -1;
    }
    // template 3
    public int search(int[] nums, int target) {
        if (nums == null || nums.length == 0)  return -1;
        int start = 0, end = nums.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] < target) start = mid;
            else    end = mid;
        }
        if (nums[start] == target)  return start;
        if (nums[end] == target)    return end;
        return -1;
    }
}
Find last position of target using template 3:

class Solution {
    public int search(int[] nums, int target) {
        if (nums == null || nums.length == 0)   return -1;
        int start = 0, end = nums.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] == target)    start = mid;
            else if (nums[mid] < target)    start = mid + 1;
            else    end = mid - 1;
        }
        if (nums[end] == target)    return end;
        if (nums[start] == target)  return start;
        return -1;
    }
}
Find first position of target using template 3:

class Solution {
    public int search(int[] nums, int target) {
        if (nums == null || nums.length == 0)   return -1;
        int start = 0, end = nums.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] == target)    end = mid;
            else if (nums[mid] > target)    end = mid - 1;
            else    start = mid + 1;
        }
        if (nums[start] == target)  return start;
        if (nums[end] == target)    return end;
        return -1;
    }
}

```

<hr/>

Template 1
Find target
```
public int binarySearch(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (target > nums[mid]) left = mid + 1;
        else if (target < nums[mid]) right = mid - 1;
        else if (target == nums[mid]) return mid;
    }
    return -1;
}
```

Template 2
Find first occurrence of the target

```
public int binarySearch(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (target > nums[mid]) left = mid + 1;
        else if (target < nums[mid]) right = mid - 1;
        else if (target == nums[mid]) right = mid - 1;
    }
    if (left >= nums.length || nums[left] != target) return -1;
    return left;                                                
}

```
Template 3
Find last occurrence of the target

```
public int binarySearch(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (target > nums[mid]) left = mid + 1;
        else if (target < nums[mid]) right = mid - 1;
        else if (target == nums[mid]) left = mid + 1;
    }
    if (right < 0 || nums[right] != target) return -1;
    return right;                                                
}
```