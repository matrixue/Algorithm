### general

##### int

位运算^：相同为0 不同为1，零和任何数异或都还是该数

##### string

```java
String s = "";
//concat not operate on itself
s = s.concat("adsf");

// string builder
StringBuilder sb = new StringBuilder();

//String equal
s1.equals(s2);

//String compare
s1.compareTo(s2); // s1 > s2 => result > 0

// string <-> char[]
char[] c = s.toCharArray();
String s = String.valueOf(c);

//string <-> int
int i = Integer.parseInt(s);
Integer i = Integer.valueOf(s);
String s = Integer.toString(i);
```

##### array

```java
//initialzie
int[][] arr = new int[10][10];
// or
int[][] arr = new int[][] {
  {1, 0},
  {0, 1},
}

// methods
Arrays.asList(array);
```

##### random number

```java
Random r = new Random();
int index = left + r.nextInt(right - left); 
```







### Data Structure

##### ArrayList(list)

```java
// declaration
List<Integer> list = new ArrayList<>();
//
list.add(x);
list.get(index);
```

```java
// from Set to ArrayList directly
ArrayList<Integer>(set);
```



##### ArrayDeque(Stack)

```java
Deque<Integer> stack = new ArrayDeque<>();
stack.push(10);
stack.pop();
```

```java
// implement a stack with array
```

```java
// implement a stack with linked list
```

##### LinkedList(Queue)

```java
Queue<Integer> q = new LinkedList<>();
q.offer(x);
q.poll();
```

```java
// implement a linkedlist with array
```

```java
// reverse a linked list
```

```java
// implement a queue with array
```



##### PriorityQueue(Heap)

```java
// initialization
Queue<ListNode> heap = new PriorityQueue<>(size, ListNodeComparator);
// 每次poll 最大
PriorityQueue<Integer> end = new PriorityQueue<>(100, Collections.reverseOrder());

// methods
pq.poll();
pq.add();

// how to write a comparator
private Comparator<ListNode> ListNodeComparator = new Comparator<ListNode>() {
  public int compare(ListNode left, ListNode right) {
    return left.val - right.val; //小 -> 大
  }
};

//注意外部的Integer和里面compare的参数类型要对应
PriorityQueue<Integer> heap = new PriorityQueue<>(k,new Comparator<Integer>() {
       @Override
       public int compare(Integer num1, Integer num2) {
           if (num1.intValue() == num2.intValue()) {
               // if (num1LIs.equals(num2))
               return 0;
           } else {
               return num2 < num1 ? -1 : 1;
           }
           // return num2.compareTo(num1)
           // return Integer.compare(num2, num1)         
       }
   });
   // PriorityQueue<Integer> heap = new PriorityQueue<>(k, Collections.reverseOrder());
   // Integer.valueOf(num1)

```

##### HashSet

```java
Set<Integer> s = new HashSet<>();
s.add(1);
s.remove();
s.contains(1);
// iterate
s.iterator().next()
```

##### HashMap

```java
Map<Integer, Integer> map = new HashMap<>();
map.put(1, 2);
map.get(1);
```





### Interview Useful Language Knowledge

##### Anonymous Inner Class

```java
class A {
  public void show() {
    System.out.println('A');
  }
}
/*
class B extends A {
  public vod show() {
  	System.out.println('A');
  }
}
*/

public class AnonymousExample() {
  public static void main(String[] args) {
    A a = new A() {
      // 跟 B 功能一样 如果你只需要重写 show（）这样很方便。
      // 使用方法就是new A()之后 直接大括号，里面写重写的show。重点就是大括号后面有分号
    	public vod show() {
  			System.out.println('A');
  		}
    };
  }
}

```

##### Comparator

```java
// 应用Anonymous class
Comparator<String> myCmpt = new Comparator<String>() {
  public int compare(String s1, String s2) {
    // -1 : s1 < s2 换
		// 0 : s1 == s2
		// +1 : s1 > s2 不换
  }
};
Arrays.sort(arr, myCmpt);
```



