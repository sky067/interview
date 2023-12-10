# Python3 基础

## GIL（全局解释器）
1. 
## 迭代器

## 生成器

## 装饰器
1. 什么是装饰器
2. 装饰器的使用场景


3. 函数装饰器 与 类装饰器 的区别

# 系统编程

## 进程、线程、协程

### 进程

进程之间数据不共享，开销较大


### 线程

1. 线程是操作系统调度资源的最小单位。
2. 进程至少有一个线程。
3. 线程间数据共享。

### 协程

1. 什么是协程
2. 协程的作用
2. 协程的底层实现
2. 协程什么时候会挂起，什么时候会启动

## 并发编程（多线程、多进程）

### 多线程

### 多进程

## 设计模式

### 单例模式

- 实现单例模式

```python
def single(cls):
    cls_obj = {}  # 一个类对应一个实例
    def f():
        if cls not in cls_obj:
            obj = cls()  # 创建实例
            cls_obj[cls] = obj
        return cls_obj[cls]
    return f

@single
class Message:
    def __init__(self):
        return


m1 = Message()
print(m1)
m2 = Message()
print(m2)
```
