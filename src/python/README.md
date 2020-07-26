# Python

## 1. CPython中的GIL

GIL，全称Global Interpreter Lock，即**全局解释器锁**。

由于CPython解释器的内存管理并不是线程安全的，为了保护多线程情况下对Python对象的访问，所以CPython使用简单的锁机制避免多个线程同时执行字节码。

GIL的影响（限制了程序的多核执行）：

- 同一个时间只能有一个线程执行字节码
- CPU密集型程序难以利用多核优势
- IO期间会释放GIL，对IO密集型程序影响不大

如何规避GIL的影响（区分CPU和IO密集型程序）：

- CPU密集可以使用多进程+进程池
- IO密集可以使用多线程/协程

## 2. Python的值类型和引用类型

- 值类型：对象本身不允许修改
- 引用类型：对象本身可以修改

## 3. Python的数据类型

- 不可变（值类型）：Number、String、Tuple
- 可变（引用类型）：List、Dictionary、Set

## 4. list作为参数会被改掉

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

## 5. Node和Tornado快是因为：事件驱动和异步非阻塞

## 6. Python是非真实并行的“假多线程”，对CPU密集型不友善

## 7. Python中数组与链表的区别

数组中存储的数据类型必须相同，且数组一经确定，大小即固定不变；而列表中的元素类型可不同，列表的大小也不固定。Python中没有数组。

## 8. Python实现斐波那契数列

```python
def fib(n):
    if n <= 1:
        return n
    else:
        return fib(n - 1) + fib(n - 2)


for i in range(10):
    print(fib(i))
```

## 9. Python是动态强类型语言

动态还是静态指的是编译期还是运行期确定类型，强类型指的是不会发生隐式类型转换。

## 10. 鸭子类型

关注点在对象的行为，而不是类型（duck typing）。

鸭子类型更关注接口而非类型。

## 11. monkey patch指运行时替换

## 12. 自省（Introspection）是指运行时判断一个对象的类型的能力

## 13. is与=的区别

## 14. 列表和字典推导

```python
[i for i in range(10) if i % 2 == 0]
```

## 15. Python之禅

```python
import this
```

## 16. Python 2与Python 3的差异

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

## 17. `*args`与`**kwargs`

函数有4种参数形式：固定参数、默认参数、可变参数args、可变参数kwargs。

用来处理可变参数

`*args`被打包成tuple（`*`），`**kwargs`被打包成dict（`**`）。

## 18. Python中的异常

所有异常都继承自`BaseException`类，要定义自己的异常需要继承自`Exception`类。

## 19. Python的生成器

生成器就是可以生成值的函数，当一个函数里有了`yield`关键字就成了生成器，生成器可以挂起执行并且保持当前执行的状态。

## 20. Python的装饰器（decorator）

## 21. 协程

Python 2 中基于生成器的协程：
Python 3 中的原生协程：

## 22. 单元测试

Python中常用的单元测试相关库：`pytest`。

## 23. 深拷贝与浅拷贝

深拷贝：
浅拷贝：
实现深拷贝：

## 24. 一行代码实现1-100之和

```python
result = sum(range(1, 101))
```

## 25. 列举5个Python标准库

- `os`：封装了常见的文件和目录操作
- `urllib`：网络请求模块，包括对url的结构解析
- `asyncio`：Python的异步库，基于事件循环的协程模块
- `re`：正则表达式模块
- `itertools`：提供了操作生成器的一些模块

## 26. 列表去重

```python
lis = [1, 2, 3, 4, 5, 4]
list(set(lis))
```

## 27. 单引号、双引号、三引号的区别

单引号和双引号无区别。三引号又分为三双引号和三单引号，这两者也无区别，三引号主要用于文档字符串，会保留格式信息。三引号也用于多行注释。

## 28. 单下划线和双下划线区别
