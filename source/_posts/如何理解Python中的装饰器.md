title: 如何理解Python中的装饰器
categories: Python
date: 2016-05-26 16:29:47
tags: [Python]
---

最近接触了一点Python，基本语法比较简单但十分惊艳，本来一路畅通，然后遇到了“装饰器”这个拦路虎，突然脑袋就短路并且开始怀疑人生。于是在网上搜索了很多博客，慢慢的也有了一点理解，下面就整理一下，也给大家提供一些思路。

### 1. 装饰器的使用场景
首先，说下装饰器的概念，装饰器是一个很著名的**设计模式**，经常被用于有切面需求的场景，较为经典的有**插入日志、性能测试、事务处理**等。装饰器是解决这类问题的绝佳设计，有了装饰器，我们就可以抽离出大量函数中与函数功能本身无关的雷同代码并继续重用。概括的讲，**装饰器的作用就是为已经存在的对象添加额外的功能**。
<!--more-->
这里给出一个例子解释一下：

```
def foo():
    print 'in foo()'
foo()

```
这是一个很无聊的函数没错。但是突然有一个更无聊的人，我们称呼他为SBJ，说我想看看执行这个函数用了多长时间，好吧，那么我们可以这样做：

```
import time
def foo():
    start = time.clock()
    print 'in foo()'
    end = time.clock()
    print 'used:', end - start
 
foo()
```
看起来十分完美，可这时，SBJ又想在foo2里实现相同的功能，那把上面的代码再复制一份？非也，重复造轮子可是大忌，所以我们又有了下面的写法：

```
import time
 
def foo():
    print 'in foo()'
 
def timeit(func):
    start = time.clock()
    func()
    end =time.clock()
    print 'used:', end - start
 
timeit(foo)
```

Good job！不再需要重复造轮子了，但似乎又引来了新的问题，之前我们
是这样调用的:foo()，修改后需要改为timeit(foo)，如果之前foo这个函数已经被调用了N次，那么就不得不去把这N处都改为timeit(foo)，而万一这个函数还有别人在用，你根本无从修改，所以，必须探索其他的出路。


因此，我们必须想办法不修改调用的代码 --> foo()必须等价于timeit(foo)  -->  我们可以想到将timeit赋值给foo，但是timeit似乎带有一个参数 --> 想办法把参数统一吧。
如果timeit(foo)不是直接产生调用效果，而是返回一个与foo参数列表一致的函数的话……就很好办了，将timeit(foo)的返回值赋值给foo，然后，调用foo()的代码完全不用修改！

```
#-*- coding: UTF-8 -*-
import time
 
def foo():
    print 'in foo()'
    
    # 定义一个计时器，传入一个，并返回另一个附加了计时功能的方法
def timeit(func):
     
    # 定义一个内嵌的包装函数，给传入的函数加上计时功能的包装
    def wrapper():
        start = time.clock()
        func()
        end =time.clock()
        print 'used:', end - start
     
    # 将包装后的函数返回
    return wrapper
 
foo = timeit(foo)
foo()
```

这样，一个简易的计时器就做好了！我们只需要在定义foo以后调用foo之前，加上foo = timeit(foo)，就可以达到计时的目的，这也就是装饰器的概念，看起来像是foo被timeit装饰了。在在这个例子中，函数进入和退出时需要计时，这被称为一个横切面(Aspect)，这种编程方式被称为面向切面的编程(Aspect-Oriented Programming)。与传统编程习惯的从上往下执行方式相比较而言，像是在函数执行的流程中横向地插入了一段逻辑。在特定的业务领域里，能减少大量重复代码。面向切面编程还有相当多的术语，这里就不多做介绍，感兴趣的话可以去找找相关的资料。

这个例子仅用于演示，并没有考虑foo带有参数和有返回值的情况，完善它的重任就交给你了 ：）

上面这段代码看起来似乎已经不能再精简了，Python于是提供了一个语法糖来降低字符输入量。

```
import time
 
def timeit(func):
    def wrapper():
        start = time.clock()
        func()
        end =time.clock()
        print 'used:', end - start
    return wrapper
 
@timeit
def foo():
    print 'in foo()'
 
foo()
```
注意上面代码片段里的**@timeit**，在定义上加上这一行与另外写foo = timeit(foo)完全等价，千万不要以为@有另外的魔力。除了字符输入少了一些，还有一个额外的好处：这样看上去更有装饰器的感觉。

---- 

要理解python的装饰器，我们首先必须明白在Python中函数也是被视为对象。这一点很重要。先看一个例子：

```
def shout(word="yes") :
    return word.capitalize()+" !"
 
print shout()
# 输出 : 'Yes !'
 
# 作为一个对象，你可以把函数赋给任何其他对象变量 
 
scream = shout
 
# 注意我们没有使用圆括号，因为我们不是在调用函数
# 我们把函数shout赋给scream，也就是说你可以通过scream调用shout
 
print scream()

# 输出 : 'Yes !' 
# 还有，你可以删除旧的名字shout，但是你仍然可以通过scream来访问该函数
 
del shout
try :
    print shout()
except NameError, e :
    print e
    
#输出 : "name 'shout' is not defined"
 
print scream()

# 输出 : 'Yes !'
```


我们暂且把这个话题放旁边，我们先看看python另外一个很有意思的属性：可以在函数中定义函数：

```
def talk() :
 
# 你可以在talk中定义另外一个函数
    def whisper(word="yes") :
        return word.lower()+"...";
 
# ... 并且立马使用它
 
    print whisper()
 
# 你每次调用'talk'，定义在talk里面的whisper同样也会被调用
talk()

# 输出 :
# yes...
 

# 但是"whisper" 不会单独存在:
 
try :
    print whisper()
except NameError, e :
    print e

#输出 : "name 'whisper' is not defined"*
    
```

### 2. 函数引用

从以上两个例子我们可以得出，函数既然作为一个对象，因此：

1. 其可以被赋给其他变量

2. 其可以被定义在另外一个函数内

这也就是说，函数可以返回一个函数，看下面的例子：

```
def getTalk(type="shout") :
 
# 我们定义另外一个函数
    def shout(word="yes") :
        return word.capitalize()+" !"
 
    def whisper(word="yes") :
        return word.lower()+"...";
 
# 然后我们返回其中一个
    if type == "shout" :

# 我们没有使用(),因为我们不是在调用该函数
# 我们是在返回该函数
        return shout
    else :
        return whisper
 
# 然后怎么使用呢 ?
# 把该函数赋予某个变量
talk = getTalk()     
 
# 这里你可以看到talk其实是一个函数对象:
print talk

# 输出 : <function shout at 0xb7ea817c>
 
# 该对象由函数返回的其中一个对象:
print talk()
 
# 或者你可以直接如下调用 :
print getTalk("whisper")()

# 输出 : yes...

# 还有，既然可以返回一个函数，我们可以把它作为参数传递给函数：
def doSomethingBefore(func) :
    print "I do something before then I call the function you gave me"
    print func()
 
doSomethingBefore(scream)

# 输出 :
# I do something before then I call the function you gave me
# Yes !

```
这里你已经足够能理解装饰器了，其他它可被视为封装器。也就是说，它能够让你在装饰前后执行代码而无须改变函数本身内容。

### 3. 手工装饰
那么如何进行手动装饰呢？

```


# 装饰器是一个函数，而其参数为另外一个函数
def my_shiny_new_decorator(a_function_to_decorate) :
 
# 在内部定义了另外一个函数：一个封装器。
# 这个函数将原始函数进行封装，所以你可以在它之前或者之后执行一些代码
    def the_wrapper_around_the_original_function() :
 
        # 放一些你希望在真正函数执行前的一些代码
        print "Before the function runs"
 
        # 执行原始函数
        a_function_to_decorate()
 
        # 放一些你希望在原始函数执行后的一些代码
        print "After the function runs"
 
    #在此刻，"a_function_to_decrorate"还没有被执行，我们返回了创建的封装函数
    #封装器包含了函数以及其前后执行的代码，其已经准备完毕
    return the_wrapper_around_the_original_function
 
	# 现在想象下，你创建了一个你永远也不远再次接触的函数
def a_stand_alone_function() :
    print "I am a stand alone function, don't you dare modify me"
 
a_stand_alone_function()
	#输出: I am a stand alone function, don't you dare modify me
 
	# 好了，你可以封装它实现行为的扩展。可以简单的把它丢给装饰器
	# 装饰器将动态地把它和你要的代码封装起来，并且返回一个新的可用的函数。
	
a_stand_alone_function_decorated = my_shiny_new_decorator(a_stand_alone_function)
a_stand_alone_function_decorated()
	
	#输出 :
	#Before the function runs
	#I am a stand alone function, don't you dare modify me
	#After the function runs
	#现在你也许要求当每次调用a_stand_alone_function时，实际调用却是
	#a_stand_alone_function_decorated。实现也很简单，可以用	#my_shiny_new_decorator来给a_stand_alone_function重新赋值。

a_stand_alone_function = my_shiny_new_decorator(a_stand_alone_function)
a_stand_alone_function()
	
	#输出 :
	#Before the function runs
	#I am a stand alone function, don't you dare modify me
	#After the function runs
 
	# And guess what, that's EXACTLY what decorators do !

```

### 4. 装饰器揭秘
前面的例子，我们可以使用装饰器的语法：

```
@my_shiny_new_decorator
def another_stand_alone_function() :
    print "Leave me alone"
 
another_stand_alone_function()
#输出 :
#Before the function runs
#Leave me alone
#After the function runs
当然你也可以累积装饰：

def bread(func) :
    def wrapper() :
        print "</''''''\>"
        func()
        print "<\______/>"
    return wrapper
 
def ingredients(func) :
    def wrapper() :
        print "#tomatoes#"
        func()
        print "~salad~"
    return wrapper
 
def sandwich(food="--ham--") :
    print food
 
sandwich()
#输出 : --ham--
sandwich = bread(ingredients(sandwich))
sandwich()
#outputs :
#</''''''\>
# #tomatoes#
# --ham--
# ~salad~
#<\______/>
使用python装饰器语法：

@bread
@ingredients
def sandwich(food="--ham--") :
    print food
 
sandwich()

#输出 :
#</''''''\>
# #tomatoes#
# --ham--
# ~salad~
#<\______/>

#装饰器的顺序很重要，需要注意：
@ingredients
@bread
def strange_sandwich(food="--ham--") :
    print food
 
strange_sandwich()

#输出 :
##tomatoes#
#</''''''\>
# --ham--
#<\______/>
# ~salad~

# 最后解决一个问题：
# 装饰器makebold用于转换为粗体
def makebold(fn):
    # 结果返回该函数
    def wrapper():
        # 插入一些执行前后的代码
        return "<b>" + fn() + "</b>"
    return wrapper
 
# 装饰器makeitalic用于转换为斜体
def makeitalic(fn):
    # 结果返回该函数
    def wrapper():
        # 插入一些执行前后的代码
        return "<i>" + fn() + "</i>"
    return wrapper
 
@makebold
@makeitalic
def say():
    return "hello"
 
print say()
#输出: <b><i>hello</i></b>
 
# 等同于
def say():
    return "hello"
say = makebold(makeitalic(say))
 
print say()
#输出: <b><i>hello</i></b>
```

### 5. 内置的装饰器
内置的装饰器有三个，分别是staticmethod、classmethod和property，作用分别是把类中定义的实例方法变成静态方法、类方法和类属性。由于模块里可以定义函数，所以静态方法和类方法的用处并不是太多，除非你想要完全的面向对象编程。


That's all, thank you.