---
title: "Python装饰器"
date: 2019-05-11T22:15:01+08:00
draft: false
author: "kenwood"
isCJKLanguage: true
tags: ["python"]
---

### python装饰器

python装饰传入一个函数，添加一些功能，然后返回它

### 闭包

先了解闭包的功能，对理解装饰器有很大的帮助

什么是闭包(closure),如何捕获变量

```python
def dog():
    height = 40
   	
    def profile():
        print("I'm a dog and my height is {}".format(height))
    return profile

if __name__ == "__main__":
    dog_profile = dog()
    dog_profile()
    
```

function dog()里面包了一个和profile()function，在调用的dog()会返回profile,在一般认知中，function中的

variable其life cycle(生命周期)会随函数执行完而消灭，理论上变量height在执行完function dog()后就要消失，但在dog_profile()(调用profile())时还能够找到variable height，原因就是return profile时，函数profile capture 住了variable,把属于上一层的变量偷渡到自己函数范围,带有 capture variable的函数就是闭包.

#### capture variable不能被assign

```python
def dog():
    height = 40
    
    def grow_up():
        height = height +1
    return grow_up

if __name__ == "__main__":
    dog_grow_up = dog()
    dog_grow_up()
```

报错UnboundLocalError，一般用global声明变量，如果在某个函数中同样命名的变量要赋值的话，一样会报UnboundLocalError，在python中，对变量赋值就等于建立局部变量

```python
global x
x = 10
def add_x():
    x = x +1
    
if __name__ == "__main__":
    add_x()
```

#### 如何赋值capture variable

```python
def dog():
    height = 40
    
    def grow_up():
        nonlocal height
        height = height +1
        print("Thanks for making me growing up.I'm now {} meters!!!".format(height))
	return grow_up

if __name__=="__main__":
    dog_grow_up = dog()
    dog_grow_up()
```

### 什么是Decorator(装饰器)

```python
# defining a decorator 
def hello_decorator(func): 

	# inner1 is a Wrapper function in 
	# which the argument is called 
	
	# inner function can access the outer local 
	# functions like in this case "func" 
	def inner1(): 
		print("Hello, this is before function execution") 

		# calling the actual function now 
		# inside the wrapper function. 
		func() 

		print("This is after function execution") 
		
	return inner1 


# defining a function, to be called inside wrapper 
def function_to_be_used(): 
	print("This is inside the function !!") 


# passing 'function_to_be_used' inside the 
# decorator to control its behavior 
function_to_be_used = hello_decorator(function_to_be_used) 


# calling the function 
function_to_be_used() 

```

**Output:**

```
Hello, this is before function execution
This is inside the function !!
This is after function execution


```

{{< figure src="/images/decorators_step.png" width="" height="" >}}

### Decorator的执行顺序

```python
def print_func_name(func):
    def warp_1():
        print("Now use function '{}'".format(func.__name__))
    	func()
    return warp_1

def print_time(func):
    import time
    def warp_2():
        print("Now the Unix time is {}".format(int(time.time())))
        func()
    return warp_2

@print_func_name
@print_time
def dog_bark():
    print("Bark !!!")
  
if __name__ == "__main__":
    dog_bark()
# > Now use function 'warp_2'
# > Now the Unix time is 16532445
# > Bark !!!
```

 dog_bark会先被@print_time吃进去，然后吐出一个warp_2的function，然后这个warp_2的function 又会被 print_func_name吃进去，返回一个叫做warp_1的function

### class式的Decorator

```python
class myDecorator:
    def __init__(self,fn):
        print("inside myDecorator.__init__()")
        self.fn = fn
    def __call__(self):
        self.fn()
        print("inside myDecorator.__call__()")
@myDecorator
def aFunction():
    print("inside aFunction()")
print("Finished decorating aFunction()")

# 输出
# inside myDecorator.__init__()
# Finished decorating aFunction()
# inside aFunction()
# inside myDecorator.__call__()
```

### Decorator的副作用

被decoratoe的函数其实已经是另一个函数了，如果你查询一下function.`__name__`的输出是"wrapper".

这会给程序埋坑，python的functool包中提供一个叫wrap的decorator来消除副作用

```python
from functools import wraps
def hello(fn):
    @wraps(fn)
    def wrapper():
        print("hello, %s"%fn.__name__)
        fn()
        print("goodby, %s"%fn.__name__)
    return wrapper
@hello
def foo():
    '''foo help doc'''
    print("I am foo")
   	pass
foo()
print(foo.__name__)
print(foo.__doc__)
```



参考文章

<https://coolshell.cn/articles/11265.html>

<https://medium.com/citycoddee/python%E9%80%B2%E9%9A%8E%E6%8A%80%E5%B7%A7-3-%E7%A5%9E%E5%A5%87%E5%8F%88%E7%BE%8E%E5%A5%BD%E7%9A%84-decorator-%E5%97%B7%E5%97%9A-6559edc87bc0>