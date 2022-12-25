---
title: interview questions
date: 2022-04-12 22:35:32
tags:
- 
- 
categories:
- interview
cover: ../../images/articles/lock.png
---
## redis during

## mq
消息队列结构，怎么放消息,怎么使用

## synchronize lock

## 幂等性

## hashtable and hashmap

## red-black tree

## 锁 
使用情况，乐观悲观可重入
[reference article](https://tech.meituan.com/2018/11/15/java-lock.html)
### the status of lock
针对synchronized(悲观锁)关键字（JVM)，锁有四种状态:
无锁，偏向锁，轻量级锁，重量级锁
为什么synchronized实现线程同步？
Hotspot的对象头主要包括两部分数据：Mark Word（标记字段）、Klass Pointer（类型指针）
现在话题回到synchronized，synchronized通过Monitor来实现线程同步，Monitor是依赖于底层的操作系统的Mutex Lock（互斥锁）来实现的线程同步。
如同我们在自旋锁中提到的“阻塞或唤醒一个Java线程需要操作系统切换CPU状态来完成，这种状态转换需要耗费处理器时间。如果同步代码块中的内容过于简单，
状态转换消耗的时间有可能比用户代码执行的时间还要长”。这种方式就是synchronized最初实现同步的方式，这就是JDK 6之前synchronized效率低的原因。
这种依赖于操作系统Mutex Lock所实现的锁我们称之为“重量级锁”，JDK 6中为了减少获得锁和释放锁带来的性能消耗，引入了“偏向锁”和“轻量级锁”。
|锁状态	|存储内容	|存储内容|
| ---- | ---- | :----: |
|无锁	|对象的hashCode、对象分代年龄、是否是偏向锁（0）|	01 |
|偏向锁   |偏向线程ID、偏向时间戳、对象分代年龄、是否是偏向锁（1）|	01 |
|轻量级锁	|指向栈中锁记录的指针	| 00 |
|重量级锁	|指向互斥量（重量级锁）的指针|	10 |

### spin lock
若当前只有一个等待线程，则该线程通过自旋进行等待。但是当自旋超过一定的次数，或者一个线程在持有锁，一个在自旋，又有第三个来访时，轻量级锁升级为重量级锁。

### fair sync and nonfair sync
ReentrantLock has a Sync class, which implements AQS(AbstractQueuedSynchronizer), add lock and release lock is implemented in Sync. it has subclassed FairSync and NonFairSync. 
ReentrantLock's default lock is NonFairSync, it can also use FairSync by constructor.
the method diff:
``` bash
...
if (!hasQueuedPredecessors() && ..) {
    ... 
}
...
// check if current thread is the first one of the sync queue, return true if it is, other wise return false
public final boolean hasQueuedPredecessors() {
    Node t = tail;
    Node h = head;
    Node s;
    return h != t &&
        ((s = h.next) == null || s.thread != Thread.currentThread());
}
... 
```
### ReentrantLock and NonReentrantLock
可重入锁/递归锁,synchronized 是内置锁，内置锁都是可重入锁， 优点：避免死锁。

## my projects
distributed crawler

## JVM
optimization data sturcture memory model

