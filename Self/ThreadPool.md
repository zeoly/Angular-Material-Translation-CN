# 线程池

## Executors工具类中的线程池

- `newSingleThreadExecutor`
  - 单线程的线程池，可用来保证任务的顺序执行
  - 由new ThreadPoolExecutor(1, 1, 0L, TimeUnit.MILLISECONDS, new LinkedBlockingQueue<Runnable>())构造
  - 即核心线程和最大线程都强制为1，并且线程不回收
- `newFixedThreadPool`
  - 定长线程池，可用来控制并发数
  - 由new ThreadPoolExecutor(nThreads, nThreads, 0L, TimeUnit.MILLISECONDS, new LinkedBlockingQueue<Runnable>())构造
  - 即线程数固定为n，同上
- `newScheduledThreadPool`
  - 定时/延时线程池，可用来做定时任务或超时检测等等

- `newCachedThreadPool`
  - 缓存线程池，一个灵活的线程池，可自动扩展和收缩
  - 由new ThreadPoolExecutor(0, Integer.MAX_VALUE, 60L, TimeUnit.MILLISECONDS, new SynchronousQueue<Runnable>())构造
  - 初始核心线程数为0，最大数量较大，60s回收
  - 使用非公平同步队列，内部实现为`TransferStack<E>`

## 阻塞队列

- `ArrayBlockingQueue`
  - 内部实现为数组结构，需指定队列大小
  - 循环使用
- `LinkedBlockingQueue`
  - 内部实现为链表结构
  - 默认大小为Integer.MAX_VALUE，当队列满时，阻塞生产线程
- `LinkedTransferQueue`
- `PriorityBlockingQueue`
- `SynchronousQueue` 同步队列
  - 生产和消费均为阻塞式，即生产线程在没有被消费时，一直被阻塞；消费线程无可消费的对象时，也会一直阻塞
  - 分为`TransferQueue<E>`和`TransferStack<E>`
- `DelayQueue`

## Spring中的线程池

``

- int `corePoolSize` 核心线程数
- int `maxPoolSize` 最大线程数
- int `keepAliveSeconds` 保持活跃时间
- int `queueCapacity` 等待队列的长度