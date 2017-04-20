# 新手搭建java环境

## 一、环境搭建

__ 1、安装并配置jdk __

	JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home
	PATH=$JAVA_HOME/bin:$PATH
	export PATH

测试命令：`echo $JAVA_HOME`


__ 2、安装并配置apache-maven __

- 在~/目录下新建.m2文件夹，在~/.m2/目录下新建repository文件夹
- 将下载的apache-maven/conf/settings.xml复制到~/.m2/目录中
- 配置环境变量，参考：http://www.jianshu.com/p/191685a33786


__ 3、安装apache-tomcat __

- 遇到问题
	- 问题：Cannot run program。。。 Permission denied  
	- 解决：命令行进入tomcat的bin根目录，执行chmod 777 *.sh问题解决
	- 参考：http://blog.csdn.net/u010067452/article/details/55047444


__ 4、安装并配置IntelliJ IDEA __

- 编辑 Run/Debug Configurations，配置应用服务器，指定tomcat位置
- 遇到问题
	- 问题：Tomcat启动时Mac端口被占用
	- 例子：`错误:代理抛出异常错误: java.rmi.server.ExportException: Port already in use: 1099; nested exception is: 
	java.net.BindException: Address already in use`
	- 解决：
	`lsof -i tcp:1099` 查看端口1099被占用的进程
	`kill -9 1533` 杀死掉进程3794

- 注意点
Stop tomcat 按钮需要点击两次，不然tomcat server JMX port 端口仍然被占用


__ 5、查看问题与解决问题途径 __

	请仔细查看Event Log面板报错，直接google





## 二、简单认识maven

__ 1、创建maven项目 __

- 首先在idea中配置maven，User settings file 与 Local repository路径
- 创建一个maven项目，会自动生成pow.xml和${projectName}.iml文件，在pow.xml中配置packaging为war，因为该项目是webapp类型

- 点击idea项目右侧有个选项Maven Projects，选择生命周期中的package，run maven build，这样将会在target目录下生成.war包文件，该包可以配置为tomcat编译服务包




## 三、认识java后端项目工程文件

__ 1、查看以${projectName}.iml工程配置文件 __

配置项：

- deploymentDescriptor 如：./src/main/webapp/WEB-INF/web.xml
- webApp/root 项目的关联目录 如：./src/main/webapp


__ 2、查看pom.xml文件，找到配置 线上访问路径的contextPath项 __
如：/yjh

因此idea编辑器配置tomcat的Application context项一定要为 /yjh，
 > 我的理解为该目录是 java控制器路由的根目录


__ 3、查看./src/main/webapp/WEB-INF/web.xml文件 __

找到配置spring mvc项，如下：

	<servlet>
	    <servlet-name>springMvc</servlet-name>
	    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	    <init-param>
	        <param-name>contextConfigLocation</param-name>
	        <param-value>classpath:yggservlet-mvc.xml</param-value>
	    </init-param>
	    <load-on-startup>1</load-on-startup>
	</servlet>

__ 4、查看./src/main/resources/yggservlet-mvc.xml __

里面有资源映射，url请求拦截，freemarker的配置 

