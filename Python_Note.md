### General

* 判断是否为空， 不论是String dict还是list通用：

```python
if not a:
```

* 最大值最小值

```python
sys.maxsize
-sys.maxsize - 1
```

* import

```python
from colllections import deque
```

* 判断null

```python
# 没有null这个keyword
if a is None:
  ...
# or
if a:
  ...
```





### Loop

* for loop

```python
# 只计数 不需要知道每项
for _ in range(size):
```





### Dict

* 判断在不在dict里

```python
if a in d:
```

* 初始化一个dict，key为已知数组nums

```python
{i:0 for i in nums}
```

* 移除一个

```python
a.pop(x)
```



### String

string 不能修改

* 判断成员运算符

```python
'j' in a
```

* 标准化输出

```python
print("My name is %s, and i am %d old" % ('huan', 24))
```

* 每个首字母大写

```python
a.capitalize()
```

* 以指定字符分割

```python
'-'.join(a)
''.join(a) #copy
```



* 去首尾空格

```python
s.strip()
```

* 判断字符串开头 or 结尾

```python
a.startswith(word)
a.endwith(word)
```





### List

* 排序

```python
nums.sort() # sort in place and return None
sorted(nums) # return a copy sorted array
```



* 翻转

```python
a.reverse() 
#或
a[::-1]
```

* 深度拷贝

```python
Combination = [1]
Results = []
# 这样combination 变了 append的东西不会变 这是我们想要的 记住 重要
Results.append(combination[:])
```





### 队列

* deque 双端队列

```python
# how to initize
q = collections.deque()
q = collections.deque([root]) # 特别注意: [] 不然报错无法iterate
size = len(q)
q.append(x)
q.popleft() # 一定是popleft deque其实是double-end queue
```

### 集合

```python
a = set()
a = set(['foo', 'abb']) #注意中括号
```

### OOP 例子

```python
class Resume:
    def __init__(self, name): #注意 __init__ 和self
        self.name = name
        self.age = 18
        
    def show_profile(self): #注意self
        print('name is ' + self.name + ' and age is ' + str(self.age))
       
resume = Resume('huan')#注意传入参数的方式
```



### 动态语言特性

* 判断是直接执行还是被import调用

```python
if __name__ == '__main__':
    #write you code
```



