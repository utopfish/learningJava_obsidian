## 一、[[IO 模型]]
Unix 有五种 I/O 模型：
- 阻塞式 I/O。
- 非阻塞式 I/O。
- I/O 复用（select 和 poll）。
- 信号驱动式 I/O（SIGIO）。
- 异步 I/O（AIO）。
## 二、[[IO 复用]]

select/poll/epoll 都是 I/O 多路复用的具体实现，**select 出现的最早，之后是 poll，再是 epoll**。