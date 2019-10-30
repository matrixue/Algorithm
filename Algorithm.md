



### Non算法

##### Integer to English Words

- 出处

  https://leetcode.com/problems/integer-to-english-words/ 

  输入一个int范围内的整数，输出翻译成英文（不用and）

- Solution

  设计一个helper函数来处理1000以内的各种情况，在一个while循环里不断调用helper 再在尾部加上thousand, million 之类的 加以组合。难点 别忘记处理11-19；尾部空格；零的情况

- Code

  ```java
  class Solution {
      String[] num = new String[] {"Dummy", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine"};
      String[] num2 = new String[] {"Dummy", "Ten", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};
      String[] num3 = new String[] {"Dummy", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen", };
      
      public String numberToWords(int num) {
          if (num == 0) {
              return "Zero";
          }
          String[] lib = new String[] {"", "Thousand ", "Million ", "Billion "};
          String res = "";
          int round = 0;
          
          while (num != 0) {
              String s = helper(num % 1000);
              if (s.length() != 0) {
                  res = s + lib[round] + res;    
              }
              num = num / 1000;
              round++;
          }
          
          return res.substring(0, res.length() - 1);
      }
      
      private String helper(int n) {
          String s = "";
          if (n >= 100) {
              s += num[n / 100] + " " + "Hundred" + " ";
              n %= 100;
          }
          if (n >= 20 || n <= 10) {
              if (n >= 10) {
                  s += num2[n / 10] + " ";    
                  n %= 10;
              }
              if (n != 0) {
                  s += num[n] + " ";    
              }
          } else {
              s += num3[n - 10] + " ";
          }
          return s;
      }
  }
  ```

  

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

##### Word Ladder I

* 出处

  [Word Ladder I](https://leetcode.com/problems/word-ladder/): 返回步数

* Solution

  

* Complexity

* Code

##### Word Ladder II

- 出处

  [Word Ladder II](https://leetcode.com/problems/word-ladder-ii/): 返回所有path

- Solution

  BFS + DFS

  先用BFS （end 到 start）获得每个单词到终点的最近距离 并 获得每个单词的next（HashMap）。bfs进入主体之前 先遍历输入list 初始化map为（new HashMap<>()）注意获得当前单词的下一个时 先用一个helper函数 用于loop from a to z 然后判断是不是在map里 来过滤不在字典的。最容易错的是添加到map 和添加到distance的时机不同。 对于distance 只要访问过 就不要二次更新（保证最短距离）而map，每两个单词都要建立连接，形成网络

- Code

```java
class Solution {
    public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {
        List<List<String>> res = new ArrayList<>();
        Map<String, Integer> distance = new HashMap<>();
        Map<String, List<String>> map = new HashMap<>();
        
        // bfs end -> start
        bfs(endWord, beginWord, map, distance, wordList);
        
        // System.out.println(map.get("hot"));
        
        // dfs start -> end
        List<String> level = new ArrayList<>();
        dfs(beginWord, endWord, map, distance, level, res);
        
        return res;
    }
    
    private void dfs(String start, String end, Map<String, List<String>> map, Map<String, Integer> distance, List<String> level, List<List<String>> res) {
        level.add(start);
        if (start.equals(end)) {
            res.add(new ArrayList<String>(level));
        } else {
            for (String next: map.get(start)) {
                if (distance.containsKey(next) && distance.get(next) + 1 == distance.get(start)) {
                    dfs(next, end, map, distance, level, res);
                }
            }           
        }
        level.remove(start);
    }
    
    private List<String> findNext(String s) {
        List<String> res = new ArrayList<>();
        
        for (int j = 0; j < s.length(); j++) {
            for (char i = 'a'; i <= 'z'; i++) {
                if (s.charAt(j) == i) {
                    continue;
                }
                String next = s.substring(0, j) + Character.toString(i) + s.substring(j + 1);
                res.add(next);
            }
        }
        return res;
    }
    
    
    private void bfs(String start, String end, Map<String, List<String>> map, Map<String, Integer> distance, List<String> wordList) {
        Queue<String> q = new LinkedList<>();
        q.offer(start);
        distance.put(start, 0);
        for (String word: wordList) {
            map.put(word, new ArrayList<String>());
        }
        map.put(end, new ArrayList<String>());
        if (!map.containsKey(start)) {
            return;
        }
        
        while (!q.isEmpty()) {
            String cur = q.poll();
            for (String next: findNext(cur)) {
                if (!map.containsKey(next)) {
                    continue;
                }
                map.get(next).add(cur);
                if (distance.containsKey(next)) {
                    continue;
                }
                distance.put(next, distance.get(cur) + 1);
                q.offer(next);
            }
        }
    }
}
```

##### Alien Dictionary 

- 出处

  https://leetcode.com/problems/alien-dictionary/

  输入是一个根据外星顺序sort的一个String Array

  输出这个外星顺序

- Solution

  topological sort：

  1. build edges
  2. build indegree（建议跟1分开 直接根据输入数组得到indegree会有重复）
  3. BFS
  4. 若res 长度跟总单词个数不等，说明不存在

- Code

  ```java
  class Solution {
      public String alienOrder(String[] words) {
          Map<Character, Integer> indegree = new HashMap<>();
          Map<Character, Set<Character>> edges = new HashMap<>();
  
          for (int i = 0; i < words.length; i++) {
              for (int j = 0; j < words[i].length(); j++) {
                  if (!edges.containsKey(words[i].charAt(j))) {
                      edges.put(words[i].charAt(j), new HashSet<>());
                  }
              }
          }
          
          for(int i = 0; i < words.length - 1; i++) {
              String first = words[i];
              String second = words[i + 1]; 
              int index = 0;
              while (index < first.length() && index < second.length() && first.charAt(index) == second.charAt(index)) {
                  index++;
              }
              if (index >= first.length() || index >= second.length()) {
                  continue;
              }
              char fc = first.charAt(index);
              char sc = second.charAt(index);
              edges.get(fc).add(sc);
          }
          
          for (Character c: edges.keySet()) {
              indegree.put(c, 0);
          }
          
          for (Character c: edges.keySet()) {
              for (Character next: edges.get(c)) {
                  indegree.put(next, indegree.get(next) + 1);    
              }
              
          }
          
          Queue<Character> q = new LinkedList<>();
          String res = "";
          for (Character c: indegree.keySet()) {
              if (indegree.get(c) == 0) {
                  q.offer(c);
                  res += Character.toString(c);
              }
          }
          
          while (!q.isEmpty()) {
              char cur = q.poll();
              for (Character next: edges.get(cur)) {
                  indegree.put(next, indegree.get(next) - 1);
                  if (indegree.get(next) == 0) {
                      q.offer(next);
                      res += Character.toString(next);
                  }
              }
          }
          if (indegree.size() != res.length()) {
              return "";
          }
          return res;
      }
  }
  ```

  

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

___

##### Word break

* 出处

  https://leetcode.com/problems/word-break/solution/

  输入是一个String 和一个List 问能不能用List的String组成String，可以重复使用

* Solution

  DFS + memorial 

* Complexity 

  Time: O(2^n)

  Space: O(n)

* Code

  ```java
  class Solution {
      public boolean wordBreak(String s, List<String> wordDict) {
          Set<String> set = new HashSet<>(wordDict);
          Map<String, Boolean> map = new HashMap<>();
          return wordBreakHelper(s, set, map);
      }
      
      private boolean wordBreakHelper(String s, Set<String> set, Map<String, Boolean> map) {
          if (s.equals("")) {
              return true;
          }
          if (map.containsKey(s)) {
              return map.get(s);
          }
          for (int i = 1; i <= s.length(); i++) {
              if (set.contains(s.substring(0, i))) {
                  if (wordBreakHelper(s.substring(i), set, map)) {
                      map.put(s, true);
                      return true;
                  }
              }
          }
          map.put(s, false);
          return false;
      }
  }
  ```

##### Word break II

- 出处

  https://leetcode.com/problems/word-break-ii/

  输入是一个String 和一个List，可以重复使用

  求所有方案

- Solution

  DFS + memorial 

- Complexity 

  Time: O(2^n)

  Space: O(n^2)

- Code

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

##### Find Median from Data Stream

- 出处

  https://leetcode.com/problems/find-median-from-data-stream/

  实时 实现俩功能：添加新数；返回当前median（有序）

- Solution

  Two PriorityQueue

  分析：比较容易想到的是每次添加新数，重新sort，然后根据index可以找到median。可以优化的方向是sort本身unnecessary 我们只需要知道中间点 而PriorityQueue可以告诉我们最大/最小 我们可以想到维护两个PriorityQueue：分别存左边一半（MaxHeap） 右边一半（MinHeap）。那么每次我们添加新数要考虑如何keep balance。当奇数个时 左边一半多一个；偶数是一样多。

  e.g. 考虑某中间状态：left：n + 1 right：n，先加到左边，左边变成n + 2 然后poll一个左边add到右边；left：n right：n，先加到左边，此时个数满足要求 但是可能左边有比右边大的，此时应该比较两边的peek 如果不符合要求则交换

- Complexity 

  Time: O(2^n)

  Space: O(n^2)

- Code

  ```java
  class MedianFinder {
      PriorityQueue<Integer> left;
      PriorityQueue<Integer> right;
      int size;
  
      /** initialize your data structure here. */
      public MedianFinder() {
          this.left = new PriorityQueue<>(100, Collections.reverseOrder());
          this.right = new PriorityQueue<>();
          this.size = 0;
      }
      
      public void addNum(int num) {
          size++;
          left.add(num);
          if (size % 2 == 0) {
              right.add(left.poll());
          } else {
              System.out.println(4);
              if (right.size() > 0 && left.peek() > right.peek()) {
                  int big = left.poll();
                  int small = right.poll();
                  left.add(small);
                  right.add(big);
              }
          }
      }
      
      public double findMedian() {
          if (size % 2 == 0) {
              return (double)(left.peek() + right.peek()) / 2;
          } else {
              return (double)left.peek();
          }
      }
  }
  ```

  



