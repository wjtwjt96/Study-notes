### Linux

#### 1. Linux目录结构

```java
home：普通用户的根目录；普通用户的文件都在这个目录下
root：超级管理员的根目录；
etc： 存放系统的配置文件
usr： 存放系统的共享资源
```

#### 2. linux常用命令

##### 1. 切换目录

```java
cd  文件名				  //切换到该文件
cd  文件名/文件名			//切换到文件下的文件
cd  ..  				//切换到上级目录
cd   /  				//切换到根目录
cd   ~ 					//切换到home目录
cd						//切换到上一个目录
```

##### 2. 查看帮助文档

```
man  命令
退出帮助文档    q
```

##### 3. 创建文件和删除文件

###### 创建文件夹

```java
mkdir  文件夹         			//创建目录
mkdir  -p  文件夹/文件夹   	  //创建层级目录     
```

######  删除文件夹   

```
rmdir  目录名
```

##### 4. 查看文件目录

   ```java
ls     			//查看所有目录和文件（可看的文件）
ls  -a  		//查看所有文件
ls  -l    		//查看所有文件的详细信息    
	缩写为：ll 
   ```

##### 5. 浏览文件内容

######  1. cat    

```java
cat  文件名    //查看文件所有内容（不能翻页）
```

###### 2. more  

```java
more  文件名    //查看文件内容（可以翻页）
空格键   向下翻页
回车键   向下显示一行
```

###### 3. less 

```java
less  文件名     //查看文件内容（可以上下翻页）
page  up        //向上翻页
page down       //向下翻页
按键q退出查看
```

###### 4. tail

```java
tail   -条数   文件名   		//查看文件的最后几条内容
tail   -f      文件名  	  //动态查看文件内容
ctrl + c 退出查看
```


##### 6. 文件的操作

###### 1. 创建文件

```java
touch  文件名
eg：touch  a.txt     //创建一个空白的文件
```

###### 2. 删除文件

```java
rm   文件名            //带询问的删除
rm   -f   文件名       //不带询问的删除
rm   -r   文件名       //带询问的递归删除
rm  -rf   文件名       //不带询问的递归删除 
```

###### 3. 移动文件（重命名）

```java
mv  文件名   新文件名       //重命名
mv   文件   目录/文件名     //移动到某目录下为  什么名称
```

###### 4. 复制文件

```java
cp  文件  目录/文件名   
eg:
cp  1.txt   a/1.txt    //copy到指定目录
cp  1.txt   2.txt      //copy当前目录
```

###### 5. 打包或解压文件或目录

1. 常用参数：

   ```java
   -c：创建一个新文件
   
   -v：显示运行过程的信息
   
   -f：指定文件名
   
   -z：调用压缩命令进行压缩
   
   -t：查看压缩文件的内容
   
   -x：解开文件
   ```

2. 常用参数的组合

   ```java
    -cvf     	//打包一个文件或目录为tar
   
    -zcvf   	//打包并压缩一个文件和目录
   
    -xvf    	//解压或打开一个tar文件
   ```

3.  压缩、打包 和 解压

   - 打包（或压缩）：**tar**   参数  文件名   要打包|解压的文件目录

     eg： tar  -cvf   test.tar   ./*

   - 解压：**tar**   参数  解压的文件名  -C   解压的目录

     将压缩包解压到指定目录

##### 7. 查找符合条件的字符串

```java
grep   字符串				  		 	 //查找字符串
grep   字符串   -color      			  //高亮显示查找的内容
grep   字符串   -color  -A条数   		//查找内容的后几行也显示
grep   字符串   -color  -B条数   		//查找内容的前几行也显示
```

##### 8. 其他常见命令

```java
pwd   显示当前所在的目录
wget  下载资料
    
eg：wget  资源路径
```

#### 3. 文本编辑器

vim 和 vi 编辑器：编辑普通文档

**三种模式：命令行、插入、底行模式。**

- 切换到命令行模式：按Esc键；

- 切换到插入模式：按 i 、o、a键；

  ​    i 	在当前位置前插入

  ​    I 	在当前行首插入

  ​    a 	在当前位置后插入

  ​    A	 在当前行尾插入

  ​    o 	在当前行之后插入一行

  ​    O 	在当前行之前插入一行 

- 切换到底行模式（在命令行模式下）：按 “：”；	

- 常用的操作

  1. 打开文件：vim file
  2. 退出：esc  : q
  3. 修改文件：输入i进入插入模式
  4. 保存并退出：esc : wq（保存退出）
  5. 不保存退出：esc : q!（强制退出）

#### 4. 重定向输出

将输出操作重定向到文件中（或者其他）

例子： cat   a.txt  >  b.txt   将a.txt的文件内容读取到b.txt中（会覆盖b.txt的文件内容）

​			cat   a.txt  >>  b.txt   将a.txt的文件内容读取到b.txt中（会追加到b.txt的文件内容中）

#### 5.  &&命令与操作

​	就是多条命令通过&&连接起来执行	

#### 6. 管道

​	将一个命令的输出作为另一个命令的输入

​	例子： ifconfig  |  grep  192.168    在ifconfig的输出中去查找192.168

#### 7. Linux高级命令

##### 1. 查看所有进程

```java
ps  -ef   

备注：一般和管道使用
```

##### 2. 系统管理命令

###### 1. date    显示系统时间

```java
date  -s  "时间"      //设置时间
```

###### 2. df       显示磁盘信息

```
df -h        ///友好显示
```

###### 3.free     显示内存状态

```
free  -m  
```

###### 4. top    任务管理器

​	按q退出

###### 5. clear   清屏

​	ctrl + L

###### 6. ps                     显示所有进程

```java
ps -ef   				//查看所有进程
ps -ef | grep ssh   	 //查找某一进程
```

###### 7. kill  pid           杀掉某一进程

````java
kill -9  pid     //强制杀掉
````

###### 8.  du                   显示文件或目录的大小

###### 9.  who               显示当前登陆的用户

###### 10. hostname   查看当前主机名

###### 11. uname         显示系统信息

```java
uname  -a     //显示的详细信息
```

#### 8. 网络管理

1. ###### ifconfig：查看所有网络设置

   ```java
   ifconfig   网卡名称  down    	//禁用网卡
   ifconfig   网卡名称  up    		//启用网卡
   ```

2. ###### ping  ：和windows一样

   ctrl+c停止

3. ###### 查看网络端口：

   ```java
linux：  netstat  -an
   windows：netstat  -ano
   ```

#### 9. 用户管理

1. ##### 添加用户

   ```java
   useradd  用户名称      				//添加用户
   
   passwd  回车输入密码
   
   useradd  用户名  -d  /home/目录：    //创建一个用户到指定目录
   ```

2. ##### 删除用户

   ```java
   userdel  用户名  		//只删除用户，不删除家目录
   userdel -r 用户名 		//删除用户，和删除家目录
   ```

3. ##### 切换目录

   ```
   1. ssh  -l  用户名称  -p  22  主机
      例如:  ssh -l tom 22  192.168.13.23
   ```

4. ##### 切换账户

   ```java
   su  用户名   //切换账户
   ```

#### 10. 组管理

1. ##### 添加组：

   ```java
   groupadd 组名         		//添加组
   useradd  用户名  -g  组名	  //将用户放入组中
   ```

2. ##### 删除组

   ```java
   groupdel   组名        //删除组
   ```

3. ##### id  su名称

   ```java
   id   		 		//查看用户的组信息
   su 用户名   		  //切换用户
   ```

#### 11. 文件的权限

##### 1. 修改文件的权限

1. ###### Linux三种文件类型：

   ```
   普通文件： 包括文本文件、数据文件、可执行的二进制程序文件等。 
   目录文件： Linux系统把目录看成是一种特殊的文件，利用它构成文件系统的树型结构。  
   设备文件： Linux系统把每一个设备都看成是一个文件
   ```

   **备注：以 d 开始的是目录文件；以 开头的是普通文件；**

2. ###### 文件权限：9个字母，三个字母一组

   ```
   第一组代表当前用户的权限
   第二组代表当前组的权限
   第三组代表其他用户的权限
   r：可读（4）
   w：可写（2）
   x：可执行（1）
   ```

   **注意：用数字对应相应权限：7代表所有权限**

3. ###### 修改权限

   ```
   chmod  755  文件名
   chmod  u=,g=,o=   文件名
   ```

##### 2. 修改文件的归属

​	chown 变更文件或目录改文件所属用户和组

```java
chown 	用户:组    文件名    	//变更文件所属用户和组
chown  -R  用户:组  目录 	//变更目录下所有文件的所属
```

#### 12. rpm包管理

```java
rpm  -ga  				//查询所有安装的包
rpm  -e   --nodeps   	 //要卸载的包
rpm  -ivh  要安装的包     //安装 
```

#### 安装jdk案例：

1. 查看linux下是否有jdk
   
   ```java
   java   -version
   ```
   
2. 检测是否安装了jdk
   
   ```java
   rpm -qa | grep jdk
   ```
   
3. 如果有就卸载
   
   ```
   rpm  -e  --nodeps  包名
   ```
   
4. 没有就上传jdk的linux安装包
   
5. 创建java文件夹
   
   ```
   mkdir  java
   ```
   
6. 将安装包 从 root 目录下 复制到 /usr/local/ 下，并解压
   
   ```
   cp  包名  /user/local
   ```

7. 安装依赖

   ```
   yum  install  glibc.i686
   ```

8. 配置环境变量

   ```java
   1. vi  /etc/profile
   
   2. 在末尾行添加
   
      #set Java environment
      //jdk解压路径
      JAVA_HOME=/usr/local/src/java
      CLASSPATH=.:$JAVA_HOME/lib.tools.jar
      PATH=$PATH:$JAVA_HOME/bin
      //导入环境变量
      export  JAVA_HOME  CLASSPATH  PATH
   
   3. 保存退出
   4. 重新加载配置文件
      source  /etc/profile    //使更改的配置立即生效        
   ```

#### 安装mysql:

```
1. 查看mysql的服务状态  service  mysql  status
  启动mysql：service  mysql  start 
  停止mysql：service  mysql  stop
2. 修改密码
  update user   set password = password("123456")  where user = 'root';
  flush  privileges;
3. 刷新修改：
  flush  privileges;
4. 开启远程访问
  grant all privileges  on \*.\*  to 'root'  @'%'  identified   by '123456';
  flush  privileges;
5. 打开服务器防火墙端口 3306
  1. 3306端口放行
     /sbin/iptables   -I   INPUT   -p  tcp   --dport  3306 -j  ACCEPT
  2. 将该设置写入到防火墙规则中
     /etc/rc.d/init.d/iptables  save
6. 设置mysql服务开机启动

  1. 加入到系统服务
     chkconfig   --add  mysql;
  2. 自动启动
     chkconfig   mysql  on;
```

#### 安装tomcat

```
1. 创建文件夹
2. 复制到文件夹
3. 解压
4. 运行
     方式1：  sh  startup.sh
     方式2：  ./startup.sh
5. 开启tomcat端口防火墙

注意：动态查看tomcat日志： tail  -f  ../logs/catalina.out
	 退出 ctrl + c
```

#### 备份数据库：

```
1. 备份：mysqldump  -uroot -p123456  数据库名  >  备份地址（如：d:/1.sql）
2. 还原：source   数据库文件
```



### Nginx

Nginx是一款[轻量级](https://baike.baidu.com/item/轻量级/10002835)的[Web](https://baike.baidu.com/item/Web/150564) 服务器/[反向代理](https://baike.baidu.com/item/反向代理/7793488)服务器及[电子邮件](https://baike.baidu.com/item/电子邮件/111106)（IMAP/POP3）代理服务器;

其特点是占有内存少，[并发](https://baike.baidu.com/item/并发/11024806)能力强，事实上nginx的并发能力确实在同类型的网页服务器中表现较好;

1. ##### Nginx特点

   反向代理，负载均衡  动静分离

2. ##### 反向代理：

   - **正向代理**：就是一个服务器来代理用户客户端向目标服务器发送请求；

     ![img](https://images2018.cnblogs.com/blog/466768/201804/466768-20180403082035578-1377801350.png)

   - **反向代理**：反向代理是指，用nginx服务器来代理目标服务器，客户端访问只能访问到nginx服务器，然后nginx分发请求到目标服务器，再接收服务器请求返回给客户端；

     ![img](C:\Users\99726\Desktop\Study-notes\pic\反向代理.png)

3. ##### 负载均衡

   负载均衡是指通过nginx服务器同时接收多个请求，分发给同一集群下不同的服务器来处理，达到一个均衡服务器的压力；

   **原理：**就是将数据流量分摊到多个服务器上执行，减轻每台服务器的压力，多台服务器共同完成工作任务，从而提高数据的吞吐量。

4. ##### 动静分离

   将静态的资源和动态的资源，分别放在不同的服务器上，节省访问时间。一般将静态资源放到反向代理服务器上，动态资源放在目标服务器上；