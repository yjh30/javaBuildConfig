# java工作环境搭建(入门)

## 环境搭建

#### 安装并配置jdk

	JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home
	PATH=$JAVA_HOME/bin:$PATH
	export PATH

测试命令：`echo $JAVA_HOME`


#### 安装并配置apache-maven

- 在~/目录下新建.m2文件夹，在~/.m2/目录下新建repository文件夹
- 将下载的apache-maven/conf/settings.xml复制到~/.m2/目录中
- 配置环境变量，参考：http://www.jianshu.com/p/191685a33786


#### 安装apache-tomcat

- 遇到问题
	问题：Cannot run program。。。 Permission denied
	解决：命令行进入tomcat的bin根目录，执行chmod 777 *.sh问题解决
	参考：http://blog.csdn.net/u010067452/article/details/55047444


#### 安装并配置IntelliJ IDEA

- 编辑 Run/Debug Configurations，配置应用服务器，指定tomcat位置
- 遇到问题
问题：Tomcat启动时Mac端口被占用
例子：`错误:代理抛出异常错误: java.rmi.server.ExportException: Port already in use: 1099; nested exception is: 
java.net.BindException: Address already in use`
解决：
`lsof -i tcp:1099` 查看端口1099被占用的进程
`kill -9 1533` 杀死掉进程3794

- 注意点
Stop tomcat 按钮需要点击两次，不然tomcat server JMX port 端口仍然被占用


#### 查看问题与解决问题途径

	请仔细查看Event Log面板报错，直接google


## 项目结构理解

#### 查看以.iml为后缀的工程配置文件

配置项：
deploymentDescriptor 如：./src/main/webapp/WEB-INF/web.xml
root 项目的关联目录 如：./src/main/webapp


#### 查看 项目模块hqbs-h5/pom.xml文件，找到配置 线上访问路径的contextPath项
如：/ygg-hqbs

因此idea编辑器配置tomcat的Application context项一定要为 /ygg-hqbs


#### 查看./src/main/webapp/WEB-INF/web.xml文件

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

#### 查看./src/main/resources/yggservlet-mvc.xml

里面有资源映射，url请求拦截，freemarker的配置

