# CPython

CPython 是 Python 中使用最多的解释器，此节主要记录与 CPython 相关的知识点。

## 垃圾回收和内存管理

一句话总结：引用计数器为主，标记清除和分代回收为辅。

### 引用计数器

在 Python 程序中创建的任何对象都会放在**环状双向链表**
[`refchain`](https://github.com/python/cpython/blob/master/Objects/object.c#L83) 中，
每创建一个对象，系统内部都会创建一些数据，不同对象创建的数据各不相同，但所有对象都有相同的一些数据，
即 [`PyObject`](https://github.com/python/cpython/blob/master/Include/object.h#L105-L109) 结构体。

简单来说，`PyObject` 结构体包含如下数据：

- 下一个对象：`_ob_next`
- 上一个对象：`_ob_prev`
- 对象的引用个数：`ob_refcnt`
- 对象的类型：`ob_type`

详见 [`object.h`](https://github.com/python/cpython/blob/master/Include/object.h) 文件。

具体而言，相应类型对象的定义详见
[`floatobject.h`](https://github.com/python/cpython/blob/master/Include/floatobject.h) 、
[`longobject.h`](https://github.com/python/cpython/blob/master/Include/longobject.h) 、
[`listobject.h`](https://github.com/python/cpython/blob/master/Include/cpython/listobject.h) 、
[`tupleobject.h`](https://github.com/python/cpython/blob/master/Include/cpython/tupleobject.h) 、
[`dictobject.h`](https://github.com/python/cpython/blob/master/Include/cpython/dictobject.h)
等。

当Python程序运行时，会根据数据类型的不同找到其对应的结构体，
根据结构体中的字段来进行创建相关的数据，然后将对象添加到 `refchain` 双向链表中。

每个对象中的 `ob_refcnt` 就是引用计数器，值默认为 1，
当有其他变量引用对象时，引用计数器就会发生变化。

当一个对象的引用计数器为 0 时，意味着没有人再使用这个对象了，
这个对象就是垃圾，则进行垃圾回收。所谓的回收就是做了 2 件事：

- 将对象从 `refchain` 双向链表中移除
- 将对象销毁，内存归还

### 标记清除

单单使用引用计数器会导致**循环引用**（交叉感染）问题，如下代码所示：

```python
v1 = [1, 2, 3]  # refchain中创建一个列表对象，由于v1=对象，所以列表对象引用计数器为1
v2 = [4, 5, 6]  # refchain中再创建一个列表对象，由于v2=对象，所以列表对象引用计数器为1
v1.append(v2)  # 把v2追加到v1中，则v2对应的[4, 5, 6]对象的引用计数器加1，最终为2
v2.append(v1)  # 把v1追加到v2中，则v1对应的[1, 2, 3]对象的引用计数器加1，最终为2

del v1  # 引用计数器-1
del v2  # 引用计数器-1
```

显然，我们已经删除了 `v1` 和 `v2`，但它们的引用计数器并不为 0，
这将导致垃圾回收机制不会运行，引起内存泄露。

为了解决引用计数器循环引用的不足，Python 内部引入了**标记清除**机制。

要实现标记清除机制，需要在 Python 的底层再维护一个链表，
链表中专门放那些可能存在循环引用的对象（列表/字典/集合）。

在 Python 内部**某种情况**下触发，会去扫描**可能存在循环引用的链表**中的每个元素，
检查是否有循环引用，如果有则让双方的引用计数器 -1；如果是 0 则垃圾回收。

### 分代回收

标记清除存在 2 个问题：

- 什么时候扫描？
- 可能存在循环引用的链表扫描代价大，每次扫描耗时久。

为了解决这 2 个问题，Python 又引入了**分代回收**机制，简而言之，
就是将可能存在循环引用的对象维护成 3 个列表：

- 0 代：0 代中对象个数达到 700 个扫描 1 次
- 1 代：0 代扫描 10 次，则 1 代扫描 1 次
- 2 代：1 代扫描 10 次，则 2 代扫描 1 次

扫描 0 代的过程中，如果有循环引用则自减 1，是垃圾自动回收，
若不是垃圾，则升级到 1 代。

详见 [`gcmodule.c`](https://github.com/python/cpython/blob/master/Modules/gcmodule.c) 文件。

### 缓存机制

基于上述流程，Python 内部还实现了一些优化机制，主要有以下 2 个：

#### 池

为了避免重复创建和销毁一些常见对象（如整型）而维护池。

具体而言，启动解释器时，Python 内部帮我们创建 `[-5, -4, ..., 257]`，
当定义 `v1 = 7` 时，内部不会开辟内存，而是直接去池中获取。

#### `free_list`

当一个对象（如浮点型/列表/元组/字典）的引用计数器为 0 时，
按理说应该回收，内部不会直接回收，而是将对象添加到 `free_list` 链表中当缓存。
以后再去创建对象时，不再重新开辟内存，而是直接使用 `free_list`。
若 `free_list` 满了则销毁。

### 总结

在 Python 中维护了一个 `refchain` 的双向环状链表，这个链表中存储程序创建的所有对象，
每种类型的对象中都有一个 `ob_refcnt` 引用计数器的值，引用个数 +1、-1，
最后当引用计数器变为 0 时会进行垃圾回收（对象销毁、`refchain` 中移除）。

但是，在 Python 中对于那些可以有多个元素组成的对象可能会存在循环引用的问题，
为了解决这个问题，Python 又引入了标记清除和分代回收，在其内部为 4 个链表：

- `refchain`
- 2 代，10 次
- 1 代，10 次
- 0 代，700 个

在源码内部当达到各自的阈值时，就会触发扫描链表进行标记清除的动作（有循环则各自 -1）。

## CPython 中的 GIL

GIL，全称 Global Interpreter Lock，即**全局解释器锁**。

由于 CPython 解释器的内存管理并不是线程安全的，为了保护多线程情况下对 Python 对象的访问，
所以 CPython 使用简单的锁机制避免多个线程同时执行字节码。

GIL 的影响（限制了程序的多核执行）：

- 同一个时间只能有一个线程执行字节码
- CPU 密集型程序难以利用多核优势
- IO 期间会释放 GIL，对 IO 密集型程序影响不大

如何规避 GIL 的影响（区分 CPU 和 IO 密集型程序）：

- CPU 密集可以使用多进程+进程池
- IO 密集可以使用多线程/协程
