---
layout: post
title: Java语言基础18---网络编程
comments: true
date: 2016-09-15 09:52:57
tags:
	- Java
---
网络编程的三要素: IP地址、端口和协议.
<!--more-->
* IP地址
	* IPv4: 4个字节组成, 0 ~ 255 之间, 2011年已经用尽
	* IPv6: 8组, 每组4个16进制数.
* 端口
	* 范围: 0 ~ 65536
	* 端口尽量使用1024之后的, 因为前面的可能会被占用.
	* 常用端口
		* mysql: 3306
		* oracle: 1521
		* web: 80
		* tomcat: 8080
		* QQ: 4000
* 协议 
	* 传输层协议
		* UDP
			* 只管发送, 数据不安全, 速度快
		* TCP
			* 三次握手, 数据安全, 速度略慢
	* 应用层协议
		* HTTP, 它工作在TCP协议之上的.
			* 超文本传输协议
			* 常用方法
				* POST
				* GET
				* HEAD

### Socket
网络上具有唯一标识的IP地址和端口号组合在一起才能构造唯一能识别的标识符套接字.


## <font color=orange>UDP</font>
DatagramSocket表示用来发送和接收数据报包的套接字。DatagramPacket,此类表示数据报包。 


	Thread receive = new Thread() {
		public void run() {
			try {
				DatagramSocket socket = new DatagramSocket(8010);
				DatagramPacket packet = new DatagramPacket(new byte[1024], 1024);
				while (true) {						
					socket.receive(packet);
					System.out.println(new String(packet.getData(), 0, packet.getLength()));
					}
			} catch (Exception e) {
				e.printStackTrace();
			}
		};
	};
	Thread send = new Thread() {
		public void run() {
			try {
				DatagramSocket socket = new DatagramSocket();
				Scanner sc = new Scanner(System.in);
				while (true) {
					String data = sc.nextLine();
					if ("quit".equals(data)) {
						break;
					}
					DatagramPacket packet = new DatagramPacket(data.getBytes(), data.getBytes().length, InetAddress.getByName("127.0.0.1"), 8010);
					socket.send(packet);
				}
				socket.close();
			} catch (Exception e) {
				e.printStackTrace();
			}
		};
	};
		
	receive.start();
	send.start();

## <font color=orange>TCP</font>
* 客户端
	* 通过Socket连接服务端(指定ip地址, 端口号), 通过ip地址找到服务端
		* socket的close()方法, 会自动关闭IO流.
	* 调用Socket的getInputStream()和getOutputStream()的方法可以获取和服务端相连的IO流.
	* 输入流可以读取服务端输出流写出的数据
	* 输出端可以写出数据到服务器的输入流.
* 服务端
	* 创建ServerSocket(需要指定端口号)
	* 调用ServerSocket的accept()方法接口一个客户端请求, 得到一个Socket
	* 调用Socket的getInputStream()和getOutputStream()的方法可以获取和客户端相连的IO流.
	* 输入流可以读取客户端输出流写出的数据.
	* 输出流可以写出数据到客户端的输入流.


* 客户端键盘录入, 发给服务端. 服务端收到字符串, 反转后发送给客户端.


	//服务端
	public static void main(String[] args) throws IOException {
		ServerSocket server = new ServerSocket(12222);
		while (true) {
			final Socket socket = server.accept();
			//多线程
			new Thread() {
				public void run() {
					while (true) {
						try {
							BufferedReader br = new BufferedReader(new InputStreamReader(
									socket.getInputStream()));
							PrintStream ps = new PrintStream(socket.getOutputStream());
							String message = br.readLine();
							System.out.println("服务端收到消息: " + message);
							String sendMessage = new StringBuffer(message).reverse().toString();
							ps.println(sendMessage);
							System.out.println("服务端发送消息: " + sendMessage);
						} catch (Exception e) {
							e.printStackTrace();	
							break;
						} 
					}
				};
			}.start();
		}
	}

	//客户端
	public static void main(String[] args) throws IOException {
		Socket socket = new Socket("127.0.0.1", 12222);
		
		BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
		PrintStream ps = new PrintStream(socket.getOutputStream());
		
		Scanner sc = new Scanner(System.in);
		while (true) {
			String message = sc.nextLine();
			System.out.println("客户端发送消息: " + message);
			ps.println(message);
			System.out.println("客户端收到信息: " + br.readLine());
		}
	}

* 客户端上传文件到服务端


	//客户端	
	public class FileUpload_Client {
		/**
		 * 1. 提示输入文件路径，　并判断
	 	* 2. 发送文件名称到服务器
	 	* 6. 接受结果, 如果存在给予提示, 程序退出
	 	* 7. 如果不存在, 上传文件
	 	* @throws IOException 
	 	* @throws UnknownHostException 
	 	*/
		public static void main(String[] args) throws UnknownHostException, IOException {
			File file = getFile();
			Socket socket = new Socket("127.0.0.1", 23232);
			
			BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
			PrintStream ps = new PrintStream(socket.getOutputStream());
		
			System.out.println("上传文件名称到服务器: " + file.getName());
			ps.println(file.getName());
		
			//判断是否存在
			if ("false".equals(br.readLine())) {
				System.out.println("你上传的文件已经存在!");
				socket.close();
				System.exit(0);
			}
		
			//发送数据
			BufferedInputStream bis = new BufferedInputStream(new FileInputStream(file));
			int length;
			while ((length = bis.read()) != -1) {
				ps.write(length); //传到服务器
			}
		
			bis.close();
			socket.close();
			
		}
	
		public static File getFile() {
			Scanner sc = new Scanner(System.in);
			System.out.println("请输入一个文件路径: ");
		
			while (true) {
				String name = sc.nextLine();
				File file = new File(name);
				if (!file.exists()) {
					System.out.println("你输入的文件路径不存在, 请重新输入!");
				} else if (file.isDirectory()) {
					System.out.println("你输入的是文件夹路径, 请重新输入!");				
				} else {
					return file;
				}
			}
		}
	}


	//服务端
	public class FileUpload_Server {

		/**
		 * 3. 建立多线程
		* 4. 读取文件名
		 * 5. 判断文件是否存在, 将结果发给客户端
		 * 6. 接受文件
	 	* @throws IOException 
		 */
		public static void main(String[] args) throws IOException {
			ServerSocket server = new ServerSocket(23232);
		
			while (true) {
				final Socket socket = server.accept();
				new Thread() {
					public void run() {
						try {
							InputStream inputStream = socket.getInputStream();
							BufferedReader br = new BufferedReader(new InputStreamReader(inputStream));
							PrintStream ps = new PrintStream(socket.getOutputStream());
						
							File dir = new File("upload");
							dir.mkdir();
							File file = new File(dir,br.readLine());
							System.out.println(file);
							if (!file.exists()) {
								ps.println(true);
							} else {
								ps.println(false);
								socket.close();
							}
						
							//写的文件
							BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(file));
						
							byte[] arr= new byte[8192];
							int length;	
							while ((length = inputStream.read(arr)) != -1) {
								bos.write(arr, 0, length);
							}
							System.out.println("读写完成!!!!");
						
							bos.close();
							socket.close();
						
						} catch (IOException e) {
							e.printStackTrace();
						}	
					};
				}.start();
			}
		}
	}
