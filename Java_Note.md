### general



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

```



##### ArrayDeque(Stack)

```java
Deque<Integer> stack = new ArrayDeque<>();
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
Queue<ListNode> heap = new PriotiryQueue<>(size, ListNodeComparator);

// how to write a comparator
private Comparator<ListNode> ListNodeComparator = new Comparator<>() {
  public int compare(ListNode left, ListNode right) {
    return left.val - right.val; //小 -> 大
  }
}
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
    // -1 : s1 < s2
		// 0 : s1 == s2
		// +1 : s1 > s2
  }
};
Arrays.sort(arr, myCmpt);
```



