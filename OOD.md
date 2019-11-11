### 注意事项

##### 多交流 多提问

##### 不要变成System Design

##### 翻来覆去改

##### 最后检查一下



### 评价标准

##### Communication

##### Thinking Process

##### Correctness

##### Reasonable

* Elegant
* Maintainable
* Extenable
* 通过封装 继承 多态来实现以上三点
* 具体表现形式 SOLID

##### *Good Practice

* 继承 vs interface
  * 一个class只能继承一个class（父亲）
  * Interface：如果一个class想implement这个interface 它必须实现所有的函数，class可以implement很多interface

#####  *Design Pattern

### S.O.L.I.D.

* single resonsibility principle 

  一个类应该有且只有一个改变它的理由，这个类只有一项工作

* Open close Principle

  对扩展（extension）开放 对修改（modification）封闭

* Linkov Substitution Principle

  任何一个子类应该能够替换它的父类

* Interface Segregation Principle

  不能强迫一个类实现它不需要的接口

* Dependence Inversion Principle

  抽象不应该依赖具体的实现

### 5C原则

##### Clarify

跟面试官沟通 去除歧义

主要是 **what** 和 **how** 偶尔会有who 和 when

##### Core Object

确定涉及的类 类与类的关系

| Class Name            |
| --------------------- |
| Attributes            |
| Functions(operation ) |

"-" 表示private

"+" 表示public

"#" 表示protected

##### Cases

确定use case （应用场景和功能）

##### Classes

根据类图（class diagram） 填充缺失的类

##### Correctness

检查自己的设计 是否满足关键点



### 管理类

* 把自己想成物体 比如一个停车场 不要以一个车一个司机的角度去想 以一个停车场的角度去想 去想输入输出

 ### 例题

##### 电梯

