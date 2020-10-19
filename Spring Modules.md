# Spring 模块组成
框架的好处：
例子：JDBC vs. Hibernate 

Traditional JDBC:
```java
try {
	// 加载MySQL的驱动类
	Class.forName("com.mysql.jdbc.Driver");
} catch(ClassNotFoundException ex) {
	ex.printStackTrace();
}

// 连接MySQL数据库，用户名和密码都是root
String url = "jdbc.mysql://localhost:3306/laiproject";
String username = "root";
String password = "root";
try {
	Connection conn = DriverManager.getConnection(url, username, password);
} catch (SQLException ex) {
	ex.printStackTrace();
}

String sql = "…";
PreparedStatement ps = con.prepareStatement(sql);
ps.setString();
…
ResultSet rs = statement.executeQuery();
while (rs.next()) {
	categories.add(rs.getString("category"));
}
rs.close();
ps.close();
con.close();
```

Hibernate Framework:
```java
Configuration cfg = new Configuration().configure();
SessionFactory factory = cfg.buildSessionFactory();
Session session = factory.openSession();
Transaction tx = session.beginTransaction();
session.save(obj);
tx.commit();
```

Spring 的模块：
- Core Container（核心模块）
	- 控制反转（IOC）inversion of control   设计思想
	-依赖注入（dependency injection）     实现形式
- Web：Spring MVC
- AOP（Aspect Oriented Programming）

## Inversion of Control & Dependency Injection:
![IOC](/images/IOC.png)

面向对象的程序通过一组对象之间相互通信来实现特定功能，这里的通信具体来说就是一个对象对另一个对象的方法调用或者属性访问。
```java
public interface ILogger {
	void log();
}

public class FileLogger implements ILogger {
	public void log() {}
} 

public class ServerLogger implements ILogger {
	public void log() {}
}

public class PaymentAction {
	private ILogger logger = new FileLogger();
	
	public void pay(BigDecimal payValue) {
		logger.log("pay begin, payValue is " + payValue);
		logger.log("pay end");
	}
}
```

对logger的依赖是由外部实例化之后注入给它的，这就是依赖注入
```java
public class PaymentAction {

	private ILogger logger
	
	public PaymentAction(ILogger logger) {
		this.logger = logger;
	}
	
	public void pay(BigDecimal payValue) {
		logger.log("pay begin, payValue is " + payValue);
		logger.log("pay end");
	}
}

public class Application {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("definition.xml");
		
		PaymentAction paymentAction = (PaymentAction) context.getBean("paymentAction");
		
		paymentAction.pay(nwe BigDecimal(2));
	}
}
```

传统的代码，每个对象负责管理自己依赖的对象，导致如果需要切换依赖对象的实现类时，需要修改多处地方。

a = paymentAction 

b = Logger
a依赖b，但a不控制b的创建和销毁，仅使用b，那么b的控制权交给a之外处理，这叫控制反转（IOC）。而a要依赖b，必然要使用b的instance，那么
1. 通过a的构造函数，把b传入；（Constructor）
2. 通过设置a的属性，把b传入；（setter method）
3. @Autowired 

这个过程叫依赖注入。

依赖注入把对象的创造交给外部去管理，很好的解决了代码紧耦合（tight couple）的问题，是一种让代码使用松耦合（loose couple）的机制。松耦合让代码更具灵活性，能更好地应对需求变动。

### IOC Container 
The container gets its instructions on what objects to instantiate, configure, and assemble by reading configuration metadata. The configuration metadata is represented in XML, Java annotations, or Java code.

The diagram above is a high-level view of how Spring works. Your application classes are combined with configuration metadata so that after ApplicationContext is created and initialized, you have a fully configured and executable system or application.
- AnnotationConfigApplicationContext:For standalone java applications using annotations based configuration.
- ClassPathXmlApplicationContext:For standalone java applications using XML based configurations.
- AnnotationConfigWebApplicationContext and XmlWebApplicationContext for web applications.

## Bean
Any normal java class that is initialized by Spring IoC container is called bean.

### Bean Scope
- singleton：该作用域将bean的定义限制在每一个Spring IoC容器中的一个单一实例（默认）
- prototype：该作用域将bean的定义限制在任意数量的对象实例

### Configuration 
1. XML configuration 
2. Annotaion-based configuration 
3. Java-based configuration 


## AOP(Aspect Oriented Programming)
- 切面（Aspect）What：指的就是通用功能的代码实现，比如我们的时间记录切面，日志切面，它们都是普通的Java类：TimeRecordingAspect和LogAspect。
- 切入点（Pointcut）Where:定义通知应该切入到什么地方，切入点的定义可以使用正则表达式，用以描述什么类型的方法调用。@Pointcut 就是用来定义切入点的。
- 通知（Advice）When：切面是一个类，而通知就是类里的方法以及这个方法如何织入到切入点的方式（用@AfterReturning 和 @Around 标注的方法）。
	- 前置通知（Before）
	- 最终通知（After）
	- 环绕通知（Around）
	
## What are the benefits of using Spring？
- Lightweight: there is a slight overhead of using the framework in development 
- Inversion of Control(IoC): Spring container takes care of wiring dependencies of various objects, instead of creating or looking for dependent objects
- IoC container: It manages Spring Bean life cycle and project specific configurations
- Spring is non invasive（非侵入式）: That means you no need to implement any interface or inherit any class from spring to your classes, so whenever you want to change from spring to any other technology then you no need to change the logics of your class.
- End to end Development: Spring supports all aspects of application development, Web aspects, Business aspects, Persistance aspects, etc, so we can develop a complete application using spring.








