## 15 Important Spring Core Annotations

Let's list all known Spring core annotations.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiQnu9dfYD2zcRDlhzt6QqUSz-cyyuaN-cu0uJZe8Ac0OUNGfAzDXSl2xK82ToDrc-zU5YVeBQicY_TnoLFDES7ALnFGieQ3AerLMLXZobNKt3JNBYt99ZU93mlge610KhyHe3ca95fpQk/s1600/spring-core-annotations.PNG)

## Spring Core Annotations for Dependency Injection and Configuration

### 1. @Autowired Annotation

The `@Autowired` annotation is used for automatic dependency injection in Spring. It can be applied to constructors, fields, and setter methods.

### Constructor Injection

```java
@RestController
public class CustomerController {
    private final CustomerService customerService;

    @Autowired
    public CustomerController(CustomerService customerService) {
        this.customerService = customerService;
    }
}

```

### Setter Injection

```java
@RestController
public class CustomerController {
    private CustomerService customerService;

    @Autowired
    public void setCustomerService(CustomerService customerService) {
        this.customerService = customerService;
    }
}

```

### Field Injection

```java
@RestController
public class CustomerController {
    @Autowired
    private CustomerService customerService;
}

```

For more details, visit our articles on [**Spring @Autowired Annotation**](https://www.javaguides.net/2018/09/spring-autowired-annotation-with-example.html) and [**Guide to Dependency Injection in Spring**](https://www.javaguides.net/2018/06/guide-to-dependency-injection-in-spring.html).

### 2. @Bean Annotation

The `@Bean` annotation indicates that a method produces a bean managed by the Spring container. It is typically declared in the configuration class.

```java
@Configuration
public class AppConfig {

    @Bean
    public CustomerService customerService() {
        return new CustomerService();
    }

    @Bean
    public OrderService orderService() {
        return new OrderService();
    }
}

```

This configuration is equivalent to the following Spring XML:

```xml
<beans>
    <bean id="customerService" class="com.companyname.projectname.CustomerService"/>
    <bean id="orderService" class="com.companyname.projectname.OrderService"/>
</beans>

```

Read more about the `@Bean` annotation on [**Spring @Bean Annotation with Example**](https://www.javaguides.net/2018/09/spring-bean-annotation-with-example.html).

### 3. @Qualifier Annotation

The `@Qualifier` annotation is used in conjunction with `@Autowired` to avoid confusion when multiple beans of the same type are configured.

### Example

Consider `EmailService` and `SMSService` implementing a single `MessageService` interface.

```java
public interface MessageService {
    void sendMsg(String message);
}

@Component
public class EmailService implements MessageService {
    public void sendMsg(String message) {
        System.out.println("Email message: " + message);
    }
}

@Component
public class SMSService implements MessageService {
    public void sendMsg(String message) {
        System.out.println("SMS message: " + message);
    }
}

```

Using `@Qualifier` to inject specific implementations:

```java
@Component
public class MessageProcessor {
    private final MessageService messageService;

    @Autowired
    @Qualifier("emailService")
    public MessageProcessor(MessageService messageService) {
        this.messageService = messageService;
    }

    public void processMsg(String message) {
        messageService.sendMsg(message);
    }
}

```

Read more on [**Spring @Qualifier Annotation Example**](https://www.javaguides.net/2018/06/spring-qualifier-annotation-example.html).

### 4. @Required Annotation

The `@Required` annotation is a method-level annotation applied to the setter method of a bean. It indicates that the setter method must be configured with a value at configuration time.

### Example

```java
@Required
void setColor(String color) {
    this.color = color;
}

```

XML Configuration:

```xml
<bean class="com.javaguides.spring.Car">
    <property name="color" value="green"/>
</bean>

```

### 5. @Value Annotation

The `@Value` annotation is used to assign default values to variables and method arguments. It supports Spring Expression Language (SpEL) for complex expressions.

### Example

```java
@Value("Default DBConfiguration")
private String defaultName;

@Value("${java.home}")
private String javaHome;

@Value("#{systemProperties['java.home']}")
private String javaHomeSpel;

```

Read more on [**Spring @Value Annotation**](https://www.javaguides.net/2018/09/spring-value-annotation.html).

### 6. @DependsOn Annotation

The `@DependsOn` annotation forces the Spring IoC container to initialize one or more beans before the bean annotated with `@DependsOn`.

### Example

```java
public class FirstBean {
    @Autowired
    private SecondBean secondBean;
}

public class SecondBean {
    public SecondBean() {
        System.out.println("SecondBean Initialized via Constructor");
    }
}

@Configuration
public class AppConfig {

    @Bean("firstBean")
    @DependsOn("secondBean")
    public FirstBean firstBean() {
        return new FirstBean();
    }

    @Bean("secondBean")
    public SecondBean secondBean() {
        return new SecondBean();
    }
}

```

Read more on [**Spring @DependsOn Annotation Example**](https://www.javaguides.net/2018/10/spring-dependson-annotation-example.html).

### 7. @Lazy Annotation

The `@Lazy` annotation delays the initialization of a singleton bean until it is first requested.

### Example

```java
public class FirstBean {
    public void test() {
        System.out.println("Method of FirstBean Class");
    }
}

@Configuration
public class AppConfig {

    @Lazy
    @Bean
    public FirstBean firstBean() {
        return new FirstBean();
    }

    @Bean
    public SecondBean secondBean() {
        return new SecondBean();
    }
}

```

Read more on [**Spring @Lazy Annotation Example**](https://www.javaguides.net/2018/10/spring-lazy-annotation-example.html).

### 8. @Lookup Annotation

A method annotated with `@Lookup` tells Spring to return an instance of the method’s return type when it is invoked.

Read more about the annotation in [**Spring @LookUp Annotation**](https://www.baeldung.com/spring-lookup).

### 9. @Primary Annotation

The `@Primary` annotation gives higher preference to a bean when multiple beans of the same type exist.

### Example

```java
@Component
@Primary
public class Car implements Vehicle {}

@Component
public class Bike implements Vehicle {}

@Component
public class Driver {
    @Autowired
    private Vehicle vehicle;
}

```

Read more on [**Spring @Primary Annotation Example**](https://www.javaguides.net/2018/10/spring-primary-annotation-example.html).

### 10. @Scope Annotation

The `@Scope` annotation defines the scope of a `@Component` class or a `@Bean` definition.

### Example

```java
@Component
@Scope(ConfigurableBeanFactory.SCOPE_SINGLETON)
public class SingletonService implements MessageService {}

@Component
@Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
public class PrototypeService implements MessageService {}

```

Read more on [**Spring @Scope Annotation with Singleton Scope Example**](https://www.javaguides.net/2018/10/spring-scope-annotation-with-singleton-scope-example.html) and [**Spring @Scope Annotation with Prototype Scope Example**](https://www.javaguides.net/2018/10/spring-scope-annotation-with-prototype.html).

### 11. @Profile Annotation

The `@Profile` annotation is used to conditionally include `@Component` classes or `@Bean` methods based on the active profile.

### Example

```java
@Component
@Profile("sportDay")
public class Bike implements Vehicle {}

```

Read more on [**Spring Profiles**](https://www.baeldung.com/spring-profiles).

### 12. @Import Annotation

The `@Import` annotation allows loading `@Bean` definitions from another configuration class.

### Example

```java
@Configuration
public class ConfigA {
    @Bean
    public A a() {
        return new A();
    }
}

@Configuration
@Import(ConfigA.class)
public class ConfigB {
    @Bean
    public B b() {
        return new B();
    }
}

```

Read more on [**Spring @Import Annotation**](https://www.javaguides.net/2018/09/spring-import-annotation-with-example.html).

### 13. @ImportResource Annotation

The `@ImportResource` annotation loads beans from an `applicationContext.xml` file into the `ApplicationContext`.

### Example

```java
@Configuration
@ImportResource({"classpath*:applicationContext.xml"})
public class XmlConfiguration {}

```

Read more on [**Spring @ImportResource Annotation**](https://www.javaguides.net/2018/09/spring-importresource-annotation-example.html).

### 14. @PropertySource Annotation

The `@PropertySource` annotation adds a `PropertySource` to Spring’s Environment.

### Example

```java
@Configuration
@PropertySource("classpath:config.properties")
public class PropertySourceDemo implements InitializingBean {

    @Autowired
    private Environment env;

    @Override
    public void afterPropertiesSet() throws Exception {
        setDatabaseConfig();
    }

    private void setDatabaseConfig() {
        DataSourceConfig config = new DataSourceConfig();
        config.setDriver(env.getProperty("jdbc.driver"));
        config.setUrl(env.getProperty("jdbc.url"));
        config.setUsername(env.getProperty("jdbc.username"));
        config.setPassword(env.getProperty("jdbc.password"));
        System.out.println(config.to

String());
    }
}

```

Read more on [**Spring @PropertySource Annotation Example**](https://www.javaguides.net/2018/09/spring-propertysource-annotation-example.html).

### 15. @ComponentScan Annotation

The `@ComponentScan` annotation configures component scanning directives for Spring to locate and register beans within the specified packages.

### Example

```java
@Configuration
@ComponentScan(basePackages = "com.example")
public class AppConfig {}

```

Read more on [**Spring @ComponentScan Annotation**](https://www.javaguides.net/2018/09/spring-componentscan-annotation-with-example.html).

### 16. @SpringBootApplication Annotation

The `@SpringBootApplication` annotation marks the main class of a Spring Boot application. It combines the functionality of `@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan`.

### Example

```java
@SpringBootApplication
public class MySpringBootApplication {
    public static void main(String[] args) {
        SpringApplication.run(MySpringBootApplication.class, args);
    }
}

```

Read more on [**Spring @SpringBootApplication Annotation**](https://www.javaguides.net/2018/09/springbootapplication-annotation-with-example.html).

### Conclusion

In this guide, we covered the fundamental Spring core annotations used for dependency injection, configuration, and managing the Spring IoC container. Understanding these annotations is crucial for effectively developing and configuring Spring applications. For further reading and deeper insights, follow the linked resources within each annotation section.