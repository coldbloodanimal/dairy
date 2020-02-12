## Thread
内容来自线程注释:

线程的常用方法：
public synchronized void start()
使这个线程开始执行，jvm调用这个线程的run方法，结果是两个线程同事运行，一个是调用start的，一个是调用run的，启动一个线程两次是不合法的，特别是一个线程不可以重新启动，如果它的执行没有完成。
     * Causes this thread to begin execution; the Java Virtual Machine
     * calls the <code>run</code> method of this thread.
     * <p>
     * The result is that two threads are running concurrently: the
     * current thread (which returns from the call to the
     * <code>start</code> method) and the other thread (which executes its
     * <code>run</code> method).
     * <p>
     * It is never legal to start a thread more than once.
     * In particular, a thread may not be restarted once it has completed
     * execution.
