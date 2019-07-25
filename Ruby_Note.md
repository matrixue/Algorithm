### String

* 双引号修饰的字符串允许替换和反斜杠，单引号不行
* 用#{}替换字符串里的表达式为一个值

```ruby
puts "it is #{10*10}"
```

* 字符串替换 牛逼！

```ruby
a = '123456'
a['123'] = '456' # a => '456456'
```

* 字符串反转

```ruby
a.reverse
```



### Array

* 遍历数组

```ruby
a.each do |index, value|
    puts n
end
```



### Loop

##### while loop 

* fjsdf

```ruby
while i < 10 [do]
    puts i
    i = i + 1
end
```

* if

```ruby
if x < 10 [then]
    puts x, '\n'
else puts 'nothing'
end
```



* case

```ruby
case x
    when exp1, exp2
    	xxxx
    when exp3
    	xxxx
	else 
    	xxxx
end
```



### OOP例子

```ruby
class Resume
    def initialize(name) #注意initialize
        @name = name #注意@
        @age = 18
    end
    
    def show_profile #注意没有： 注意没有括号
        p "name is #{@name} and age is #{@age}"
    end #注意有end
end

resume = Resume.new('huan')
resume.show_profil
```

### test

