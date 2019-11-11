



### Non算法

##### sort a almost sorted list by swap two elements once

```java
int first = -1, second = -1;
for (int i = 1; i < arr.length; i++) {
  if (arr[i] < arr[i - 1]) {
    if (first == -1) { // first disorder, take former one
      first = i - 1;
      second = i;
    } else { // second disorder, take later one
      second = i;
    }
  }
}
swap(first, second);
```



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

### Binary Search

##### Max Sum of Rectangle No Larger Than K

- 出处

  https://leetcode.com/problems/max-sum-of-rectangle-no-larger-than-k/

  输入一个int二维数组代表矩形 和一个k 找到小于k的最大矩形面积

- Solution

  用TreeSet 因为可以帮我在logn的复杂度下找到大于xxx的最小值

  set init后要添加0

- Complexity

  - Time: O(n * n  *m  * log m)
  - Space O(m)

- Code

  ```java
  class Solution {
      public int maxSumSubmatrix(int[][] matrix, int k) {
          int res = Integer.MIN_VALUE;
          
          if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
              return res;
          }
          
          int r = matrix.length;
          int c = matrix[0].length;
          for (int i = 0; i < c; i++) {
              int[] row = new int[r];
              for (int j = i; j < c; j++) {
                  for (int h = 0; h < r; h++) {
                      row[h] += matrix[h][j];
                  }
                  
                  int accu = 0;
                  TreeSet<Integer> set = new TreeSet<>();
                  set.add(0);
                  for (int h = 0; h < r; h++) {
                      accu += row[h];
                      Integer minus = set.ceiling(accu - k);
                      if (minus != null) {
                          res = Math.max(res, accu - minus);    
                      }
                      set.add(accu);
                  }
              }
          }
          
          return res;
      }
  }
  ```

  

### 链表

##### Remove Linked List Elements

- 出处

  https://leetcode.com/problems/remove-linked-list-elements/

- Solution

  注意 （1）判断等 用while 因为可能有连续的等 （2）init dmmy.next

- Code

  ```java
  class Solution {
      public ListNode removeElements(ListNode head, int val) {
          ListNode dmmy = new ListNode(-1);
          ListNode prev = dmmy;
          prev.next = head; // 注意
          
          while (head != null) {
              while (head != null && head.val == val) { // 注意
                  prev.next = head.next;
                  head = head.next;
              }
              if (head != null) {
                  prev = head;
                  head = head.next;                
              }
          }
          
          return dmmy.next;
      }
  }
  ```

### HashMap

##### Degree of an Array

- 出处

  https://leetcode.com/problems/degree-of-an-array/

- Solution

  非常有意思的一道题，不是说题多难 而是正确答案多么精妙,  利用三个HashMap一次循环得到每个值出现的频率 每个值第一次出现的index 和最后一次出现的index

- Complexity

  TIme: O(n)

  Space: O(n)

- Code

  ```java
  class Solution {
      public int findShortestSubArray(int[] nums) {
          Map<Integer, Integer> left = new HashMap<>(), right = new HashMap<>(), count = new HashMap<>();
          
          for (int i = 0; i < nums.length; i++) {
              int x = nums[i];
              if (!left.containsKey(x)) {
                  left.put(x, i);
              }
              right.put(x, i);
              count.put(x, count.getOrDefault(x, 0) + 1);
          }
          
          int res = Integer.MAX_VALUE;
          int max = Collections.max(count.values());
          for (int x: count.keySet()) {
              if (count.get(x) == max) {
                  res = Math.min(res, right.get(x) - left.get(x) + 1);
              }
          }
          return res;
      }
  }
  ```

### Stack

##### 132 Pattern

* 出处

  https://leetcode.com/problems/132-pattern/

* Solution

  有点像单调栈 132 pattern 可以分解为要找 1 < 3 && 3 > 2 && 1 < 2

  其中『3』我们iterate 『1』用mins数组 『2』用stack。非常巧妙。

* Complexity

  TIme: O(n)

  Space: O(n)

* Code

  ```java
  class Solution {
      public boolean find132pattern(int[] nums) {
          Deque<Integer> stack = new ArrayDeque<>();
          int[] mins = new int[nums.length];
          for (int i = 0; i < nums.length; i++) {
              mins[i] = i == 0 ? nums[i] : Math.min(mins[i - 1], nums[i]);
          }
          for (int i = nums.length - 1; i >= 0; i--) {
              while (stack.size() > 0 && mins[i] >= stack.peek()) {
                  stack.pop();
              }
              if (nums[i] > mins[i] && stack.size() > 0 && nums[i] > stack.peek()) {
                  return true;
              }
              
              stack.push(nums[i]);
          }
          return false;
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

#####  Snakes and Ladders

- 出处

  https://leetcode.com/problems/snakes-and-ladders/

  输入:n x n的board  问多少步可以从1走到n*n 

- Solution

  1. 注意 这题坐标变换问题. 单独搞一个函数来处理
  2. 要用一个map来记录到当前点最少步数 避免往回走的情况

- Code

  ```java
  class Solution {
      public int snakesAndLadders(int[][] board) {
          Queue<Integer> q = new LinkedList<>();
          Map<Integer, Integer> map = new HashMap<>();
          int c = board[0].length;
          q.offer(1);
          map.put(1, 0);
          
          // bfs
          while (!q.isEmpty()) {
              int cur = q.poll();
              if (cur == c * c) {
                  return map.get(cur);
              }
              for (int j = 1; j < 7; j++) {
                  int next = cur + j;
                  if (next > c * c) {
                      break;
                  }
                  int x = getCord(next, c)[0];
                  int y = getCord(next, c)[1];
                  if (board[x][y] != -1) {
                      next = board[x][y];
                  }
                  if (!map.containsKey(next)) {
                      map.put(next, map.get(cur) + 1);
                      q.offer(next);
                  }
              }
          } 
          return -1;
      }
      
      private int[] getCord(int next, int c) {
          int[] res = new int[2];
          res[0] = c - 1 - (next - 1) / c;
          if ((next - 1) / c % 2 == 0) {
              res[1] = (next - 1) % c;
          } else {
              res[1] = c - 1 - (next - 1) % c;
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

##### Word search II

- 出处

  https://leetcode.com/problems/word-search-ii/submissions/

  输入是二维char board 和 待检测的word数组

  输出那些可以找到 上下左右找 同一个char不能使用两次（在一个单词里面）

- Solution

  DFS

  注意 （1）dfs时要记录已经用过的char （2）再判断完最后一个char就输出 不要等到再进dfs一次利用cur == length 判断（因为board只有一个char时 想再进dfs都不valid）

- Complexity 

  Time: O()

  Space: O()

- Code

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

##### Path Sum II

- 出处

  https://leetcode.com/problems/path-sum-ii/

  输入是一个Tree 和一个sum 

  求所有root 到leaf 和为sum的paths

- Solution

  recursion + backtracking

  又忘了在当前层 remove path最后一个了！尤其注意当遇到一个可行方案时 别忘了也需要remove 反正就是这层你add了你就必须在return前remove了

- Complexity 

  Time: O(n)

  Space: O(1) + call stack

- Code

  ```java
  
  ```

  

### 分治法

**注意：从根到叶子的啥啥的，对于叶子的判断是**

```java
node.left == null && node.right == null
```

##### Diameter of Binary Tree

- 出处

  https://leetcode.com/problems/diameter-of-binary-tree/

- Solution

  recursion helper函数返回当前深度，同时设置instance variable（全局） max 与每步中左深度 + 右深度比较

- Complexity

  Time: O(n)

  Space: O(1)

- Code

  ```java
  class Solution {
      int maxLen;
      
      public int diameterOfBinaryTree(TreeNode root) {
          maxLen = 0;
          helper(root);
          return maxLen;
      }
      
      private int helper(TreeNode root) {
          if (root == null) {
              return 0;
          }
          if (root.left == null && root.right == null) {
              return 0;
          }
          int left = 0, right = 0;
          int cur = 0;
          if (root.left != null) {
              left = helper(root.left);
              cur += left + 1;
          }
          if (root.right != null) {
              right = helper(root.right);
              cur += right + 1;
          }
          maxLen = Math.max(maxLen, cur);
          return Math.max(left, right) + 1;
      }
  }
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

### PriorityQueue

一般求最大K 最小K的时候用PriorityQueue

PriorityQueue 默认是最小堆

add - O（log n）

remove - O（log n）

Min or Max - O（1）

##### Merge K sorted Lists

* 出处

  https://leetcode.com/problems/merge-k-sorted-lists/

  输入一个数组 每个元素是LinkedList（ListNode）

  输出一个排好序的ListNode的头

* Solution

  PriorityQueue 

  注意：（1）PriorityQueue要重写Comparator（因为是ListNode）(2) 输入长度是0单独处理 因为声明Queue的时候要声明长度 没发声明长度为0的queue 会报错（3）for循环加入头节点时要注意if(node != null)再加

* Complexity

  Time：O（n * m）

  Space: O(n)

  n is length of Array, m is average number of each ListNode

* Code

  ```java
  /**
   * Definition for singly-linked list.
   * public class ListNode {
   *     int val;
   *     ListNode next;
   *     ListNode(int x) { val = x; }
   * }
   */
  class Solution {
      Comparator<ListNode> myComparator = new Comparator<ListNode>() { // 这
          public int compare(ListNode l1, ListNode l2) {
              return l1.val - l2.val;
          }
      };
      
      public ListNode mergeKLists(ListNode[] lists) {
          if (lists.length == 0) { // 这
              return null;
          }
          ListNode dmmy = new ListNode(-1);
          ListNode prev = dmmy;
          PriorityQueue<ListNode> pq = new PriorityQueue<>(lists.length, myComparator);
          
          for (int i = 0; i < lists.length; i++) {
              if (lists[i] != null) { //这
                  pq.add(lists[i]);    
              }
          }
          
          while (pq.size() > 0) {
              ListNode node = pq.poll();
              if (node.next != null) {
                  pq.add(node.next);
              }
              prev.next = node;
              prev = node;
          }
          
          return dmmy.next;
      }
  }
  ```

  



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

  



