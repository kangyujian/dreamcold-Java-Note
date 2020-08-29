1. 删除数组中的重复项

[题目地址](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

给定一个排序数组，你需要在 原地 删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

 
```
示例 1:

给定数组 nums = [1,1,2], 

函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 

你不需要考虑数组中超出新长度后面的元素。
示例 2:

给定 nums = [0,0,1,1,1,2,2,3,3,4],

函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。

你不需要考虑数组中超出新长度后面的元素。

```

**思路**

- index维护着指向新的不重复数组的末尾位置
- 使用下标i遍历整个有序数组，因为数组是有序的，所以一旦i指向的元素值和index指向的元素值不一样的时候，就可以将不重复的元素加入到不重复的数组也就是++index
- 最后要返回的是整个数组的长度，index指向的是不重复数组末尾的位置，整个新的不重复数组的长度是index+1


```java
class Solution {
    public int removeDuplicates(int[] nums) {
        if(nums==null||nums.length==0){
            return 0;
        }
        int index=0;
        for(int i=0;i<nums.length;i++){
            if(nums[i]!=nums[index]){
                nums[++index]=nums[i];
            }
        }
        return index+1;
        
    }
}
```


2. 买卖股票的最佳时机 II

[题目地址](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

```
示例 1:

输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
示例 2:

输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
示例 3:

输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。

```

**思路**

本题是连续的进行交易并获得最大的利润，遵循着低买高卖的原则，那么只要前一天的价格比后一天低的话，那么就可以获得利润，我们可以将这些利润进行加和，得到最终的结果。


```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length<2){
            return 0;
        }
        int max=0;
        for(int i=1;i<prices.length;i++){
            int temp=prices[i]-prices[i-1];
            if(temp>0)
                max+=temp;
        }
        return max;
    }
}
```

3. 旋转数组

[题目地址](https://leetcode-cn.com/problems/rotate-array/)

给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。

```
示例 1:

输入: [1,2,3,4,5,6,7] 和 k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]
示例 2:

输入: [-1,-100,3,99] 和 k = 2
输出: [3,99,-1,-100]
解释: 
向右旋转 1 步: [99,-1,-100,3]
向右旋转 2 步: [3,99,-1,-100]
说明:

尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
要求使用空间复杂度为 O(1) 的 原地 算法。
```

**思路**
1. 采用额外的数组空间实现取余操作
```java
class Solution {
    public void rotate(int[] nums, int k) {
        int[] newnums=new int[nums.length];
        for(int i=0;i<nums.length;i++){
            newnums[(i+k)%nums.length]=nums[i];
        }
        for(int i=0;i<nums.length;i++){
            nums[i]=newnums[i];
        }
    }
}
```

2. 模拟循环后移，将数组的最后一个元素放到数组第一个元素的位置，数组中其他元素都向后移动一位，整个过程相当于将整个数组循环移动一次，由于我们需要将整个数组移动k次，所以上述的过程要进行k次，由于我们仅仅需要移动k的余数次就可以，移动多个圈是无意义的，所以取余。


```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n=nums.length;
        k%=n;

        for(int i=0;i<k;i++){
            int temp=nums[n-1];
            for(int j=n-1;j>0;j--){
                nums[j]=nums[j-1];
            }
            nums[0]=temp;
        }

    }
}
```

4. 存在重复元素

[题目地址](https://leetcode-cn.com/problems/contains-duplicate/)

给定一个整数数组，判断是否存在重复元素。

如果任意一值在数组中出现至少两次，函数返回 true 。如果数组中每个元素都不相同，则返回 false 。

```

示例 1:

输入: [1,2,3,1]
输出: true
示例 2:

输入: [1,2,3,4]
输出: false
示例 3:

输入: [1,1,1,3,3,4,3,2,4,2]
输出: true

```

**思路**

1. 哈希表统计法

使用哈希表来统计每个元素出现的次数，一旦某个值的出现次数超过了两次的情况下，那么就将该元素提出来。

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Map<Integer,Integer> map=new HashMap<>();
        for(int i=0;i<nums.length;i++){
            if(map.getOrDefault(nums[i],0)>=1){
                return true;
            }
            map.put(nums[i],map.getOrDefault(nums[i],0)+1);
        }
        return false;
    }
}
```

2. 排序法

一个排序好的数组，如果有重复元素，那么重复元素必相邻。

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        if(nums.length<2){
            return false;
        }
        Arrays.sort(nums);
        for(int i=0;i<nums.length-1;i++){
            if(nums[i]==nums[i+1]) 
            return true;
        }
        return false;
    }
}
```

5. 只出现一次的数字

[题目地址](https://leetcode-cn.com/problems/single-number/)

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

```
示例 1:

输入: [2,2,1]
输出: 1
示例 2:

输入: [4,1,2,1,2]
输出: 4
``` 

**思路**

这道题其实按照之前的方法如哈希表、排序也可以做出来，但是不是最优。最优的方法是采用异或的性质。首先我们需要知道异或的性质：

异或的真值表

|p|q|p^q|
|--|--|--|
|0|0|0|
|0|1|1|
|1|0|1|
|1|1|0|

说白点，就是两布尔值，两者不一样那么就异或为1，反之就是异或为0.异或运算具有如下的性质

1. 任何数和0做异或结果仍然是原来的数
2. 任何数和自身做异或运算结果都是0
3. 异或运算满足交换律和结合律：也就是a^b^a=a^a^b=b^a^a=0^b=b

根据上面的性质，对于一个数组{a1,a2,a3,...,a4}

我们初始化res=0,然后依次与数组中的每一个元素进行异或，那么相当于

```
0^a1^a2^a3^....^an
```
看起来有些太抽象了，那么具体化，假如是这样的式子

```
0^1^2^2^1^6^7^6
```

由于满足交换律

```
0^(2^2)^(1^1)^(6^6)^(7)
=0^0^0^0^7
=7
```

通过上面的式子，我们就可以求出仅仅出现一次的元素

```java
class Solution {
    public int singleNumber(int[] nums) {
        int res=0;
        for(int num:nums){
            res^=num;
        }
        return res;
    }
}
```