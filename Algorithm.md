### BFS

##### BFS 使用场景

- 最短路径

- 分层遍历

- 拓扑排序

  - Course schedule I: 注意 如何new一个ArrayList数组

    ```java
    List<Integer>[] map = new ArrayList[];
    for (int i = 0; i < n; i++) {
      map[i] = new ArrayList<>(); //别忘了挨个初始化
    }
    ```

    

- 克隆graph

  - Leetcode 133 注意new一个新节点的时候要初始化neighbors 

    ```java
    // 没有后面的ArrayList 就是null 然后就add不进去 fuck！！
    new Node(val, new ArrayList<>());
    ```

---

##### Word Ladder I & II

* 出处

  [Word Ladder I](https://leetcode.com/problems/word-ladder/): 返回步数

  [Word Ladder II](https://leetcode.com/problems/word-ladder-ii/): 返回所有path

* Solution

  

* Complexity

* Code



### DFS

##### DFS 使用场景

- permutation(排列)
- combination(组合)
- 所有方案
  - 在递归函数中传递的List一直是同一个，这点不同于整数的传递。整数是值传递，object是reference传递。**所以在把path加到paths时要新建arraylist** 

##### 回溯思想

不要忘了递归前add 递归后remove

> 例题
>
> 1 combination sum系列
>
> (I) 每个元素可以选多次：要把输入数组去重
>
> (II) 每个元素用一次 数组中有重复 要求输出unique：注意 判断当前 和 前一个 一样 并且前一个没有选
>
> ```java
> if (i >= cur && arr[i] == arr[i - 1]) {
>   continue;
> }
> ```
>
> (IV) 每个元素可以选多次 无重复 但顺序matter：dfs超时 用DP 建立function f(x) = Sum{f(x - i)} i from 0 to n



### 分治法

**注意：从根到叶子的啥啥的，对于叶子的判断是**

```java
node.left == null && node.right == null
```



### 动态规划

##### 实现方式

* 循环(递推)

f[i] can be expressed by f[i - 1] or f[i - 1] + f[i - 2] 从而我们可以分解问题  用一个简单的for循环解决

* 记忆化搜索(递归)

当转移状态有点麻烦时，不是顺序型，考虑记忆化搜索

当初始化状态不是很容易找到，考虑记忆化搜索

【NOTE】滚动数组(可以优化空间复杂度 by 取模)

##### 使用场景

* 求最大值最小值
* 判断是否可行
* 统计方案个数

##### 不能使用动态规划的使用场景

* 求具体方案而不是方案个数的
* 输入数据是一个集合而不是序列
* 暴力解法已经是多项式级别的（动规擅长优化指数级别到多项式级别 e.g. n!/ 2^n -> n^2)

##### 四要素

* 状态 state
* 方程 function
* 初始化 initialization
* 答案 answer

### 高级数据结构

##### PriorityQueue

一般求最大K 最小K的时候用PriorityQueue

PriorityQueue 默认是最小堆

add - O（log n）

remove - O（log n）

Min or Max - O（1）



