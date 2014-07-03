

系统学习编程的笔记


2014.6.30

1.  detach , join的问题
 
detach表示线程结束后由系统自动回收。主进程不再管。
join表示主进程一直等待直到线程结束。
 
一个线程必须pthread_join或者pthread_detach，否则内存泄露。
 
如果想创建一个不需要管的线程，
pthread_attr_setdetachstate(atrr, PTHREAD_CREATE_DETACHABLE) 后创建线程就无需担心了
 PTHREAD_CREATE_JOINABLE ，就需要父进程等待了
 
2.  pthread_key_t 线程私有数据
 
线程私有数据，线程之间是相互不干扰的
 
pthread_key_t key
pthread_key_create(&key, NULL)
 
线程中就可以使用 pthread_key_setspecific  pthread_key_getspecific 来完成设置和获取了
 
注意的是： key显然是个每个线程都有的一个特性，例如errno之类。实际上errno就是这么设计的。
 
3.  once 线程强制执行一次
 
能够保证初始化，或者特定动作只会被执行一遍。 （例如单例的初始化）
pthread_once_t once=Pthread_ONCE_INIT
 
pthread_once(once,  once_func)

4.  mutex cond
 
mutex +cond
 
mutex 互斥锁， 确保只有一个线程处理代码块
pthread_mutex_init
pthread_mutex_lock()
pthread_mutex_unlock()
 
 
cond
 
pthread_cond_t
pthread_mutex_lock()
 
while(gcond){  /// 条件满足，挂起，mutex释放
       pthread_cond_wait(&cond, &mutex);
}
//条件不满足了，继续走，mutext也恢复
 
pthread_mutex_unlock()
 
 
pthread_mutex_lock()
//处理gcond
pthread_cond_broadcast()
pthread_mutex_unlock()
 
 
5. mutex + cond 能多个线程协作完成任务

6. sigaction(sig, struct sigaction *, NULL)
完成信号的处理



