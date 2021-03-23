# Python

## 值类型和引用类型

- 值类型：对象本身不允许修改
- 引用类型：对象本身可以修改

## 可变类型和不可变类型

- 不可变（值类型）：Number、String、Tuple
- 可变（引用类型）：List、Dictionary、Set

## 深拷贝与浅拷贝

深拷贝：
浅拷贝：
实现深拷贝：

## 装饰器（decorator）

```python
import functools


def outer(func):
    @functools.wraps(func)
    def inner(*args, **kwargs):
        print('before')
        result = func(*args, **kwargs)
        print('after')
        return result
    return inner


@outer  # func = outer(func)
def func():
    print('func')


func()
print(func.__name__)
```

参考：

1. [闭包和装饰器](https://github.com/howiezhao/python-notebook/blob/master/notebook/04-closure-decorator.ipynb)

## 迭代器（iterator）与可迭代对象（iterable）

**迭代器类**实现了以下 2 个魔法方法：

- `__iter__()`：返回对象本身，即 `self`
- `__next__()`：返回下一个数据，若没有数据了，则抛出 `StopIteration` 异常

根据迭代器类可以实例化一个**迭代器对象**。

迭代器对象支持通过 `next` 取值，若取值结束则自动抛出 `StopIteration`。

`for` 循环内部在循环时，先执行 `__iter__` 方法，获取一个迭代器对象，然后不断的执行 `next` 取值，有异常 `StopIteration` 则终止循环。

如下：

```python
# 创建迭代器类
class IT(object):
    def __init__(self):
        self.counter = 0

    def __iter__(self):
        return self

    def __next__(self):
        self.counter += 1
        if self.counter == 3:
            raise StopIteration()
        return self.counter


# 根据迭代器类实例化一个迭代器对象
obj1 = IT()
# 使用迭代器对象
obj1.__next__()  # 1
obj1.__next__()  # 2
obj1.__next__()  # 抛出异常

# 另一种使用迭代器对象的方法
obj2 = IT()
next(obj2)  # 等同于 obj2.__next__()，输出 1
next(obj2)  # 2
next(obj2)  # 抛出异常

# 迭代器对象可以使用 for 语句
obj3 = IT()
for item in obj3:
    print(item)  # 1 2
```

如果一个类中有 `__iter__` 方法且返回一个迭代器对象（或生成器对象），则我们称以这个类创建的对象为**可迭代对象**。

列表、字典、元组等都是可迭代对象。

可迭代对象是可以使用 `for` 来进行循环，在循环的内部其实是先执行 `__iter__` 方法，获取其迭代器对象，然后再在内部执行这个迭代器对象的 `next` 功能，逐步取值。

如下：

```python
class Foo(object):
    def __iter__(self):
        return IT()


# 创建可迭代对象
obj = Foo()

for item in obj:
    print(item)  # 1 2
```

总结，迭代器对象内部有 `__iter__`、`__next__` 方法，而可迭代对象内部只有 `__iter__` 方法。如下：

```python
v1 = range(10)  # v1 是可迭代对象，可通过 dir() 查看
v2 = v1.__iter__()  # v2 是迭代器对象
v2.__next__()  # 0
```

## 生成器

当一个函数里有了 `yield` 关键字就成了**生成器函数**，调用生成器函数就可以得到一个**生成器对象**。

得到生成器对象时，内部是根据生成器类 generator 创建的对象，生成器类的内部也声明了 `__iter__`、`__next__` 方法

如果按照迭代器的规定来看，其实生成器类也是一种特殊的迭代器类，即**生成器也是一种特殊的迭代器**。

如下：

```python
# 创建生成器函数
def func():
    yield 1
    yield 2


# 创建生成器对象
obj1 = func()

next(obj1)  # 1
next(obj1)  # 2
next(obj1)  # 抛出异常

obj2 = func
for item in obj2:
    print(item)  # 1 2
```

生成器就是可以生成值的函数，

生成器可以挂起执行并且保持当前执行的状态。

## `sort` 与 `sorted`

`sorted` 函数生成一个新的已排序的序列，`sort` 方法在原序列上进行排序。如下：

```python
a = [2, 6, 3, 1, 2]
print(sorted(a))  # [1, 2, 2, 3, 6]
print(a)  # [2, 6, 3, 1, 2]
print(a.sort())  # None
print(a)  # [1, 2, 2, 3, 6]
```

不管是 `sorted` 函数还是 `sort` 方法，都有两个可选的关键字参数：

- `reverse`：`True` 则降序，默认为 `False`（即顺序）
- `key`：单参数函数，用于排序算法

它们背后的排序算法是 [Timsort](https://zh.wikipedia.org/wiki/Timsort)，这是一种自适应算法、会根据原始数据的顺序特点交替使用插入排序和归并排序，以达到最佳效率。这样的算法被证明是很有效的，因为来自真实世界的数据通常是有一定的顺序特点的。

由于插入排序和归并排序是稳定的，所以 Timsort 也是稳定的排序算法，即就算两个元素比不出大小，在每次排序的结果里它们的相对位置是固定的。

Timsort 的创始人是 Tim Peters，他是 Python 核心开发者，也是 “Python 之禅”（`import this`）的作者。

## `zip`

`zip()` 函数用于将可迭代对象作为参数，将对象中对应的元素**打包**成一个个元组，然后返回由这些元组组成的列表。

如下：

```python
a = [1, 2, 3, 4]
b = [5, 6, 7]

list(zip(a, b))  # [(1, 5), (2, 6), (3, 7)]
```

## `map`、`filter`、`reduce`

```python
from functools import reduce
from operator import add

reduce(add, range(1, 4))  # 6


list(map(str, range(5)))  # ['0', '1', '2', '3', '4']


def is_odd(n):
    return n % 2 == 1


list(filter(is_odd, range(1, 11)))  # [1, 3, 5, 7, 9]
```

参考：

1. [函数](https://github.com/howiezhao/python-notebook/blob/master/notebook/05-functions.ipynb)

## `all`、`any`

```python
lis1 = [True, True, True]
lis2 = [True, True, False]
lis3 = [False, False, False]

all(lis1)  # True
all(lis2)  # False
all(lis3)  # False

any(lis1)  # True
any(lis2)  # True
any(lis3)  # False
```

参考：

1. [函数](https://github.com/howiezhao/python-notebook/blob/master/notebook/05-functions.ipynb)

## 读写文件

```python
with open('file.txt', 'r') as file:
    print(file.read())

with open('file.txt', 'w') as file:
    file.write('hello')
```

其中：

- `r`：读
- `w`：写
- `b`：二进制模式
- `a`：追加
- `+`：可读可写

## classmethod（类方法）与 staticmethod（静态方法）

- classmethod：定义操作类，而不是操作实例的方法。
- staticmethod：就是普通的函数，只是碰巧在类的定义体中，而不是在模块层定义。

`classmethod` 改变了调用方法的方式，因此类方法的第一个参数是类本身，而不是实例。classmethod 最常见的用途是定义备选构造方法。按照约定，类方法的第一个参数名为 `cls`（但是 Python 不介意具体怎么命名）。

`staticmethod` 装饰器也会改变方法的调用方式，但是第一个参数不是特殊的值。

`classmethod` 装饰器非常有用，`staticmethod` 则不是特别有用。如果想定义不需要与类交互的函数，那么在模块中定义就好了。有时，函数虽然从不处理类，但是函数的功能与类紧密相关，因此想把它放在近处。即便如此，在同一模块中的类前面或后面定义函数也就行了。

```python
class Demo:
    @classmethod
    def klassmeth(*args):
        return args

    @staticmethod
    def staticmeth(*args):
        return args


print(Demo.klassmeth())  # (<class '__main__.Demo'>,)
print(Demo.klassmeth('spam'))  # (<class '__main__.Demo'>, 'spam')
print(Demo().klassmeth())  # (<class '__main__.Demo'>,)

print(Demo.staticmeth())  # ()
print(Demo.staticmeth('spam'))  # ('spam',)
print(Demo().staticmeth())  # ()
print(staticmeth('spam'))  # NameError: name 'staticmeth' is not defined
```

参考：

1. [Luciano Ramalho.流畅的Python[M].北京:人民邮电出版社,2017:208-210.](https://book.douban.com/subject/27028517/)
2. [如何在Python中正确使用static、class、abstract方法](https://foofish.net/guide-python-static-class-abstract-methods.html)

## `is` 与 `==` 的区别

`==` 运算符比较两个对象的值（对象中保存的数据），而 `is` 运算符比较对象的标识（或称编号）。如下：

```python
a = [1, 2, 3]
b = [1, 2, 3]
a == b  # True
a is b  # False
```

以下引用自 [Python 语言参考：3.数据模型](https://docs.python.org/zh-cn/3/reference/datamodel.html)：

> 每个对象都有各自的编号、类型和值。一个对象被创建后，它的 *编号* 就绝不会改变；你可以将其理解为该对象在内存中的地址。 '`is`' 运算符可以比较两个对象的编号是否相同；`id()` 函数能返回一个代表其编号的整型数。

通常，我们关注的是值，而不是标识，因此 Python 代码中 `==` 出现的频率比 `is` 高。

然而，在变量和单例值之间比较时，应该使用 `is`。目前，最常使用 `is` 检查变量绑定的值是不是 `None`。下面是推荐的写法：

```python
x is None
```

否定的正确写法是：

```python
x is not None
```

`is` 运算符比 `==` 速度快，因为它不能重载，所以 Python 不用寻找并调用特殊方法，而是直接比较两个整数 ID。而 `a == b` 是语法糖，等同于 `a.__eq__(b)`。继承自 `object` 的 `__eq__` 方法比较两个对象的 ID，结果与 `is` 一样。但是多数内置类型使用更有意义的方式覆盖了 `__eq__` 方法，会考虑对象属性的值。相等性测试可能涉及大量处理工作，例如，比较大型集合或嵌套层级深的结构时。

参考：

1. [Luciano Ramalho.流畅的Python[M].北京:人民邮电出版社,2017:185-186.](https://book.douban.com/subject/27028517/)

## Node 和 Tornado 快是因为：事件驱动和异步非阻塞

## Python 是非真实并行的“假多线程”，对 CPU 密集型不友善

## Python 中数组与链表的区别

数组中存储的数据类型必须相同，且数组一经确定，大小即固定不变；而列表中的元素类型可不同，列表的大小也不固定。

## Python 是动态强类型语言

动态还是静态指的是编译期还是运行期确定类型，强类型指的是不会发生隐式类型转换。

## 鸭子类型

关注点在对象的行为，而不是类型（duck typing）。

鸭子类型更关注接口而非类型。

## monkey patch指运行时替换

## 自省（Introspection）是指运行时判断一个对象的类型的能力

## 列表和字典推导

```python
[i for i in range(10) if i % 2 == 0]
```

## Python 2 与 Python 3 的差异

Python 3 改进：

- `print`成为函数（Python 2 中为关键字）
- 编码问题。Python 3 不再有Unicode对象，默认str就是unicode（Python 2 中默认为字节）。
- 除法变化。Python 3 除号返回浮点数。
- 增加类型注解（type hint）。可帮助IDE实现类型检查

  ```python
  def hello(name: str) -> str
      return 'hello ' + name
  ```

- 优化的`super()`方便直接调用父类函数。
- 高级解包操作。`a, b, *rest = range(10)`
- 限定关键字参数
- Python 3 重新抛出异常不会丢失栈信息。`raise from`
- 一切返回迭代器，如`range`、`zip`、`map`、`dict.values`等。（Python 2 中`range`、`map`返回列表）
- 生成的`pyc`文件统一放到__pycache__
- 一些内置库的修改，如`urllib`、`selector`等
- 性能优化等，如`dict`

Python 3 新增：

- yield from 链接子生成器
- asyncio内置库，async/await原生协程支持异步编程
- 新的内置库，如enum、mock、asyncio、ipaddress、concurrent.futures等

兼容2/3的工具：

- `six`模块
- `2to3`等工具转换代码
- `__future__`

## `*args` 与 `**kwargs`

函数有4种参数形式：固定参数、默认参数、可变参数args、可变参数kwargs。

用来处理可变参数

`*args`被打包成tuple（`*`），`**kwargs`被打包成dict（`**`）。

## Python 中的异常

所有异常都继承自 `BaseException` 类，要定义自己的异常需要继承自 `Exception` 类。

## 协程

Python 2 中基于生成器的协程：
Python 3 中的原生协程：

## 列举 5 个 Python 标准库

- `os`：封装了常见的文件和目录操作
- `urllib`：网络请求模块，包括对url的结构解析
- `asyncio`：Python的异步库，基于事件循环的协程模块
- `re`：正则表达式模块
- `itertools`：提供了操作生成器的一些模块

## 单引号、双引号、三引号的区别

单引号和双引号无区别。三引号又分为三双引号和三单引号，这两者也无区别，三引号主要用于文档字符串，会保留格式信息。三引号也用于多行注释。

## 单下划线和双下划线区别

## Python 中运算符重载的实现

通过重写相应的魔法方法实现，如下：

- `__add__`：`+`
- `__sub__`：`-`
- `__lt__`：`<`
- `__eq__`：`==`
- `__gt__`：`>`

等。

## Python 实现斐波那契数列

```python
def fib(n):
    if n <= 1:
        return n
    else:
        return fib(n - 1) + fib(n - 2)


for i in range(10):
    print(fib(i))
```

## 一行代码实现 1-100 之和

```python
result = sum(range(1, 101))
```

## 列表去重

```python
lis = [1, 2, 3, 4, 5, 4]
list(set(lis))
```

## `list` 作为参数会被改掉

```python
lis = [1, 2, 3]


def foo(v):
    v[1] = 4
    return v


print(foo(lis))
print(lis)

Output:
[1, 4, 3]
[1, 4, 3]
```
