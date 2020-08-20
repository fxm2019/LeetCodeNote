## 分治

### 引文

MapReduce（分治算法的应用） 是 Google 大数据处理的三驾马车之一，另外两个是 GFS 和 Bigtable。它在倒排索引、PageRank 计算、网页分析等搜索引擎相关的技术中都有大量的应用。

### 主要思想

分治算法的主要思想是将原问题**递归地分成**若干个子问题，直到子问题**满足边界条件**，停止递归。将子问题逐个击破(一般是同种方法)，将已经解决的子问题合并，最后，算法会**层层合并**得到原问题的答案。

### 分治算法的步骤

* 分：**递归地**将问题**分解**为各个的子**问题**(性质相同的、相互独立的子问题)；
* 治：将这些规模更小的子问题**逐个击破**；
* 合：将已解决的子问题**逐层合并**，最终得出原问题的解；

![](https://github.com/fxm2019/LeetCodeNote/blob/master/pics/20200408204450701.png)

### 分治法适用的情况

分治法所能解决的问题一般具有以下几个特征：

* 1) 该问题的规模缩小到一定的程度就可以容易地解决

* 2) 该问题可以分解为若干个规模较小的相同问题，即该问题具有最优子结构性质。

* 3) 利用该问题分解出的子问题的解可以合并为该问题的解；

* 4) 该问题所分解出的各个子问题是相互独立的，即子问题之间不包含公共的子子问题。

第一条特征是绝大多数问题都可以满足的，因为问题的计算复杂性一般是随着问题规模的增加而增加；

**第二条特征是应用分治法的前提**它也是大多数问题可以满足的，此特征反映了递归思想的应用；

**第三条特征是关键，能否利用分治法完全取决于问题是否具有第三条特征**，如果**具备了第一条和第二条特征，而不具备第三条特征，则可以考虑用贪心法或动态规划法**。

**第四条特征涉及到分治法的效率**，如果各子问题是不独立的则分治法要做许多不必要的工作，重复地解公共的子问题，此时虽然可用分治法，但**一般用动态规划法较好**。

### 伪代码

```python
def divide_conquer(problem, paraml, param2,...):
    # 不断切分的终止条件
    if problem is None:
        print_result
        return
    # 准备数据
    data=prepare_data(problem)
    # 将大问题拆分为小问题
    subproblems=split_problem(problem, data)
    # 处理小问题，得到子结果
    subresult1=self.divide_conquer(subproblems[0],p1,..…)
    subresult2=self.divide_conquer(subproblems[1],p1,...)
    subresult3=self.divide_conquer(subproblems[2],p1,.…)
    # 对子结果进行合并 得到最终结果
    result=process_result(subresult1, subresult2, subresult3,...)
```

### 算法应用

#### [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)

* 题目描述

  给定一个大小为 n 的数组，找到其中的众数。众数是指在数组中出现次数大于 [n/2] 的元素。

  你可以假设数组是非空的，并且给定的数组总是存在众数。

  示例 1:

  ```python
  输入: [3,2,3]
  输出: 3
  ```

  示例 2:

  ```python
  输入: [2,2,1,1,1,2,2]
  输出: 2
  ```

### 分治法：

* 解题思路

  * 确定切分的终止条件
  
    直到所有的子问题都是长度为 1 的数组，停止切分。
  
  * 准备数据，将大问题切分为小问题
  
    递归地将原数组二分为左区间与右区间，直到最终的数组只剩下一个元素，将其返回
  
  * 处理子问题得到子结果，并合并
  
    - 长度为 1 的子数组中唯一的数显然是众数，直接返回即可。
  
    - 如果它们的众数相同，那么显然这一段区间的众数是它们相同的值。
  
    - 如果他们的众数不同，比较两个众数在整个区间内出现的次数来决定该区间的众数

* 代码

  ```python
  class Solution(object):
      def majorityElement2(self, nums):
          """
          :type nums: List[int]
          :rtype: int
          """
          # 【不断切分的终止条件】
          if not nums:
              return None
          if len(nums) == 1:
              return nums[0]
          # 【准备数据，并将大问题拆分为小问题】
          left = self.majorityElement(nums[:len(nums)//2])
          right = self.majorityElement(nums[len(nums)//2:])
          # 【处理子问题，得到子结果】
          # 【对子结果进行合并 得到最终结果】
          if left == right:
              return left
          if nums.count(left) > nums.count(right):
              return left
          else:
              return right    
  ```
  

### 哈希表法

![image-20200818204007735](https://github.com/fxm2019/LeetCodeNote/blob/master/pics/image-20200818204007735.png)

```python
class Solution:
    def majorityElement(self, nums):
        counts = collections.Counter(nums)
            return max(counts.keys(), key=counts.get)

```

![image-20200818204303116](https://github.com/fxm2019/LeetCodeNote/blob/master/pics/image-20200818204303116.png)

### 排序

![image-20200818204359470](\pics\image-20200818204359470.png)

```python
class Solution:
    def majorityElement(self, nums):
        nums.sort()
        return nums[len(nums)//2]

```

#### [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

* 题目描述

  给定一个整数数组 `nums` ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

  示例:

  ```python
  输入: [-2,1,-3,4,-1,2,1,-5,4],
  输出: 6
  解释: 连续子数组 [4,-1,2,1] 的和最大为6。
  ```

* 解题思路

  * 确定切分的终止条件

    直到所有的子问题都是长度为 1 的数组，停止切分。

  * 准备数据，将大问题切分为小问题

    递归地将原数组二分为左区间与右区间，直到最终的数组只剩下一个元素，将其返回

  * 处理子问题得到子结果，并合并

    - 将数组切分为左右区间
      - 对与左区间：从右到左计算左边的最大子序和
      - 对与右区间：从左到右计算右边的最大子序和

    - 由于左右区间计算累加和的方向不一致，因此，左右区间直接合并相加之后就是整个区间的和
    - 最终返回左区间的元素、右区间的元素、以及整个区间(相对子问题)和的最大值

* 代码

  ```python
  class Solution(object):
      def maxSubArray(self, nums):
          """
          :type nums: List[int]
          :rtype: int
          """
          # 【确定不断切分的终止条件】
          n = len(nums)
          if n == 1:
              return nums[0]
  
          # 【准备数据，并将大问题拆分为小的问题】
          left = self.maxSubArray(nums[:len(nums)//2])
          right = self.maxSubArray(nums[len(nums)//2:])
  
          # 【处理小问题，得到子结果】
          #　从右到左计算左边的最大子序和
          max_l = nums[len(nums)//2 -1] # max_l为该数组的最右边的元素
          tmp = 0 # tmp用来记录连续子数组的和
          
          for i in range( len(nums)//2-1 , -1 , -1 ):# 从右到左遍历数组的元素
              tmp += nums[i]
              max_l = max(tmp ,max_l)
              
          # 从左到右计算右边的最大子序和
          max_r = nums[len(nums)//2]
          tmp = 0
          for i in range(len(nums)//2,len(nums)):
              tmp += nums[i]
              max_r = max(tmp,max_r)
              
          # 【对子结果进行合并 得到最终结果】
          # 返回三个中的最大值
          return max(left,right,max_l+ max_r)
  ```

  

#### [50. Pow(x, n)](https://leetcode-cn.com/problems/powx-n/)

* 题目描述

  实现 `pow(x, n) `，即计算 `x` 的 `n` 次幂函数。

  示例 1:

  ```
  输入: 2.00000, 10
  输出: 1024.00000
  ```

  示例 2:

  ```
  输入: 2.10000, 3
  输出: 9.26100
  ```

  示例 3:

  ```
  输入: 2.00000, -2
  输出: 0.25000
  解释: 2-2 = 1/22 = 1/4 = 0.25
  ```

  说明:

  `-100.0 < x < 100.0`
  `n `是 32 位有符号整数，其数值范围是$[−2^{31}, 2^{31} − 1] $。

* 解题思路

  * 确定切分的终止条件

    对`n`不断除以2，并更新`n`，直到为0，终止切分

  * 准备数据，将大问题切分为小问题

    对`n`不断除以2，更新

  * 处理子问题得到子结果，并合并

    * `x`与自身相乘更新`x`
    * 如果`n%2 ==1`
      - 将`p`乘以`x`之后赋值给`p`(初始值为1)，返回`p`
  * 最终返回`p`

* 代码

  ```python
  class Solution(object):
      def myPow(self, x, n):
          """
          :type x: float
          :type n: int
          :rtype: float
          """
          # 分治 
          # 处理n为负的情况
          if n < 0 :
              x = 1/x
              n = -n
          # 【确定不断切分的终止条件】
          if n == 0 :
              return 1
  
          # 【准备数据，并将大问题拆分为小的问题】
          if n%2 ==1:
            # 【处理小问题，得到子结果】
            p = x * self.myPow(x,n-1)# 【对子结果进行合并 得到最终结果】
            return p
          return self.myPow(x*x,n/2)
  ```



### 参考资料

[五大常用算法之一：分治算法](https://www.cnblogs.com/steven_oyj/archive/2010/05/22/1741370.html)

[你不知道的 Python 分治算法](https://gitchat.csdn.net/activity/5d39b22ba2a28a54fc0090f7?utm_source=so#1?utm_source=so&utm_source=so)



