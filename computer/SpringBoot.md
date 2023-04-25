1，项目结构
 SpringBoot就相当于一个启动器（start point）
安装maven ， setting.xml文件中设置<mirror>镜像地址为阿里云的。
创建项目：（1）start.spring.io生成压缩包 ，导入即可（2）idea直接生成
Application文件是一个网页的入口程序。换位置后，在该文件中加@ComponentScan("所在包路径")，才能正常启动网页。
pom.xml是与maven相关联的，有相关dependencies，properties。
banner自定义：在resources里新建一个banner.txt，可以自定义运行后的横幅。
通过Maven的package包可以将整个项目打jar包，然后通过命令行运行jar文件。

2，配置
resources里的application.properties里可以写配置，如server.port=8080，或是 新建application.yaml写成server: port:  8082(空格一定要加)
①定义自己用的配置，在类外加注解@ConfigurationProperties(prefix="person（自己建的配置名）")，类加@Autowired可以给类成员初始化。
   注意命名是松散绑定，变量名有一定的限制。
    @Value("${配置文件中的属性名}") 引入变量
②配置位置：优先级由高到低，第一二个是打包后的设置，三四是开发阶段
	file: config/..yml              
	file: ..yml		  
	classpath: config/..yml
	classpath: ..yml
③多环境配置：
配置根据所处阶段拆分（dev，test），还可以根据功能拆分（devMVC， devDB）
启动哪个配置： spring.profiles.active=dev ,  附加配置：include: devMVC 或是 group：“dev”: devMVC 
同一个yml中不同环境要用---分隔开，不同yml命名时用application-dev这种格式
④修改配置文件名称： 设置 临时变量 --spring.config.name="新名称"
⑤多环境开发控制，pom.xml中设置profiles（maven在控制，idea中切换配置环境是要maven，compile），yml中用@profile.active@读取

3，
注解的原理：反射
元注解（@Target（在哪使用，变量or方法）， Retention（啥阶段生效source，class，runtime）， Doucument， Inhertited（子类是否可以继承父类的注解））解释其他所有注解。
   
@Bean作用在方法上，一般标明返回的对象直接被Spring管理起来。调用的时候和@Component一样，用@Autowired 调用有@Bean注解的方法，
多用于第三方类无法写@Component的情况

4，
静态html文件放在resources的static文件下，欢迎页面是index.html，双击shift搜索WebMvcAutoConfiguration，里有相关程序
alt+7 打开structure显示整体目录结构，ctrl+鼠标左键可以转到方法定义

5，ctrl+alt+u 查看类图

6，springmvc中的@responsebody是将controller的返回值转化为json格式返回前端

7，有条件查询：使用mp中的selectList(QueryWrapper) 
      分页: 需要sql语句中加 limit ，新建一个config类为MP的 拦截器，添加@Configuration。在其中定义方法（固定格式，MP的分页拦截器） ，添加@Bean
      selectPage(IPage page, QueryWrapper qw)

8, linux下nohup(no hang up) 在系统后台不挂断地运行命令。

9，日志
   记录日志，可以用LoggerFactory.getLogger(xxxConstroller.class)，可以添加debug，info，warn，error四种类别的日志
   配置文件中，logging.level.root = info设置日志的级别，显示小于等于该级别的日志。生产日志文件file.name=server.log。rollingpolicy设置滚动日志，超过大小就新建
   加上lombok坐标，使用注解@slf4j快速添加日志对象log，可以直接用log.debug(".....")
   日志格式：时间 级别 线程名称 哪个类里产生的日志 消息

lombok还可以用@Data注解生成类的get()和set()方法

10，自动配置，自定义starter，boot启动核心原理
一，
生成bean：
① resources里生成 applicationContext.xml 文件里，定义<bean id="..." class="....."/>。在类中实例化ApplicationContext类，可以获得bean
√ ② 在class定义外，加 @Component（设置id），@Service（设置id）, @Controller , @Repository 等等。xml中定义加载bean的位置，<content:component-scan base-package="包的位置">
     对于第三方类，可以写一个方法返回该类的对象给方法外加注释 @Bean，包含该方法的类外加 @Component 或是 @Configuration
③ 干掉配置文件xml，改成创建配置类加 @ComponentScan({扫描Bean的位置信息})

继承FactoryBean是创建其他类的，可以自己初始化，用注释是不能自己初始化的。new FactoryBean()是造不出来自身的对象的！
@Configuration 默认使用Bean代理，不会每次都创建全新的Bean

④在类外加@Import（{要创建的类的class}） ，被导入的另类无需声明注释，利于解耦！
⑤上下文容器对象初始化后，调用register

二，
bean加载控制：
①必须是编程代码注册bean才可以控制，用注解无法控制
②或是加上spring-boot-starter的坐标后，用 @ConditionalOnClass(Mouse.class) 等注解控制bean的加载

三，bean的依赖属性配置
单独创建一个类@ConfigurationProperties(prefix="....")，在真正实体类中用构造函数注入（@AutoWire）。并在实体类外加@EnableConfigurationProperties，这样两个类都不用注解为bean。