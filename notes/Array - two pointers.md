# 数组系列 - 双指针法
双指针法通常是指运用多个指针，辅助记录数据位置，以方便赋值、内部交换等操作。

我把双指针分为：
- 前后指针：一个指针从前往后，一个指针从后往前
- 快慢指针：两个指针同向，但一个移动的快，一个移动的慢(条件达成才移动)
- 间隔指针：两个指针一直保持一个不变的间距
- 双指针：只是用了多个指针，但指针之间并无什么关系

前后指针相关练习：
* [905. Sort Array By Parity](#Sort_Array_By_Parity)

快慢指针相关练习：

* [283. Move Zeroes](#Move_Zeroes)


# Sort_Array_By_Parity

**问题描述**

Given an array A of non-negative integers, return an array consisting of all the even elements of A, followed by all the odd elements of A.
<br/>
You may return any answer array that satisfies this condition.

对一个数组重新排序，将其中的所有偶数元素全部移动到基数元素前面（不要求稳定）。

**Example 1:**
> Input: [3,1,2,4]

> Output: [2,4,3,1]

> The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.

**Note:**
> 1 <= A.length <= 5000

> 0 <= A[i] <= 5000

<br/>

### 解法1：双指针复制元素，有点像链表合并
```java
public int[] sortArrayByParity(int[] A) {
    int l = 0, h = A.length - 1;
    int[] B = new int[A.length];
    
    for (int i = 0; i < A.length; i++) {
        if (A[i] % 2 == 0)
            B[l++] = A[i];
        else
            B[h--] = A[i];
    }
    return B;
}
```
缺点：浪费空间&无法保证元素的初始顺序
<br/>
如果想保证稳定性，可以利用两次for循环，一次只取偶数，一次只取基数
<br/><br/>

### 解法2：前后(双)指针交换元素，可以对比快速排序
从前往后找一个基数，从后往前找一个偶数，然后交换。
```java
public int[] sortArrayByParity(int[] A) {
    int l = 0, h = A.length - 1;
    
    while (l < h) {
        while (A[l] % 2 == 0 && l < h) l++;
        while (A[h] % 2 == 1 && l < h) h--;
        if (l < h) {
            A[l] ^= A[h];
            A[h] ^= A[l];
            A[l] ^= A[h];
        }
    }
    return A;
}
```
优点：节省空间；缺点：无法保证元素的初始顺序
<br/><br/><br/>

# Move_Zeroes

**问题描述**

Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

**Example:**
> Input: [0,1,0,3,12]

> Output: [1,3,12,0,0]

**Note:**

> You must do this in-place without making a copy of the array.

> Minimize the total number of operations.

<br/>

### 解法1：快慢(双)指针交换元素
```java
public void moveZeroes(int[] nums) {
    int l = nums.length;
      
    int slow = 0;
    for (int i = 0; i < l; i++)
        if (nums[i] != 0)
            nums[slow++] = nums[i];
    for (int i = slow; i < l; i++)
        nums[i] = 0;
}
```

