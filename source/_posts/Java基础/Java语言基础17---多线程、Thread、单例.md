---
layout: post
title: Java语言基础17---多线程、Thread、单例
comments: true
date: 2016-09-10 09:36:03
tags:
	- Java
---

线程是执行程序的路径, 一个进程中可以包含多天进程. 多线程并发执行可以提高程序的效率.

<!--more-->

JVM的启动是多线程的, 至少启动了垃圾回收线程和主线程.
* 并行
	* 多个任务同时执行, 需要多核CPU
* 并发
	* 多个任务需要运行, 而同时只运行一个, CPU来回切换任务


## <font color=orange>Thread</font>
每个线程都有一个优先级，高优先级线程的执行优先于低优先级线程。
创建新执行线程有两种方法, 都可以使用匿名内部类来实现.
* 将类声明为 Thread 的子类。该子类应重写 Thread 类的 run 方法。然后子类对象调用<font color=red>start()</font>方法开启线程.
* 声明实现 Runnable 接口的类。该类然后实现 run 方法。通过Thread的构造方法传入runnable对象, 调用Thread对象的<font color=red>start()</font>方法.
* 实现Callable接口, 实现call()方法, 不常用


	//方式一: 继承于Thread
	class FirstThread extends Thread {
		@Override
		public void run() {
			while (true) {
			System.out.println("2");
			}
		}
	}
	//方式二: 实现接口Runnable, 实现run()方法
	class TwoThread implements Runnable {
		@Override
		public void run() {
			while (true) {
				System.out.println("1");
			}
		}	
	}
	public static void main(String[] args) {
		//示范1
		FirstThread thread1 = new FirstThread();
		thread1.start();
		
		//示范2
		Thread thread2 = new Thread(new TwoThread());
		thread2.start();
	}

* 两种方式的区别
	* 源码的区别
		* 继承: 当调用start()时直接调子类的run()方法
		* 实现Runnable: Thread构造函数传入Runnable对象, 使用成员变量指向Runnable对象, start()方法调用run()方法时, 判断Runnable对象是否为空, 不为空调用Runnable的run()方法.
	* 使用的区别
		* 继承Thread
			* 好处: 可以直接使用Thread类的方法, 代码简单
			* 弊端: 如果已经有父类, 就不能使用这种方法
		* 实现Runnable接口
			* 好处: 即使已经有父类, 也可以实现这个接口, 而且接口是多实现的.
			* 弊端: 不能直接使用Thread里面的方法.需要先获取线程对象后, 才能得到Thread的方法.


	//使用匿名内部类来实现
	//方式1: 继承
	new Thread() {
		@Override
		public void run() {
			while (true) {
				System.out.println(1);
			}
		}
	}.start();
	//方式2: 接口
	new Thread(new Runnable() {
		public void run() {
			while (true) {
				System.out.println(2);
			}
		}
	}).start();

### Thread常用方法
* void start()使该线程开始执行；Java 虚拟机调用该线程的 run 方法。 	
	* 一个线程多次start是非法的.
* String getName() 返回该线程的名称。
* void SetName(String name) 改变线程名称
* static Thread currentThread() 返回对当前正在执行的线程对象的引用。 
* static void sleep(long millis, int nanos) 在指定的毫秒数加指定的纳秒数内让当前正在执行的线程休眠（暂停执行），此操作受到系统计时器和调度程序精度和准确性的影响。 
* void setDaemon(boolean on) true将该线程标记为守护线程或用户线程。 当其他非守护线程都执行完成后, 自动退出.
* join() 当前线程暂停, 等待指定的线程执行结束后, 当前线程才继续执行.
	* join(long millis) 最多等待毫秒值, 之后交替执行
	* jion(long millis, int nanos) 最多等待毫秒值+纳秒值, 之后交替执行
* static void yield() 暂停当前正在执行的线程对象，并执行其他线程 
* void setPriority(int newPriority) 更改线程的优先级。 范围为1~10
	* public final static int MIN_PRIORITY = 1 最小为1
	* public final static int NORM_PRIORITY = 5 默认为5
	* public final static int MAX_PRIORITY = 10 最大为10


### 同步代码块和同步方法
* 使用synchronized关键字加上一个锁对象来定义一段代码, 这就叫同步代码块.它可以保证在代码块执行时CPU不会切换线程.
* 多个同步代码块如果使用相同的锁对象(对象地址相同), 那么他们就是同步的.
* * 如果两段代码是同步的, 那么同一时间只能执行一段, 在一段代码没执行结束之前, 不会执行另外一段代码.
* 使用synchronized关键字修饰一个方法, 该方法中所有的代码都是同步的.
	* 非静态同步函数的锁是:this
	* 静态同步函数的锁是:字节码对象class


	class Printer {
		static void p1() {
			synchronized (Printer.class) {
				for (int i = 0; i < 10; i++) {
					System.out.println(i + "a");
				}
			}
		}

		synchronized static void p2() {
			for (int i = 0; i < 10; i++) {
				System.out.println(i + "b");
			}
		}
	}
	//测试
	public static void main(String[] args) {
		new Thread() {
			public void run() {
				Printer.p1();
			}
		}.start();
			
		new Thread() {
			public void run() {
				Printer.p2();
			}
		}.start();
	}


### 线程安全问题
多线程并发操作同一数据时, 就有可能出现线程安全问题. 使用同步技术可以解决这种问题, 把操作数据的代码进行同步(必须保证使用同一个锁对象).

	
	class Ticket extends Thread {
		private static int total = 100;

		public void run() {
			while (true) {
				synchronized (Ticket.class) {
					if (total > 0) {
						try {
							Thread.sleep(1);
						} catch (InterruptedException e) {
							e.printStackTrace();
						}
						System.out.println("这是第" + total-- + "号票");
					} else {
						System.out.println("票已售罄!");
						break;
					}
				}
			}
		}
	}
	//测试
	public static void main(String[] args) {
		new Ticket().start();
		new Ticket().start();
		new Ticket().start();
		new Ticket().start();
	}

* 同步代码块的死锁问题, 如果同步代码块使用相同的锁嵌套就可能出现死锁.
	* 尽量不要嵌套使用

* Collections.synchronized(List/Map/Set) 可以把对应的集合转为线程安全的 

## <font color=orange>单例设计模式</font>
* 懒汉式, 单例的延迟加载
	* 节省空间, 只有在使用时才会创建对象
	* 在多线程使用时有可能会创建多个对象
* 饿汉式
	* 节省时间, 一上来就创建对象


	//懒汉式
	class Singleton {
		//私有化构造方法
		private Singleton() {	}
		private static Singleton s;
		public static Singleton getInstance() {
			if (s == null) {
				s = new Singleton();
			}
			return s;
		}
	}
	//饿汉式
	class Singleton {
		//私有化构造方法
		private Singleton() {	}
	
		private static Singleton s = new Singleton();
	
		public static Singleton getInstance() {
			return s;
		}
	}

	//使用final
	class Singleton {
		//私有化构造方法
		private Singleton() {	}
	
		public static final Singleton shareInstance = new Singleton();

	}

## <font color=orange>Runtime</font>
Runtime类是单例模式, 静态方法getRuntime()可以获取该单例.
* exec(String str) 可以执行字符串命令


	Runtime r = Runtime.getRuntime();
	//r.exec("shutdown -s -t 300");		//300秒后关机
	//r.exec("shutdown -a");				//取消关机

## <font color=orange>Timer</font>
Timer是定时器类, 可以在后台执行任务一次或者多次.
* 常用方法
	* void cancel() 终止此计时器
	* int purge()  从此计时器的任务队列中移除所有已取消的任务。 
	* void schedule(TimerTask task, Date time) 安排在指定的时间执行指定的任务。 
	* void schedule(TimerTask task, Date firstTime, long period) 安排指定的任务在指定的时间开始进行重复的固定延迟执行。 
	* void schedule(TimerTask task, long delay) 安排在指定延迟后执行指定的任务。 
	* void schedule(TimerTask task, long delay, long period) 安排指定的任务从指定的延迟后开始进行重复的固定延迟执行。 
	* void scheduleAtFixedRate(TimerTask task, Date firstTime, long period) 安排指定的任务在指定的时间开始进行重复的固定速率执行。 
	* void scheduleAtFixedRate(TimerTask task, long delay, long period) 安排指定的任务在指定的延迟后开始进行重复的固定速率执行。 

### 线程间通信
多个线程并发执行时, 在默认情况下CPU是随机切换线程的.
* wait()方法可以使当前线程等待
* notify() 随机唤醒一个线程
* notifyAll() 唤醒所有线程
* wait、notify和notifyAll方法必须在同步代码块或者同步方法中执行, 并且使用同步锁对象来调用
* 多个线程之间通信, 需要使用notifyAll()唤醒所有线程, 然后使用while来反复判断条件

### wait和sleep的区别
* sleep必须传入时间参数, 时间到了自动唤醒
* wait可以传入参数, 也可以传入参数.
	* 传入时间, 就是在时间之后等待
* sleep在同步函数或者同步代码块中, 不释放锁
* wait方法在同步函数或者同步代码块中, 释放锁


## ReentrantLock互斥锁
JDK1.5新增的一个可重入的互斥锁 Lock，它具有与使用synchronized 方法和语句所访问的隐式监视器锁相同的一些基本行为和语义，但功能更强大。
* void unlock() 释放此锁 
* boolean isLocked() 查询此锁是否由任意线程保持。 
* void lock() 获取锁 
Condition newCondition() 返回用来与此 Lock 实例一起使用的 Condition 实例。 
	* Condition 将 Object 监视器方法（wait、notify 和 notifyAll）分解成截然不同的对象，以便通过将这些对象与任意 Lock 实现组合使用，为每个对象提供多个等待 set（wait-set）。其中，Lock 替代了 synchronized 方法和语句的使用，Condition 替代了 Object 监视器方法的使用。
	* void await() 造成当前线程在接到信号或被中断之前一直处于等待状态。 
	* void signal() 唤醒一个等待线程。 
 	* void signalAll()  唤醒所有等待线程。 


	class BoundedBuffer {
		final Lock lock = new ReentrantLock();
		final Condition notFull  = lock.newCondition(); 
		final Condition notEmpty = lock.newCondition(); 

		final Object[] items = new Object[100];
		int putptr, takeptr, count;

    	public void put(Object x) throws InterruptedException {
			lock.lock();
     		try {
				while (count == items.length) 
         			notFull.await();
      			items[putptr] = x; 
       			if (++putptr == items.length) 	putptr = 0;
       			++count;
       				notEmpty.signal();
     		} finally {
       			lock.unlock();
     		}
		}

		public Object take() throws InterruptedException {
			lock.lock();
     		try {
       			while (count == 0) 
         			notEmpty.await();
       			Object x = items[takeptr]; 
       			if (++takeptr == items.length) 	
					takeptr = 0;
       			--count;
				notFull.signal();
     			return x;
     		} finally {
				lock.unlock();
     		}
		} 
	}

### ThreadGroup
ThreadGroup是线程组, 它可以对一批线程进行分类管理, 默认情况下: 所有线程都属于主线程组.
* public final ThreadGroup getThreadGroup() 通过线程对象获取他所属于的组, 默认都是main
* void setDaemon(boolean daemon) 更改此线程组的后台程序状态。 
*  void setMaxPriority(int pri) 设置线程组的最高优先级。 

### 线程池
程序启动一个新线程的成本是比较高的, 因为它涉及到要与操作系统进行交互, 而使用线程池很好的提高性能.JDk1.5之前,必须手动实现线程池, 而JDK1.5之后Java内置了线程池.线程池里的每一个线程代码结束后，并不会死亡，而是再次回到线程池中成为空闲状态，等待下一个对象来使用。
JDK5新增了一个Executors工厂类来产生线程池，有如下几个方法
* public static ExecutorService 
* public static ExecutorService newSingleThreadExecutor()
* 这些方法的返回值是ExecutorService对象，该对象表示一个线程池，可以执行Runnable对象或者Callable对象代表的线程。它提供了如下方法
* Future<?> submit(Runnable task)
* <T> Future<T> submit(Callable<T> task)
    * 可以使用Callable和线程池实现多线程

	
	ExecutorService pool = Executors.newFixedThreadPool(2);
	// 可以执行Runnable对象或者Callable对象代表的线程
	pool.submit(new MyRunnable());
	pool.submit(new MyRunnable());

	//结束线程池
	pool.shutdown();


### 工厂思想
* 工厂设计模式
	* 又叫静态工厂方法模式, 定义一个工厂类, 使用静态方法根据不同情况, 返回不同的对象.缺点是如果有新对象或者不同的创建方式, 需要不断修改源码, 不利于后期维护.
* 工厂方法模式
	* 抽象工厂类负责定义创建对象的接口, 具体对象的创建工作由继承抽象工厂的具体类实现.
	* 需要额外的编写代码, 增加工作量

### 适配器设计模式
适配器就是一个抽象类, 实现了监听接口, 所有抽象方法都被重写了, 但是方法都是空实现.
