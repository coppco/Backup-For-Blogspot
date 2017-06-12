---
layout: post
title: Java语言基础16---异常、File类、I/O流
comments: true
date: 2016-09-06 14:57:52
tags:
	- Java
---

## <font color=orange>异常</font>
异常就是Java程序在运行过程中出现的错误. main收到异常时, ①自己处理, 如果自己没有处理方式, 那么交给JVM处理, JVM默认处理方式是将该异常的名称、信息等打印在控制台,并且终止程序运行.Throwable 类是 Java 语言中所有错误或异常的超类
* <font color=red>Throwable</font>
	* Error
		* 数据库异常, 服务器宕机等
	* Exception
<!--more-->


### 常用处理方式
* 使用try - catch - finally
	* 自己处理异常
	* 三种形式
		* try - catch
		* try - finally
		* try - catch - finaly
	* try: 用来检测异常, 当发生异常后, try里面的代码不会再执行, 而调到对应的catch里面
	* catch: 用来捕获异常, 可以有多个catch
		* 多个catch时,需要注意顺序, 小类别Exception要放在前面, 大类别Exception放在后面
	* finally: 不管是否有异常最后都会执行, 一般用来释放资源等
		* 特殊情况: 在执行finally之前jvm退出了(如System.exit(0))
		* 在catch里面有return, finally里面语句也会执行
	>	try {
		&#8195;&#8195;int x =10 / 0;
		&#8195;&#8195;int[] arr = {1};
		&#8195;&#8195;arr[10] = 10;
		&#8195;&#8195;arr = null;
		&#8195;&#8195;arr[0] = 10;
		} catch (ArithmeticException e) {//除数为0
		&#8195;&#8195;System.out.println(e);
		} catch (ArrayIndexOutOfBoundsException e) {//越界
	&#8195;&#8195;System.out.println(e);
		} catch (NullPointerException e) {//空指针
		&#8195;&#8195;System.out.println(e);
		} /\*catch (ArrayIndexOutOfBoundsException | NullPointerException e) {//JDK7.0以后可以这样写
		&#8195;&#8195;System.out.println(e);
		}\*/ finally {
		&#8195;&#8195;System.out.println(finally);
		}
* throws
	* 异常交给调用者处理
	* 可以抛出运行时异常RuntimeException
		* 方法里面可以不添加throws + 异常类名
		* 异常类名可以有多个, 用逗号,分开
	* 可以抛出编译期异常Excption
		* 方法中必须添加throws + 异常类名
		* 异常类名可以有多个, 用逗号,分开


	public static void readFile(String fileName) throws FileNotFoundException, IOException {
		FileInputStream fileIn = new FileInputStream(fileName);
		fileIn.close();
	}


### 编译器异常和运行期异常
Java中所有异常被分为两大类
* 运行期异常
	* RuntimeException类及其子类
* 编译期异常
	* 除了运行期异常都是编译期异常
	* 必须处理不然无法通过编译


### 自定义异常
自定义类继承于Exception或者RuntimeException, 通过message来区分什么问题.

## <font color=orange>File类</font>
文件和目录路径名的抽象表示形式。路径有相对路径和绝对路径.
* 构造方法
	* File(File parent, String child) 根据file对象和 child路径名字符串创建一个新 File 实例。 
	* File(String pathname) 通过将给定路径名字符串创建一个新 File 实例。 
		* 可以是相对路径或者绝对路径
	* File(String parent, String child) 根据 parent 路径名字符串和 child 路径名字符串创建一个新 File 实例。 
	* File(URI uri) 通过URI来创建一个新的 File 实例 
	>	File file = new File("D:\\SourceTreeCode\\Backup-For-Blogspot\\source\\_posts");
		System.out.println(file.exists());
* 创建功能
	* boolean createNewFile() 创建文件
		* 文件不存在就创建, 返回true
		* 文件存在就不创建, 返回false
	* boolean mkdir() 创建文件夹
	* boolean mkdirs() 创建多级文件夹 
	>	//创建文件
		File file1 = new File("xx.txt");
		file1.createNewFile();
		//创建文件夹
		File file2 = new File("aaa");
		file2.mkdir();
		//创建多级文件夹
		File file3 = new File("a//b//c");
		file3.mkdirs();
* 重命名和删除
	* boolean renameTo(File dest) 重新命名此路径名表示的文件。
		* 如果同一个目录, 改名
		* 如果不是同一个目录, 改名并剪切
	* boolean delete() 删除此抽象路径名表示的文件或目录。 
		* 删除不走回收站, 慎重
		* 要删除文件, 那么该文件夹里面必须是空的
	* void deleteOnExit() 在虚拟机终止时，请求删除此路径名表示的文件或目录。 
* 判断功能
	* boolean isDirectory() 判断是否是文件夹
	* boolean isFile() 判断是否是文件
	* boolean exists() 判断文件/文件夹是否存在
	* boolean canExecute() 判断出现是否可执行此路径
	* boolean canRead() 判断路径是否可读 
	* boolean canWrite() 判断路径是否可写
	* boolean isHidden() 判断是否是隐藏的。 
* 获取功能
	* public String getAbsolutePath()：获取绝对路径
	* public String getPath():获取路径
		* 获取的是构造方法里面传的路径
		* 可能是绝对路径也可能是相对路径
	* public String getName():获取名称 
		* 获取的是文件/文件夹名称
	* public long length():获取长度。字节数 
	* public long lastModified():获取最后一次的修改时间，毫秒值 
	* public String[] list():获取指定目录下的所有文件或者文件夹的名称数组 
	* public File[] listFiles():获取指定目录下的所有文件或者文件夹的File数组


	//递归获取目录下面的文件
	public static void findFile(File file) throws IOException {
		for (File subFile : file.listFiles()) {
			System.out.println(subFile);
			if (subFile.isFile()) {
				System.out.println(subFile);
				BufferedWriter write = new BufferedWriter(new OutputStreamWriter(new FileOutputStream("fileList.txt", true), "utf-8"));
				write.write(subFile.getAbsolutePath());
				write.newLine();
				write.close();
			} else if (subFile.isDirectory() && !subFile.isHidden()) {
				findFile(subFile);
			}
		}
	}

* 文件过滤器
	* String[] list(FilenameFilter filter) 返回一个字符串数组，这些字符串指定此抽象路径名表示的目录中满足指定过滤器的文件和目录。 
	* File[] listFiles(FileFilter filter) 返回抽象路径名数组，这些路径名表示此抽象路径名表示的目录中满足指定过滤器的文件和目录。 
	* File[] listFiles(FilenameFilter filter)返回抽象路径名数组，这些路径名表示此抽象路径名表示的目录中满足指定过滤器的文件和目录。 
	>	File dir = new File("E:\\");
		File[] subFile = dir.listFiles(new FilenameFilter() {
		&#8195;&#8195;@Override
		&#8195;&#8195;public boolean accept(File dir, String name) {
		&#8195;&#8195;&#8195;&#8195;File file = new File(dir, name);
		&#8195;&#8195;&#8195;&#8195;return file.isFile() && file.getName().endsWith(".exe");
		&#8195;&#8195;}
		});
		for (File file : subFile) {
		&#8195;&#8195;System.out.println(file);
		}

## <font color=orange>IO流</font>
Java中对数据的操作是通过流的方式, Java用于操作流的类都在IO包里面.
* 按照流方向
	* 输入流
	* 输出流
* 按照操作类型
	* 字节流: 字节流可以操作如何数据
		* 抽象父类
			* InputStream: 字节输入流
			* OutputStream: 字节输出流
	* 字符流: 字符流只能操作字符数据, 比较方便
		* 抽象父类
			* Writer: 字符输入流
			* Reader: 字符输出流

## <font color=orange>字节流</font>
read()返回值和write的写入值都是int类型, Java在读写的时候都会处理.
* FileInputStream, 字节输入流
	* int available() 返回下一次对此输入流调用的方法可以不受阻塞地从此输入流读取（或跳过）的估计剩余字节数。 	
	* void close() 关闭此文件输入流并释放与此流有关的所有系统资源。 
	* int read() 读取一个字节流数据,没有读到数据为-1。 
	* int read(byte[] b) 读取字节数组b长度的字节数据, 返回值为读取到的字节个数, 没有读到数据为-1。 


	FileInputStream fis = new FileInputStream("xx.txt");
	int x;
	while ((x = fis.read()) != -1) {
		System.out.println(x);
	}
	fis.close();

* FileOutputStream, 字节输出流
	* 创建的时候, 如果路径不存在则会自动创建文件
	* 构造方法(当append为true时结尾追加内容)
		* FileOutputStream(String name, boolean append) 可以在文件结尾追加内容
		* FileOutputStream(File file, boolean append)	在文件结尾追加内容
	* 构造方法(清空文件内容)
		* FileOutputStream(File file) 初始化时会清空文件内容
		* FileOutputStream(String name) 初始化时会清空文件内容
	* 常用方法
		* void write(byte[] b) 将 b.length 个字节从指定 byte 数组写入此文件输出流中。 
		* void write(byte[] b, int off, int len) 将指定 byte 数组中从偏移量 off 开始的 len 个字节写入此文件输出流。 
		* void write(int b) 将指定字节写入此文件输出流。 
		* close() 具有刷新功能, 在关闭流之前先去刷新缓冲区,把数据写到文件后再关闭.
			* 流关闭后不能再写入
		* flush() 具有刷新功能, 刷完之后还可以继续写入. 

	
	FileOutputStream fos = new FileOutputStream("xx.txt");
	fos.write(97);
	fos.write(33);
	fos.close();

* IO流一般读写操作


	FileInputStream input = new FileInputStream("TheFatRat - Monody.mp3");
	FileOutputStream output = new FileOutputStream("copy.mp3");
	/*
	//一个字节一个字节的读写, 效率慢
	int d;
	while ((d = input.read()) != -1) {
		output.write(d);
	}
	*/
	/*
	//整体读写, 但是不推荐, 因为文件过大可能会内存溢出
	byte[] read = new byte[input.available()];
	input.read(read);
	output.write(read);
	*/
	//可以使用小数组, 如1024*8 来拷贝, 推荐
	byte[] arr = new byte[1024*8];
	int len;
	while ((len = input.read(arr)) != -1) {
		output.write(arr, 0, len);
	}
	/*
	//使用BufferedInputStream和BufferedOutputStream来拷贝, 推荐
	BufferedInputStream bis = new BufferedInputStream(input);
	BufferedOutputStream bos = new BufferedOutputStream(output);
	int b;
	while ((b = bis.read()) != -1) {
		bos.write(b);
	}
	//只关闭封装后的对象即可
	bis.close();
	bos.close();
	*/
	input.close();
	output.close();
	System.out.println("拷贝完成了");

### <font color=orange>BufferedInputStream和BufferedOutputStream</font>
这两个类里面都有一个缓冲区, 大小为8\*1024, BufferedInputStream读的时候从文件一次读取8\*1024个大小字节的数据, 然后写到BufferedOutputStream的缓冲区, 缓冲区写满才一次写到文件里面.这样减少了硬盘读写操作, 而大部分在内存操作, 加快了读写速度.
* 完整读写代码


	//JDK1.6的写法
	public static void copy(String source, String dest) throws IOException {
		FileInputStream in = null;
		FileOutputStream out = null;
		try {
			in = new FileInputStream(source);
			out = new FileOutputStream(dest);
			byte[] arr = new byte[8*1024];
			int len;
			while ((len = in.read(arr)) != -1) {
				out.write(arr,0,len);
			}
			System.out.println("复制完成");
		} finally {
			try {
				if(in != null) {
					in.close();				
				}				
			} finally {
				if(out != null){
					out.close();				
				}				
			}
		}
	}
	
	//JDK1.7新写法, 只有实现了AutoCloseable接口的可以这样写, 在使用后会自动close()方法来关闭流.
	public static void newCopy(String source, String dest) throws IOException {
		try (
				//这里面只能写自动关闭的类, 实现了接口: AutoCloseable
				FileInputStream in = new FileInputStream(source);
				FileOutputStream out = new FileOutputStream(dest);
			){
				byte[] arr = new byte[8 * 1024];
				int len;
				while ((len = in.read(arr)) != -1) {
					out.write(arr,0,len);
				}
				System.out.println("新复制完成");
		}
	}


### 数据的加密
在读写的时候, 改变byte字节. 推荐使用异或同一个int数.



## <font color=orange>字符流</font>
### 字节流和字符流
* 当拷贝文本文件的时候不推荐使用字符流, 因为中间有一个字节-(读)-字符-(写)-字节转换过程.
* 单单操作只读或者只写的文本文件可以使用字符流.
* 除了文本文件, 其他文件使用字节流
* FileReader
	* int read() 读取单个字符。 
	* int read(char[] cbuf) 将字符读入数组。 
	* int read(char[] cbuf, int offset, int length) 将字符读入数组中的某一部分。 
	* boolean ready() 判断此流是否已经准备好用于读取。 
* FileWriter
	* void write(char[] cbuf) 写入字符数组。 
	* void write(char[] cbuf, int off, int len) 写入字符数组的某一部分。 
	* void write(int c) 写入单个字符。 
	* void write(String str) 写入字符串。 
	* void write(String str, int off, int len)  写入字符串的某一部分。
 
	
	FileReader reader = new FileReader("xx.txt");
	FileWriter write = new FileWriter("copy.txt");
	int a;
	while ((a = reader.read()) != -1) {
		write.write(a);
	}
	reader.close();
	write.close();


### BufferedReader和BufferedWriter
和BufferedInputStream和BufferedOutputStream类似.除了常用方法, 也有独有方法.
* BufferedReader
	* String readLine() 通过下列字符之一即可认为某行已终止：换行 ('\n')、回车 ('\r') 或回车后直接跟着换行。
		* 如果已经到达流的末尾返回null, 否则返回这一行内容
* BufferedWriter
	* newLine() 写入一个行分隔符。行分隔符字符串由系统属性 line.separator 定义,  它是跨平台的，并且不一定是单个新行 ('\n') 符。 


	BufferedReader reader = new BufferedReader(new FileReader("xx.txt"));
	BufferedWriter writer = new BufferedWriter(new FileWriter("copy.txt"));
		
	String st;
	while ((st = reader.readLine()) != null) {
		writer.write(st);
		writer.newLine();
	}
		
	reader.close();
	writer.close();

### LineNumberReader
BufferedReader的子类, 跟踪行号的缓冲字符输入流。此类定义了方法 setLineNumber(int) 和 getLineNumber()，它们可分别用于设置和获取当前行号。默认情况下，行编号从 0 开始。该行号随数据读取在每个行结束符处递增，并且可以通过调用 setLineNumber(int) 更改行号。但要注意的是，setLineNumber(int) 不会实际更改流中的当前位置；它只更改将由 getLineNumber() 返回的值。 
内部封装了一个lineNumber 默认为0 , 每readLine()一次, lineNumber++.

### 使用指定码表读写文本
Java中的文本默认使用GBK码表, 如果一个utf-8编码的文本使用默认码表来读, 就会出现乱码.
InputSteamReader和OutputStreamReader可以指定用什么码表来将字节流转为字符流.当然也可以把InputSteamReader和OutputStreamReader封装成BufferedReader和BufferedWriter.

	//默认码表是gbk, 如果是文本编码是gbk, 可以省略.大小可以忽略.
	InputStreamReader reader = new InputStreamReader(new FileInputStream("utf-8.txt"), "utf-8");
	OutputStreamWriter writer = new OutputStreamWriter(new FileOutputStream("gbk.txt"), "gbk");
		
	int d;
	while ((d = reader.read()) != -1) {
		writer.write(d);
	}
		
	reader.close();
	writer.close();

	//获取目录下面的文件列表
	public static void findFile(File file) throws IOException {
		for (File subFile : file.listFiles()) {
			System.out.println(subFile);
			if (subFile.isFile()) {
				System.out.println(subFile);
				BufferedWriter write = new BufferedWriter(new OutputStreamWriter(new FileOutputStream("fileList.txt", true), "utf-8"));
				write.write(subFile.getAbsolutePath());
				write.newLine();
				write.close();
			} else if (subFile.isDirectory() && !subFile.isHidden()) {
				findFile(subFile);
			}
		}
	}


### 序列流
序列流可以把多个字节输入流整合成一个, 从序列流中读取数据时, 将从被整合的第一个流开始读, 读完一个之后继续读第二个, 以此类推.
* 整合两个

	>	FileInputStream fi1 = new FileInputStream("1.txt");
	FileInputStream fi2 = new FileInputStream("2.txt");
	SequenceInputStream sequence = new SequenceInputStream(fi1, fi2);
* 整合多个使用集合
	
	>	FileInputStream fi1 = new FileInputStream("1.txt");
	FileInputStream fi2 = new FileInputStream("2.txt");
	FileInputStream fi3 = new FileInputStream("3.txt");
	Vector<FileInputStream> v = new Vector<>();
	v.add(fi1);
	v.add(fi2);
	v.add(fi3);
	SequenceInputStream s = new SequenceInputStream(v.elements());
		
### 内存输出流
ByteArrayOutputStream该输出流可以向内存中写数据, 把内存当作一个缓冲区, 写出之后可以一次性获取出所有数据
* write() 可以把数据写到内存中
* toByteArray() 转为字节数组
* toString() 转为字符串


	FileInputStream in = new FileInputStream("zhongwen.txt");;
		
	byte[] arr = new byte[5];
	int d;
	ByteArrayOutputStream out = new ByteArrayOutputStream();
	while ((d = in.read(arr)) != -1) {
		out.write(arr, 0, d);
	}
	System.out.println(out.toString());
	in.close();

### 对象操作流
ObjectOutputStream和ObjectInputStream可以把对象序列化和反序列化.Object类需要实现Serializable接口.
读的时候可能遇到问题, 不知道有几个, 那么可以通过写的时候放在集合里面, 把集合写入, 这样读的时候也是读取集合.

	Person p = new Person("张三", 24);
		//写
		ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("Person.txt"));
		out.writeObject(p);
		out.close();
		//读
		ObjectInputStream in = new ObjectInputStream(new FileInputStream("Person.txt"));
		//每readObject()一次就会得到一个对象.
		Object o = in.readObject();
		in.close();
		if (o instanceof Person) {
			Person newP = (Person)o;
			System.out.println(newP.getName());
		}

### 标准输入流和标准输出流
System.in是InputStream, 标准输入流, 默认可以从键盘输入读取字节数据.
System.out是PrintStream, 标准输出流, 默认可以向控制台输出字符和字节数据
* System.setIn(InputStream) 修改输入流
* System.setOut(PrintStream) 修改输出流


### 随机访问流
RandomAccessFile既可以读流也可以写流.
* 构造方法里面有个mode
	* r: 以只读方式打开。调用结果对象的任何 write 方法都将导致抛出 IOException。
	* rw: 打开以便读取和写入。如果该文件尚不存在，则尝试创建该文件。 
	* rws: 
	* rwd: 
* void seek(long pos) 设置到此文件开头测量到的文件指针偏移量，在该位置发生下一个读取或写入操作。 

### Properties配置文件
Properties是一个Map集合.它可以使用put(key, value)、get(key)、getProperty(key)、setProperty(key, value)等方法.也有特有的方法.
* load()方法可以从一个输入流获取数据, 文件中使用=或者:来描述键值对
	* void load(InputStream inStream) 从输入流中读取属性列表（键和元素对）。 
	* void load(Reader reader) 按简单的面向行的格式从输入字符流中读取属性列表（键和元素对）。
* store() 可以把数据写入到输出流中, conments是属性列表描述
	* void store(OutputStream out, String comments) 以适合使用 load(InputStream) 方法加载到 Properties 表中的格式，将此 Properties 表中的属性列表（键和元素对）写入输出流。 
	* void store(Writer writer, String comments) 以适合使用 load(Reader) 方法的格式，将此 Properties 表中的属性列表（键和元素对）写入输出字符。 
	* void storeToXML(OutputStream os, String comment) 发出一个表示此表中包含的所有属性的 XML 文档。 
	* void storeToXML(OutputStream os, String comment, String encoding) 使用指定的编码发出一个表示此表中包含的所有属性的 XML 文档 
