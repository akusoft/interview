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

## 生成器

生成器就是可以生成值的函数，当一个函数里有了 `yield` 关键字就成了生成器，生成器可以挂起执行并且保持当前执行的状态。

## 迭代器

实现以下 2 个魔法方法：

- `__iter__()`：返回一个迭代器
- `__next__()`：返回下一个迭代器

## `sort` 与 `sorted`

`sorted` 函数生成一个新的已排序的序列，`sort` 方法在原序列上进行排序。它们的底层实现都是 Timsort，这是一种归并排序和插入排序的混合体。

## `zip`

`zip()` 函数用于将可迭代对象作为参数，将对象中对应的元素**打包**成一个个元组，然后返回由这些元组组成的列表。

如下：

```python
a = [1, 2, 3, 4]
b = [5, 6, 7]

list(zip(a, b))  # [(1, 5), (2, 6), (3, 7)]
```

## `map`、`filter`、`reduce`

`map`、`filter`、`reduce` 同属于高阶函数，接受函数为参数，或者把函数作为结果返回的函数是**高阶函数**（higher-order function）。

- `map()`：`map()` 函数返回一个可迭代对象，里面的元素是把第一个参数（一个函数）应用到第二个参数（一个可迭代对象）中各个元素上得到的结果
- `filter()`
- `reduce()`

如下：

```python
from functools import reduce
from operator import add

reduce(add, range(1, 4))  # 6


list(map(str, range(5)))  # ['0', '1', '2', '3', '4']


def is_odd(n):
    return n % 2 == 1


list(filter(is_odd, range(1, 11)))  # [1, 3, 5, 7, 9]
```

## `all`、`any`

`all`、`any` 同属归约函数，**归约函数**是把某个操作连续应用到序列的元素上，累计之前的结果，把一系列值**归约**成一个值。

- `all(iterable)`：如果 `iterable` 的每个元素都是真值，返回 `True`；`all([])` 返回 `True`。
- `any(iterable)`：只要 `iterable` 中有元素是真值，就返回 `True`；`any([])` 返回 `False`。

如下：

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

## 读写文件

## 类方法、静态方法、实例方法

## `is` 与 `==` 的区别

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
