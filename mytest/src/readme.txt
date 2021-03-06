Step By Step Project

此为案例项目,以练习为主，先加入各个框架并测试通过，最后集成，改进：
	后续，以框架代码为开发目标，对整体做架构设计，开发为一个快速开发框架项目。
--------------------------------------------------------------------
	常用j2ee框架整合(struts2+ibatis+spring2,struts2+hibernate3+spring2)，
	webservice框架cxf，
	缓存框架 oscache，memcache
	数据库连接池 例如 DBCP1.1 ; c3p00.9.1
	视图技术 sitemash ，freemaker，velocity，jsp
	js库jquery (常用正则及含义) 

	环境:windows xp sp3 pro 
		java6 ,tomcat6,mysql5,windows xp(sp3),myeclipse6.5
		struts2
		spring2
		ibatis2.3.4
		
	Date:2011-10-24
--------------------------------------------------------------------
step1:
	* 新建 web project
	* 配置log4j，将信息根据其级别分别捕获，开发时打印debug，部署时打印error级别
	
step2:
	* 加入spring2.5支持(作为业务层，并使用其提供的各类功能)，即把spring的core和web部分的jar包拷贝到项目lib目录下;
	* 配置web.xml文件，如下：
	  <!-- 定义spring配置文件位置和文件名 -->
	  <context-param>
	  	<param-name>contextConfigLocation</param-name>
	  	<param-value>WEB-INF/classes/bean.xml</param-value>
	  </context-param>

	  <listener>
	  	<description>spring listener</description>
	  	<display-name>listener</display-name>
	  	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	  </listener>
	这样，spring在项目启动时进行初始化。	
	* 编写测试代码，(测试spring的IOC，测试..待补充)
	* spring自带了log4j日志插件的jar包，在src下建立log4j.properties文件,配置可以从其jar包拷贝，或者拷贝其他项目配置
	  详细配置参考log4j的框架指南。

step3:
	* 加入ibatis框架作为Dao层的O/R映射框架
	* ibatis就一个jar包，把它拷贝到lib目录下；其他的jar根据需要自行添加，在框架的依赖说明文件里有详细列表
	* 参考ibatis自带的一个小例子，修改配置文件和映射文件，并测试

step4:
	* 集成ibatis和spring
	* 加入spring持久层模块的jar包
	* 找到spring支持ibatis集成的类库接口(org.springframework.orm.ibatis.SqlMapClientFactoryBean)
	* 在spring配置文件里配置上面这个工厂类(定义了bean的class属性后，IDE可提示此类有哪些属性可以配置,这是spring配置文件的小技巧)，
	  此类即可提供ibatis与数据库交互的 SqlMapClient 对象
	* 在其他需要用到 SqlMapClient 的类中，得到这个工厂类返回的 SqlMapClient 即可
	* 测试：通过springIOC注入 SqlMapClient 到业务类中，并使用 SqlMapClient 与数据库交互(web容器测试，或者初始化spring上下文来测试getBean)
step5:
	* 加入数据库连接池(这里指JDBC连接池，hibernate自带的不适合生产环境)框架 c3p0(或者DBCP等)
	* 数据源的配置 (简单jdbc数据源，容器数据源，jndi数据源，从缓冲池取得的数据源)
	
	补充：
			配置就是，在项目的meta-inf下配置context.xml文件；然后再web.xml里引用数据源；如果出问题，比如找到了数据源，但是
			是无效的数据源，获取不到jdbc连接，可能原因是：
			在项目的meta-inf目录下定义的context.xml文件没用出现在 ：
		{tomcat_home}\conf\Catalina\localhost目录里 有上面这个context文件，文件名为访问的命名 （E:\apache-tomcat-6.0.29\conf\Catalina\localhost）

	测试方法，最小化测试：建个web项目，其他上面都不要只要最基本的测试jndi的东西（比如jdbc驱动，jsp页，数据源配置文件）
	
	
	* 配置好 c3p0对无效数据库链接的处理，比如数据库重启了；需要配置链接可用性检测策略。可在其自带文档中找解决方法。
	* spring结合c3p0，当连接不可用时(比如数据库重启后，会一直报一个错误)，c3p0报的异常如何捕获？ 待 
	
	
step5:
	* 加入spring事务管理 ,事务到方法级 
		测试 5种spring事务配置方式(测试)
		1) tx方式配置service层事务
		2)
	待...
	测试通过，具体见测试代码
	* 
step6:
	* 将代码版本管理放到 google code(网络开发,团队开发)	
	* google code 版本控制：
	  - 暂选用 SubVersion 作为版本控制，登陆googlecode后，找到source标签，里面有如何使用的说明，我的配置：
			地址 
			
					https://my-javaweb-project-demo.googlecode.com/svn/mytest/ 
					
					(mytest是用myeclipse的svn插件登陆后新建的目录)
					
			用户名 邮箱名 ；密码 在 googlecode的setting标签里管理

			
step7: 
	* 使用 spring-mvc ，作为控制器 spring自带的step by step mvc开发文档作为参考
	* 在web.xml文件中配置控制器(DispatcherServlet,一个servlet)，作用：It is going to control
	  where all our requests are routed based on information we will enter at a later point
	  具体配置见web.xml文件
	* 创建上面dispatcher要用的配置文件，文件名要和web.xml里配置的servlet名匹配(springapp-servlet.xml)；
	   注意，配置文件放到WEB-INF 目录下。
	* 创建controller(继承spring的org.springframework.web.servlet.mvc.Controller)并用junit测试(需要导入junit4的jar包)，
	   测试controller是否返回正确的view，具体见代码
	   补充：
	  	  用Ant工具来编译并测试：到apache网站下载binary包和它的manual，根据manual配置。
			1) Make sure you have a Java environment installed, See System Requirements for details. 
			2) Download Ant. See Binary Edition for details. 
			3) Uncompress the downloaded file into a directory. 
			4) Set environmental variables JAVA_HOME to your Java environment, ANT_HOME to the directory you uncompressed Ant to, and add ${ANT_HOME}/bin (Unix) or %ANT_HOME%/bin (Windows) to your PATH. See Setup for details. 
			5) Optionally, from the ANT_HOME directory run ant -f fetch.xml -Ddest=system to get the library dependencies of most of the Ant tasks that require them. If you don't do this, many of the dependent Ant tasks will not be available. See Optional Tasks for details and other options for the -Ddest parameter. 
			6) Optionally, add any desired Antlibs. See Ant Libraries for a list.	  	  
	   		example:path = %JAVA_HOME%\bin;E:\Program Files\Git\bin;%ANT_HOME%\bin;
	   	  myeclipse 提供了对ant的支持，提供ant编辑器编辑ant的配置文件，有属性提示等方便功能。
	   	  
	   	  - ant支持svn，需要下载svnant的支持包，里面有例子示范如何使用svnant
	   	  待解决？
	   	  
	* 创建视图hello.jsp
	* 部署，测试 http://localhost:8080/mytest/hello.action	,返回到正确的jsp页面上
	* 注意这部分代码测试等的目录设置，测试与开发分开
	* jsp页面的jstl支持：attribute value does not accept any expressions ，可能是jstl的版本导致不支持,比如有的版本out标签的value属性不支持表达式。
	* 对测试用例中，把视图直接定义在controller中的代码进行解耦(Decouple the view from the controller)
	* spring的Dao层封装异常 DataAccessException 是否需要捕捉(在哪里捕捉)，以及和事务回滚的关联？自定义事务回滚策略
	* Controller的成员变量，在dispatcher配置文件中配置，比如要用到的service等
	* 可以通过tomcat的管理页面来启动和关闭实例。
	* spring 初始化出现问题之一：没有导入对应的包，比如用了aop就需要导入aop对应的jar包，这在配置文件中配置了，但jar包没有导入，可能导致隐蔽的异常。
		aop功能需要导入spring.jar包，用到aop公共组织的库;用aspectj实现aop需要导入aspectj的相关包,asm,cglib;
		主要就是aop的相关jar包，aop的实现方式有多个。
		方法：工具提示的错误，比如某个class找不到，即可定位是少了那些jar包。
		
	* 将project从svn上checkout一份到其他地方，以备份。
	* 配置国际化 ? 视图显示时用标签输出实现国际化 将国际化配置文件配置到容器上下文或者其他位置。
		具体配置到web.xml中，用jstl标签的fmt:message ；或者配置到spring中用spring的标签来输出
		这些标签的定义都有对应的标签库tld文件，也可以自定义自己的标签以适应需要。
		标签定义都是以xml格式来定义,更多关于xml的见文档
		
		
	* 为每个错误准备一个错误页面。？
			
step7 增加ant功能 ，可参考其他项目的ant配置文件
	结合ant进行测试，部署等任务
	
	
step8 测试了JNDI数据源，对step5做了补充，jndi数据源测试通过

step9 配置spring异常捕获及处理 待 ？


step10 计划分支出maven版本 待？


step11 对于项目context下配置的数据源，需要提供关闭入口，
如果建立了数据源但在服务器关闭时没有关闭，服务器会强制关闭，并提示资源未关闭，将被强制关闭。
	tomcat配置基本数据源(unpooled)和配置c3p0带连接池的数据源且都注册到JNDI中
	
	修改项目meta-info下面的context.xml配置文件后，部署到服务器时ide可能不能正确部署这个文件，比如tomcat，即使删除了这个部署重新部署旧的配置文件也还是没变，这时需要手动删除后，
	再部署。参看 TipsOfDay 的 day331 第2条 有记录
	
	~不要钻牛角尖~，在正确的前提下，去尝试变化，不要将希望寄托于一个错误的前提 
	案例：
		在配置基本数据源，并测试通过情况下再去配置c3p0连接池；需要配置的地方有：
			* 项目meta-info下的context.xml
			* 项目web.xml里引用配置
			* 调用数据源的地方配置（比如spring配置文件）
	
	经过：忘了配置web.xml，还一直以为context.xml配置不对，钻进去了。回头测了下基本数据源也提示错误，才意识到不是这里的问题。	
	
	
step12 基于lucene检索框架，实现简易的检索模块，集成到mytestsvn项目中 ?
		* 下载lucene包，并根据文档将jar加入到项目lib目录中
		
		
step13 实现天气webservice的调用，然后发布自己的service ？ 改为获得腾讯QQ在线状态服务调用
		技术：jax-ws ，及其支持的api，服务供应商提供运行时包等

		*  得到服务对应的WSDL文件(直接访问URL或者下载下来)
		* 利用IDE提供的插件，根据WSDL由上到下生成需要的客户端文件
		* 建立test类，测试服务调用
		
		注：测试时，碰到版本冲突问题 部分异常如下：
						java.lang.LinkageError: JAXB 2.0 API is being loaded from the bootstrap classloader, but this RI (from jar:file:/E:/Program%20Files/MyEclipse%206.5/myeclipse/eclipse/plugins/com.genuitec.eclipse.ws.xfire_6.5.0.zmyeclipse650200806/lib/webservices-rt.jar!/com/sun/xml/bind/v2/model/impl/ModelBuilder.class) needs 2.1 API. Use the endorsed directory mechanism to place jaxb-api.jar in the bootstrap classloader. (See http://java.sun.com/j2se/1.5.0/docs/guide/standards/)
				最后通过 sun的Endorsed Standards Override Mechanism机制解决新旧版本问题。(在javahome的lib或jre的lib下，建立endorse目录，并把最新的jar放到这里，加载时优先加载)
				
				
		
		
		

---------------------------------------------------------------------------
其他功能需求：
	* 类似 google reader ，提供rss阅读功能；可以收藏rss并显示其内容
	* 实现android手机版	
	* 所部署的服务器(apache+tomcat / apache+resin)
	  服务器cluster,loadbalance，数据库集群		
	  
	  resin:tomcat移resin后，classpath不一致配置文件路径需要修改，web.xml的某些标签比如国际化标签不支持。
			resin pro 相对于tomcat来说，其配置和管理功能非常丰富，方便集群和负载均衡。详细见其文档 
			tip:调试时，根据控制台输出来做相应调整。	
	* 
	
	
---------------------------------------------------------------------------
other
	* 任务 对一个简单请求的完整处理流程有个大致了解。
		从http请求达到web服务器到返回客户端，一些点的过程：
			HTTP服务器，请求到达端口(一般http请求在80端口)口，被HTTP服务器侦测到，并处理请求；根据配置把一些
		动态资源请求交给一些容器处理(比如处理servlet请求的servlet容器)。
			servlet容器，根据servlet规范请求来了是先经过过滤器(filter)然后才是servlet，到
		servlet后，交给servlet的doService方法处理(根据servlet规范)。
	
	* usemon - 根据svn代码调试。
	
	
	* 在 com.servlet包里新建一个servlet重写tomcat自带的DefaultServlet 测试
	
	