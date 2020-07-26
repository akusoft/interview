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

详见 [`object.h`](https://github.com/python/cpython/blob/master/Include/object.h) 文件，
其他类型对象的定义详见
[`floatobject.h`](https://github.com/python/cpython/blob/master/Include/floatobject.h) 、
[`longobject.h`](https://github.com/python/cpython/blob/master/Include/longobject.h) 、
[`listobject.h`](https://github.com/python/cpython/blob/master/Include/listobject.h) 、
[`tupleobject.h`](https://github.com/python/cpython/blob/master/Include/tupleobject.h) 、
[`dictobject.h`](https://github.com/python/cpython/blob/master/Include/dictobject.h)
等。

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
