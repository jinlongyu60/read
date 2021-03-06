# Integration Testing

- 01 [Goals of Integration Testing](#01-goals-of-integration-testing)
- 02 [Context management and cachin](#02-context-management-and-cachin)
- 03 [Dependency Injection of test fixtures](#03-dependency-injection-of-test-fixtures)
- 04 [Transaction management](#04-transaction-management)
- 05 [Support classes for integration testing](#05-support-classes-for-integration-testing)
- 06 [JDBC Testing Support](#06-jdbc-testing-support)
- 07 [Annotations](#07-annotations)
- 08 [Spring Testing Annotations](#08-spring-testing-annotations)
- 09 [Standard Annotation Support](#09-standard-annotation-support)
- 10 [Spring JUnit 4 Testing Annotations](#10-spring-junit-4-testing-annotations)
- 11 [Meta-Annotation Support for Testing](#11-meta-annotation-support-for-testing)
- 12 [Key abstractions](#12-key-abstractions)
- 13 [Context management](#13-context-management)
- 14 [Context configuration inheritance](#14-context-configuration-inheritance)
- 15 [Mixing XML, Groovy scripts, and annotated classes](#15-mixing-xml-groovy-scripts-and-annotated-classes)
- 16 [Context configuration with context initializers](#16-context-configuration-with-context-initializers)
- 17 [Context configuration inheritance](#17-context-configuration-inheritance)
- 18 [Context configuration with environment profiles](#18-context-configuration-with-environment-profiles)
- 19 [Loading a WebApplicationContext](#19-loading-a-webapplicationcontext)
- 20 [Context caching](#20-context-caching)
- 21 [Context hierarchies](#21-context-hierarchies)
- 22 [Dependency injection of test fixtures](#22-dependency-injection-of-test-fixtures)
- 23 [Testing request and session scoped beans](#23-testing-request-and-session-scoped-beans)
- 24 [Transaction management](#24-transaction-management)
- 25 [TestContext Framework support classes](#25-testcontext-framework-support-classes)
- 26 [Spring MVC Test Framework](#26-spring-mvc-test-framework)
  - [26.1 Server-Side Tests](#261-server-side-tests)
  - [26.2 Performing requests](#262-performing-requests)
  - [26.3 Defining expectations](#263-defining-expectations)
  - [26.4 Filter Registrations](#264-filter-registrations)
  - [26.5 Differences between Out-of-Container and End-to-End Integration Tests](#265-differences-between-out-of-container-and-end-to-end-integration-tests)
  - [26.6 Client-Side REST Tests](#266-client-side-rest-tests)
  - [26.7 HtmlUnit Integration](#267-htmlunit-integration)

## 01 Goals of Integration Testing

Spring’s integration testing support has the following primary goals:

- To manage Spring IoC container caching between test execution.
- To provide Dependency Injection of test fixture instances.
- To provide transaction management appropriate to integration testing.
- To supply Spring-specific base classes that assist developers in writing integration tests.

The next few sections describe each goal and provide links to implementation and configuration details.

## 02 Context management and caching

See Section 15.5.4, [Context managemen](https://docs.spring.io/spring/docs/4.3.x/spring-framework-reference/htmlsingle/#testcontext-ctx-management) and the section called [Context caching](https://docs.spring.io/spring/docs/4.3.x/spring-framework-reference/htmlsingle/#testcontext-ctx-management-caching) with the TestContext framework.

## 03 Dependency Injection of test fixtures

## 04 Transaction management

## 05 Support classes for integration testing

- ApplicationContext

- JdbcTemplate

## 06 JDBC Testing Support

- JdbcTestUtils
- AbstractTransactionalJUnit4SpringContextTests
- AbstractTransactionalTestNGSpringContextTests

## 07 Annotations

[Link](https://docs.spring.io/spring/docs/4.3.x/spring-framework-reference/htmlsingle/#integration-testing-annotations)

### 08 Spring Testing Annotations

@BootstrapWith

@ContextConfiguration

@ContextConfiguration defines class-level metadata that is used to determine how to load and configure an ApplicationContext for integration tests. Specifically, @ContextConfiguration declares the application context resource locations or the annotated classes that will be used to load the context.

Resource locations are typically XML configuration files or Groovy scripts located in the classpath; whereas, annotated classes are typically @Configuration classes. However, resource locations can also refer to files and scripts in the file system, and annotated classes can be component classes, etc.

```java
@ContextConfiguration("/test-config.xml")
public class XmlApplicationContextTests {
    // class body...
}
```

@WebAppConfiguration

@WebAppConfiguration is a class-level annotation that is used to declare that the ApplicationContext loaded for an integration test should be a WebApplicationContext. The mere presence of @WebAppConfiguration on a test class ensures that a WebApplicationContext will be loaded for the test, using the default value of "file:src/main/webapp" for the path to the root of the web application (i.e., the resource base path). The resource base path is used behind the scenes to create a MockServletContext which serves as the ServletContext for the test’s WebApplicationContext.

```java
@ContextConfiguration
@WebAppConfiguration("classpath:test-web-resources")
public class WebAppTests {
    // class body...
}
```

@ContextHierarchy

```java
@ContextHierarchy({
    @ContextConfiguration("/parent-config.xml"),
    @ContextConfiguration("/child-config.xml")
})
public class ContextHierarchyTests {
    // class body...
}
```

@ActiveProfiles

```java
@ContextConfiguration
@ActiveProfiles({"dev", "integration"})
public class DeveloperIntegrationTests {
    // class body...
}
```

@TestPropertySource

@TestPropertySource is a class-level annotation that is used to configure the locations of properties files and inlined properties to be added to the set of PropertySources in the Environment for an ApplicationContext loaded for an integration test.

The following example demonstrates how to declare a properties file from the classpath.

```java
@ContextConfiguration
@TestPropertySource("/test.properties")
public class MyIntegrationTests {
    // class body...
}
```

The following example demonstrates how to declare inlined properties.

```java
@ContextConfiguration
@TestPropertySource(properties = { "timezone = GMT", "port: 4242" })
public class MyIntegrationTests {
    // class body...
}
```

@DirtiesContext

@TestExecutionListeners

@Commit

@Commit indicates that the transaction for a transactional test method should be committed after the test method has completed. @Commit can be used as a direct replacement for @Rollback(false) in order to more explicitly convey the intent of the code. Analogous to @Rollback, @Commit may also be declared as a class-level or method-level annotation.

```java
@Commit
@Test
public void testProcessWithoutRollback() {
    // ...
}
```

@Rollback

@Rollback indicates whether the transaction for a transactional test method should be rolled back after the test method has completed. If true, the transaction is rolled back; otherwise, the transaction is committed (see also @Commit). Rollback semantics for integration tests in the Spring TestContext Framework default to true even if @Rollback is not explicitly declared.

When declared as a class-level annotation, @Rollback defines the default rollback semantics for all test methods within the test class hierarchy. When declared as a method-level annotation, @Rollback defines rollback semantics for the specific test method, potentially overriding class-level @Rollback or @Commit semantics.

```java
@Rollback(false)
@Test
public void testProcessWithoutRollback() {
    // ...
}
```

@BeforeTransaction

@AfterTransaction

@Sql

@SqlConfig

@SqlGroup

## 09 Standard Annotation Support

`@Autowired`
`@Qualifier`
`@Resource` (javax.annotation) if JSR-250 is present
`@ManagedBean` (javax.annotation) if JSR-250 is present
`@Inject` (javax.inject) if JSR-330 is present
`@Named` (javax.inject) if JSR-330 is present
`@PersistenceContext` (javax.persistence) if JPA is present
`@PersistenceUnit` (javax.persistence) if JPA is present
`@Required`

## 10 Spring JUnit 4 Testing Annotations

The following annotations are only supported when used in conjunction with the SpringRunner, Spring’s JUnit rules, or Spring’s JUnit 4 support classes.

spring and JUnit 4 特殊注解

@IfProfileValue

@ProfileValueSourceConfiguration

@Timed

制定在多少时间内完成测试

```java
@Timed(millis=1000)// (in milliseconds 毫秒)
public void testProcessWithOneSecondTimeout() {
    // some logic that should not take longer than 1 second to execute
}
```

@Repeat

重复执行10次

```java
@Repeat(10)
@Test
public void testProcessRepeatedly() {
    // ...
}
```

## 11 Meta-Annotation Support for Testing

- `@BootstrapWith`
- `@ContextConfiguration`
- `@ContextHierarchy`
- `@ActiveProfiles`
- `@TestPropertySource`
- `@DirtiesContext`
- `@WebAppConfiguration`
- `@TestExecutionListeners`
- `@Transactional`
- `@BeforeTransaction`
- `@AfterTransaction`
- `@Commit`
- `@Rollback`
- `@Sql`
- `@SqlConfig`
- `@SqlGroup`
- `@Repeat`
- `@Timed`
- `@IfProfileValue`
- `@ProfileValueSourceConfiguration`

> For example, if we discover that we are repeating the following configuration across our JUnit 4 based test suite…​

## 12 Key abstractions

`TestContextManager`
`TestContext`
`TestExecutionListener`
`SmartContextLoader`

## 13 Context management

Context configuration with XML resources

```java
@RunWith(SpringRunner.class)
// ApplicationContext will be loaded from "/app-config.xml" and
// "/test-config.xml" in the root of the classpath
@ContextConfiguration(locations={"/app-config.xml", "/test-config.xml"})
public class MyTest {
    // class body...
}
```

Context configuration with Groovy scripts

```java
@RunWith(SpringRunner.class)
// ApplicationContext will be loaded from "/AppConfig.groovy" and
// "/TestConfig.groovy" in the root of the classpath
@ContextConfiguration({"/AppConfig.groovy", "/TestConfig.Groovy"})
public class MyTest {
    // class body...
}
```

Context configuration with annotated classes

```java
@RunWith(SpringRunner.class)
// ApplicationContext will be loaded from AppConfig and TestConfig
@ContextConfiguration(classes = {AppConfig.class, TestConfig.class})
public class MyTest {
    // class body...
}
```

@Configuration

```java
@RunWith(SpringRunner.class)
// ApplicationContext will be loaded from the
// static nested Config class
@ContextConfiguration
public class OrderServiceTest {

    @Configuration
    static class Config {

        // this bean will be injected into the OrderServiceTest class
        @Bean
        public OrderService orderService() {
            OrderService orderService = new OrderServiceImpl();
            // set properties, etc.
            return orderService;
        }
    }

    @Autowired
    private OrderService orderService;

    @Test
    public void testOrderService() {
        // test the orderService
    }

}
```

## 14 Context configuration inheritance

## 15 Mixing XML, Groovy scripts, and annotated classes

If you want to use resource locations (e.g., XML or Groovy) and @Configuration classes to configure your tests, you will have to pick one as the entry point, and that one will have to include or import the other. For example, in XML or Groovy scripts you can include @Configuration classes via component scanning or define them as normal Spring beans; whereas, in a @Configuration class you can use @ImportResource to import XML configuration files or Groovy scripts. Note that this behavior is semantically equivalent to how you configure your application in production: in production configuration you will define either a set of XML or Groovy resource locations or a set of @Configuration classes that your production ApplicationContext will be loaded from, but you still have the freedom to include or import the other type of configuration.

## 16 Context configuration with context initializers

## 17 Context configuration inheritance

inheritLocations

inheritInitializers

```java
RunWith(SpringRunner.class)
// ApplicationContext will be loaded from "/base-config.xml"
// in the root of the classpath
@ContextConfiguration("/base-config.xml")
public class BaseTest {
    // class body...
}

// ApplicationContext will be loaded from "/base-config.xml" and
// "/extended-config.xml" in the root of the classpath
@ContextConfiguration("/extended-config.xml")
public class ExtendedTest extends BaseTest {
    // class body...
}
```

## 18 Context configuration with environment profiles

## 19 Loading a WebApplicationContext

This is interpreted as a path relative to the root of your JVM (i.e., normally the path to your project). If you’re familiar with the directory structure of a web application in a Maven project, you’ll know that "src/main/webapp" is the default location for the root of your WAR. If you need to override this default, simply provide an alternate path to the @WebAppConfiguration annotation (e.g., @WebAppConfiguration("src/test/webapp")). If you wish to reference a base resource path from the classpath instead of the file system, just use Spring’s classpath: prefix.

## 20 Context caching

缓存费时的`ApplicationContext`

>note

The Spring TestContext framework stores application contexts in a static cache. This means that the context is literally stored in a static variable. In other words, if tests execute in separate processes the static cache will be cleared between each test execution, and this will effectively disable the caching mechanism.

To benefit from the caching mechanism, all tests must run within the same process or test suite. This can be achieved by executing all tests as a group within an IDE. Similarly, when executing tests with a build framework such as Ant, Maven, or Gradle it is important to make sure that the build framework does not fork between tests. For example, if the forkMode for the Maven Surefire plug-in is set to always or pertest, the TestContext framework will not be able to cache application contexts between test classes and the build process will run significantly slower as a result.

## 21 Context hierarchies

[Link](https://docs.spring.io/spring/docs/4.3.x/spring-framework-reference/htmlsingle/#testcontext-ctx-management-ctx-hierarchies)

When writing integration tests that rely on a loaded Spring ApplicationContext, it is often sufficient to test against a single context; however, there are times when it is beneficial or even necessary to test against a hierarchy of ApplicationContexts. For example, if you are developing a Spring MVC web application you will typically have a root WebApplicationContext loaded via Spring’s ContextLoaderListener and a child WebApplicationContext loaded via Spring’s DispatcherServlet. This results in a parent-child context hierarchy where shared components and infrastructure configuration are declared in the root context and consumed in the child context by web-specific components. Another use case can be found in Spring Batch applications where you often have a parent context that provides configuration for shared batch infrastructure and a child context for the configuration of a specific batch job.

## 22 Dependency injection of test fixtures

`DependencyInjectionTestExecutionListener`

- @Autowired and @Qualifier

- @Inject and @Named

## 23 Testing request and session scoped beans

- `MockHttpServletRequest`

- `MockHttpSession`

## 24 Transaction management

`TransactionalTestExecutionListener`

By default, test transactions will be automatically rolled back after completion of the test; however, transactional commit and rollback behavior can be configured declaratively via the `@Commit` and `@Rollback` annotations. See the corresponding entries in the annotation support section for further details.

Demonstration of all transaction-related annotations

```java
@RunWith(SpringRunner.class)
@ContextConfiguration
@Transactional(transactionManager = "txMgr")
@Commit
public class FictitiousTransactionalTest {

    @BeforeTransaction
    void verifyInitialDatabaseState() {
        // logic to verify the initial state before a transaction is started
    }

    @Before
    public void setUpTestDataWithinTransaction() {
        // set up test data within the transaction
    }

    @Test
    // overrides the class-level @Commit setting
    @Rollback
    public void modifyDatabaseWithinTransaction() {
        // logic which uses the test data and modifies database state
    }

    @After
    public void tearDownWithinTransaction() {
        // execute "tear down" logic within the transaction
    }

    @AfterTransaction
    void verifyFinalDatabaseState() {
        // logic to verify the final state after transaction has rolled back
    }

}
```

## 25 TestContext Framework support classes

Spring JUnit 4 Runner

`@RunWith(SpringJUnit4ClassRunner.class)` `@RunWith(SpringRunner.class)`

The Spring TestContext Framework offers full integration with JUnit 4 through a custom runner (supported on JUnit 4.12 or higher). By annotating test classes with @RunWith(SpringJUnit4ClassRunner.class) or the shorter @RunWith(SpringRunner.class) variant, developers can implement standard JUnit 4 based unit and integration tests and simultaneously reap the benefits of the TestContext framework such as support for loading application contexts, dependency injection of test instances, transactional test method execution, and so on. If you would like to use the Spring TestContext Framework with an alternative runner such as JUnit 4’s Parameterized or third-party runners such as the MockitoJUnitRunner, you may optionally use Spring’s support for JUnit rules instead.

```java
@RunWith(SpringRunner.class)
@TestExecutionListeners({})
public class SimpleTest {

    @Test
    public void testMethod() {
        // execute test logic...
    }
}
```

Spring JUnit 4 Rules

In order to support the full functionality of the TestContext framework, a SpringClassRule must be combined with a SpringMethodRule. The following example demonstrates the proper way to declare these rules in an integration test.

```java
// Optionally specify a non-Spring Runner via @RunWith(...)
@ContextConfiguration
public class IntegrationTest {

    @ClassRule
    public static final SpringClassRule springClassRule = new SpringClassRule();

    @Rule
    public final SpringMethodRule springMethodRule = new SpringMethodRule();

    @Test
    public void testMethod() {
        // execute test logic...
    }
}
```

JUnit 4 support classes

- `AbstractJUnit4SpringContextTests` -> `ApplicationContext`
- `AbstractTransactionalJUnit4SpringContextTests` -> `javax.sql.DataSource` -> `PlatformTransactionManager` -> `ApplicationContext` -> `jdbcTemplate`

TestNG support classes

[link](https://docs.spring.io/spring/docs/4.3.x/spring-framework-reference/htmlsingle/#testcontext-support-classes-testng)

## 26 Spring MVC Test Framework

The Spring MVC Test framework provides first class support for testing Spring MVC code using a fluent API that can be used with JUnit, TestNG, or any other testing framework. It’s built on the Servlet API mock objects from the spring-test module and hence does not use a running Servlet container. It uses the DispatcherServlet to provide full Spring MVC runtime behavior and provides support for loading actual Spring configuration with the TestContext framework in addition to a standalone mode in which controllers may be instantiated manually and tested one at a time.

Spring MVC Test also provides client-side support for testing code that uses the RestTemplate. Client-side tests mock the server responses and also do not use a running server.

[Differences between Out-of-Container and End-to-End Integration Tests](https://docs.spring.io/spring/docs/4.3.x/spring-framework-reference/htmlsingle/#spring-mvc-test-vs-end-to-end-integration-tests)

## 26.1 Server-Side Tests

> The goal of Spring MVC Test is to provide an effective way for testing controllers by performing requests and generating responses through the actual `DispatcherServlet`.

Spring MVC Test builds on the familiar "mock" implementations of the Servlet API available in the spring-test module. This allows performing requests and generating responses without the need for running in a Servlet container. For the most part everything should work as it does at runtime with a few notable exceptions as explained in the section called “Differences between Out-of-Container and End-to-End Integration Tests”. Here is a JUnit 4 based example of using Spring MVC Test:

```java
// Static Imports
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@RunWith(SpringRunner.class)
@WebAppConfiguration
@ContextConfiguration("test-servlet-context.xml")
public class ExampleTests {

    @Autowired
    private WebApplicationContext wac;

    private MockMvc mockMvc;

    @Before
    public void setup() {
        this.mockMvc = MockMvcBuilders.webAppContextSetup(this.wac).build();
        // add controller
        // this.mockMvc = MockMvcBuilders.standaloneSetup(new AccountController()).build();
    }

    @Test
    public void getAccount() throws Exception {
        this.mockMvc.perform(get("/accounts/1").accept(MediaType.parseMediaType("application/json;charset=UTF-8")))
            .andExpect(status().isOk())
            .andExpect(content().contentType("application/json"))
            .andExpect(jsonPath("$.name").value("Lee"));
    }
}
```

## 26.2 Performing Requests

[#Performing Requests](https://docs.spring.io/spring/docs/4.3.x/spring-framework-reference/htmlsingle/#spring-mvc-test-server-performing-requests)

It’s easy to perform requests using any HTTP method:

```java
mockMvc.perform(post("/hotels/{id}", 42).accept(MediaType.APPLICATION_JSON));
```

You can also perform file upload requests that internally use MockMultipartHttpServletRequest so that there is no actual parsing of a multipart request but rather you have to set it up:

```java
mockMvc.perform(fileUpload("/doc").file("a1", "ABC".getBytes("UTF-8")));
```

You can specify query parameters in URI template style:

```java
mockMvc.perform(get("/hotels?foo={foo}", "bar"));
```

Or you can add Servlet request parameters representing either query of form parameters:

```java
mockMvc.perform(get("/hotels").param("foo", "bar"));
```

If application code relies on Servlet request parameters and doesn’t check the query string explicitly (as is most often the case) then it doesn’t matter which option you use. Keep in mind however that query params provided with the URI template will be decoded while request parameters provided through the param(…​) method are expected to already be decoded.

In most cases it’s preferable to leave out the context path and the Servlet path from the request URI. If you must test with the full request URI, be sure to set the contextPath and servletPath accordingly so that request mappings will work:

```java
mockMvc.perform(get("/app/main/hotels/{id}").contextPath("/app").servletPath("/main"))
```

Looking at the above example, it would be cumbersome to set the contextPath and servletPath with every performed request. Instead you can set up default request properties:

```java
public class MyWebTests {

    private MockMvc mockMvc;

    @Before
    public void setup() {
        mockMvc = standaloneSetup(new AccountController())
            .defaultRequest(get("/")
            .contextPath("/app").servletPath("/main")
            .accept(MediaType.APPLICATION_JSON).build();
    }
```

The above properties will affect every request performed through the MockMvc instance. If the same property is also specified on a given request, it overrides the default value. That is why the HTTP method and URI in the default request don’t matter since they must be specified on every request.

## 26.3 Defining Expectations

`andExpect`

```java
mockMvc.perform(get("/accounts/1")).andExpect(status().isOk());
```

`print()` 打印log

```java
mockMvc.perform(post("/persons"))
    .andDo(print())
    .andExpect(status().isOk())
    .andExpect(model().attributeHasErrors("person"));
```

`.andReturn()`

```java
MvcResult mvcResult = mockMvc.perform(post("/persons")).andExpect(status().isOk()).andReturn();
```

## 26.4 Filter Registrations

When setting up a MockMvc instance, you can register one or more Servlet Filter instances:

```java
mockMvc = standaloneSetup(new PersonController()).addFilters(new CharacterEncodingFilter()).build();
```

Registered filters will be invoked through via the MockFilterChain from spring-test, and the last filter will delegate to the DispatcherServlet.

## 26.5 Differences between Out-of-Container and End-to-End Integration Tests

[link](https://docs.spring.io/spring/docs/4.3.x/spring-framework-reference/htmlsingle/#spring-mvc-test-vs-end-to-end-integration-tests)

## 26.6 Client-Side REST Tests

[link](https://docs.spring.io/spring/docs/4.3.x/spring-framework-reference/htmlsingle/#spring-mvc-test-client)

## 26.7 HtmlUnit Integration

[link](https://docs.spring.io/spring/docs/4.3.x/spring-framework-reference/htmlsingle/#spring-mvc-test-server-htmlunit)