#Spring MVC from scratch

* fung.xu@corp.globalmarket.com 2016.10

This document is for my study on Spring MVC 4.x


## Software list
---

- IntelliJ IDEA 2016.01
- JDK 1.7
- Maven 3.2.5
- Tomcat 8.5.5

## 1. Create project from archetype:
---

### 1.1 Download the archetype-catalog from central repository

So we can accelerate the speed for next step, It's very slow from China. If you run it serval times, this will save you much time.

```
cd ~/.m2
wget http://repo1.maven.org/maven2/archetype-catalog.xml
```

### 2.1 Generate the project template 

```
mvn archetype:generate \
	-DgroupId=com.fung.test \
	-DartifactId=mvctest2 \
	-DarchetypeCatalog=internal \
	-DarchetypeGroupId=org.apache.maven.archetypes \
	-DarchetypeArtifactId=maven-archetype-webapp \
	-DinteractiveMode=false
``` 

We will get a bunch of files like this:

```
./mvctest2
./mvctest2/pom.xml
./mvctest2/src
./mvctest2/src/main
./mvctest2/src/main/resources
./mvctest2/src/main/webapp
./mvctest2/src/main/webapp/index.jsp
./mvctest2/src/main/webapp/WEB-INF
./mvctest2/src/main/webapp/WEB-INF/web.xml

```

## 3. Prepare pom.xml
---

### 3.1 Create a empty pom.xml file:

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.fung.test</groupId>
  <artifactId>mvctest2</artifactId>

  <!-- This project will be finnally got a war package-->
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>mvctest2 Maven Webapp</name>
  <url>http://maven.apache.org</url>

  <build>
  	<!-- the war file name --> 
    <finalName>${artifactId}</finalName>
  </build>

</project>
```

run `mvn clean package`, should got a file  in `target/mvctest2.war`


### 3.2 Add spring mvc dependency to pom.xml, like following, we use 4.4.3.RELEASE for this testing:

```
...
  <properties>
    <java-version>1.8</java-version>
    <spring.version>4.3.3.RELEASE</spring.version>
  </properties>

  <dependencies>

  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>${spring.version}</version>
  </dependency>

  </dependencies>
...
```

run `mvn clean package` again , should got more jar files as following. According to the list, we know the mininum required jar libary is here

```
./mvctest2/WEB-INF/lib/commons-logging-1.2.jar
./mvctest2/WEB-INF/lib/spring-aop-4.3.3.RELEASE.jar
./mvctest2/WEB-INF/lib/spring-beans-4.3.3.RELEASE.jar
./mvctest2/WEB-INF/lib/spring-context-4.3.3.RELEASE.jar
./mvctest2/WEB-INF/lib/spring-core-4.3.3.RELEASE.jar
./mvctest2/WEB-INF/lib/spring-expression-4.3.3.RELEASE.jar
./mvctest2/WEB-INF/lib/spring-web-4.3.3.RELEASE.jar
./mvctest2/WEB-INF/lib/spring-webmvc-4.3.3.RELEASE.jar

```
### 3.3 Study the stuff we download just now:

- The `spring-core` and `spring-beans` modules provide the fundamental parts of the framework, including the IoC and Dependency Injection features. 

- The `spring-context` module builds on the solid base provided by the Core and Beans modules: it is a means to access objects in a framework-style manner that is similar to a JNDI registry.

- The `spring-expression` module provides a powerful Expression Language for querying and manipulating an object graph at runtime. 

- The `spring-aop` module provides an AOP Alliance-compliant aspect-oriented programming implementation allowing you to define, for example, method interceptors and pointcuts to cleanly decouple code that implements functionality that should be separated.

- The `spring-web` module provides basic web-oriented integration features such as multipart file upload functionality and the initialization of the IoC container using Servlet listeners and a web-oriented application context. It also contains an HTTP client and the web-related parts of Spring’s remoting support.

- The `spring-webmvc` module (also known as the Web-Servlet module) contains Spring’s model-view-controller (MVC) and REST Web Services implementation for web applications. Spring’s MVC framework provides a clean separation between domain model code and web forms and integrates with all of the other features of the Spring Framework.

This diagram from springsource.org, illustrate those relationships between spring components.

![](http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/images/overview-full.png)

## 4. Prepare web.xml & Spring mvc config file
---

### 4.1 Add Spring Servlet and mapping to web.xml, the seckelton of web.xml should be ready, after we create the project from archetype ``
```
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>

  <display-name>Archetype Created Web Application</display-name>

  <servlet>
    <servlet-name>mvc-dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
  </servlet>

  <servlet-mapping>
    <servlet-name>mvc-dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
  
</web-app>
```

### 4.2 Create a file in `webapp/WEB-INF/mvc-dispatcher-servlet.xml`


```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

</beans>
```

## 5. Work with IntelliJ IDEA
---

> Why we use IntelliJ?

> * Fast code developing.
> * Reload the class to Tomcat without restart the whole server, it's time consuming work before.
> * IntelliJ is better then eclipse from my side

### 5.1 Import the project by maven project type

  
### 5.2 Setting for debug with Tomcat

* Create a Tomcat configuration profile as, 

![](http://newimg.globalmarket.com/group0/2c/7c/8345f16216d5d05daec06507f8f8.png)

* remember to choose "xxx:war exploded" artifact from Deployment

![](http://newimg.globalmarket.com/group0/47/02/82cbc0bc30b06834aa469b243ceb.png)

### 5.3 Deploy to Tomcat

^D to Debug current project to tomcat

![](http://newimg.globalmarket.com/group0/45/0c/7cbe260d3513c178e29668b644bd.png)

### 5.4 Make changes to currnet java file and load to Tomcat

shift + command + fn + F9 to compile current java file

And IDE will detect the class is change and prompt you to reload the tomcat

![](http://newimg.globalmarket.com/group0/1f/6b/91877adef94524c2a896571d7cf9.png)

when you click "Yes" the code will be reload.

![](http://newimg.globalmarket.com/group0/ff/a1/4136a7fe7cfbf7809abf47c3e98c.png)

## 6. Create a simple app
---

### 6.1 Create a java and jsp file

* java/com/fung/MainController.java

```
package com.fung.test;



import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;


@Controller
public class MainController {

    @RequestMapping(value = "/fung", method = RequestMethod.GET)
    public String index() {
        return "fung";
    }
}
```

* webapp/WEB-INF/pages/fung.jsp

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    Hello, Fung~
</body>
</html>
```

### 6.2 add following line to `mvc-dispatcher-servlet.xml`

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--指明 controller 所在包，并扫描其中的注解-->
    <context:component-scan base-package="com.feng.test"/>

    <!-- 静态资源(js、image等)的访问 -->
    <mvc:default-servlet-handler/>

    <!-- 开启注解 -->
    <mvc:annotation-driven/>

    <!--ViewResolver 视图解析器-->
    <!--用于支持Servlet、JSP视图解析-->
    <bean id="jspViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
        <property name="prefix" value="/WEB-INF/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>

```
### 6.3 Deploy to tomcat

![](http://newimg.globalmarket.com/group0/2f/46/d32656563aca8aebd7d11527c063.png) 


### 6.4 We got problems!

![](http://newimg.globalmarket.com/group0/14/5e/437d087b734646a97b6df5085a61.png)

add following lines to `pom.xm` and deploy the project again

```
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
```

### 6.5 More complicated case:

#### 6.5.1 Accept value and return to page?

* Add one more controller method to `MainController.java`:

```
@RequestMapping("/sayhello/{name}")
public String updateBlog(@PathVariable("name") String name, ModelMap modelMap) {

    modelMap.addAttribute("name", name);
    return "sayhello";
}
```

* Add one jsp file in `WEB-INF/pages/sayhello.jsp`

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    Hello, ${name}
</body>
</html>
```

* But we got this, why?

![](http://newimg.globalmarket.com/group0/a8/14/5dedcf8b5c2980808964e923c39f.png)

add following line to jsp is ok, but it's not a good solution, the jsp el function was disabled by default in version 2.3 .

```
<%@ page isELIgnored="false" %>
```

[This link explain the details](http://stackoverflow.com/questions/7374821/jsp-el-stuff-syntax-not-working)

We change the web.xml which comes from maven-archetype-webapp in the first step,  from: 

```
<!DOCTYPE web-app PUBLIC
        "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
        "http://java.sun.com/dtd/web-app_2_3.dtd" >
<web-app>
```

to:

```
<web-app xmlns="http://java.sun.com/xml/ns/j2ee"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd"
     version="2.4">

```

#### 6.5.2 Accept url parameter, form POST and json POST value

* GET request by parameter/form POST, add following to `MainController.java`


```
    @RequestMapping(value = "/sayyes")
    public String sayyes(@RequestParam("name") String name, ModelMap modelMap) {
        modelMap.addAttribute("name", name);
        return "sayhello";
    }

```

* we introduce the PAW test client to simulate above case, by form POST

![](http://newimg.globalmarket.com/group0/5d/23/29213978faac82d3bc46a1343de8.png)

* what if JSON ?

add following method to MainController.java

```
    @ResponseBody
    @RequestMapping(value = "/json")
    public List<String> sayJson() {

        List<String> list = new ArrayList<String>();
        list.add("fung");
        list.add("xu");
        return list;
    }
```

When we visit the URL, we got this problem

![](http://newimg.globalmarket.com/group0/c1/12/d5aeae54a29149b32299045a3acb.png)

We need to import jackson lib, should use 2.x while we use 4.x spring mvc framework.

```
...

  <properties>
    <java-version>1.8</java-version>
    <spring.version>4.3.3.RELEASE</spring.version>
    <jackson.version>2.6.3</jackson.version>
  </properties>

...

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
            <version>${jackson.version}</version>
        </dependency>

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>${jackson.version}</version>
        </dependency>
```

* What if JSON output?


```
    @ResponseBody
    @RequestMapping(value = "/jsonObj")
    public UserModel sayJsonObj() {
        UserModel user = new UserModel();
        user.setName("fung");
        user.setValue("xu");
        return user;
    }
```


* What if JSON input ?

```
    @ResponseBody
    @RequestMapping(value = "/jsonInput")
    public UserModel inputObj(@RequestBody UserModel user) {
        return user;
    }
```

with picture , with truth:

![](http://newimg.globalmarket.com/group0/45/4b/d6f76e49bfb7be69b958eb99b1c5.png)


### 7. Spring-session with redis
---

#### 7.1 Add dependency

```
    <!-- Spring Data Redis -->
    <dependency>
      <groupId>org.springframework.data</groupId>
      <artifactId>spring-data-redis</artifactId>
      <version>1.7.4.RELEASE</version>
    </dependency>

    <!-- Spring Session -->
    <dependency>
      <groupId>org.springframework.session</groupId>
      <artifactId>spring-session</artifactId>
      <version>1.2.2.RELEASE</version>
    </dependency>
```

we got more jar from it

![](http://newimg.globalmarket.com/group0/35/5a/56955bfb686b485aa358727505e6.png)

And introduce more problems

![](http://newimg.globalmarket.com/group0/21/37/1e9fb6b722e72d0770ea12678349.png)

> See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
... which says: This error is reported when the org.slf4j.impl.StaticLoggerBinder class could not be loaded into memory. This happens when no appropriate SLF4J binding could be found on the class path. Placing one (and only one) of slf4j-nop.jar, slf4j-simple.jar, slf4j-log4j12.jar, slf4j-jdk14.jar or logback-classic.jar on the class path should solve the problem.

> As of SLF4J version 1.6, in the absence of a binding, SLF4J will default to a no-operation (NOP) logger implementation.

* Solution: add the log4j lib

```
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
      <version>1.7.21</version>
    </dependency>

```

* Add `log4j.properties` in `resources` folder


```
log4j.rootLogger=info, ServerDailyRollingFile, stdout

log4j.appender.ServerDailyRollingFile=org.apache.log4j.DailyRollingFileAppender

log4j.appender.ServerDailyRollingFile.DatePattern='.'yyyy-MM-dd

log4j.appender.ServerDailyRollingFile.File=logs/notify-subscription.log

log4j.appender.ServerDailyRollingFile.layout=org.apache.log4j.PatternLayout

log4j.appender.ServerDailyRollingFile.layout.ConversionPattern=%d - %m%n

log4j.appender.ServerDailyRollingFile.Append=true

log4j.appender.stdout=org.apache.log4j.ConsoleAppender

log4j.appender.stdout.layout=org.apache.log4j.PatternLayout

log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH\:mm\:ss} %p [%c] %m%n
```

* enh, how about logback . log4j is old enough

replace slf4j-log4j12 with 

```
<dependency>
  <groupId>ch.qos.logback</groupId>
  <artifactId>logback-classic</artifactId>
  <version>1.1.2</version>
</dependency>
<dependency>
  <groupId>ch.qos.logback</groupId>
  <artifactId>logback-core</artifactId>
  <version>1.1.2</version>
</dependency>
```

create resources/logback.xml file 


```
<?xml version="1.0" encoding="UTF-8"?>

<configuration scan="false" scanPeriod="60 seconds" debug="false">
    <property name="LOG_HOME" value="." />
    <property name="appName" value="fung"></property>
    <appender name="stdout" class="ch.qos.logback.core.ConsoleAppender">
        <Encoding>UTF-8</Encoding>
        <layout class="ch.qos.logback.classic.PatternLayout">
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
        </layout>
    </appender>
    <appender name="appLogAppender" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <Encoding>UTF-8</Encoding>
        <file>${LOG_HOME}/${appName}.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_HOME}/${appName}-%d{yyyy-MM-dd}-%i.log</fileNamePattern>
            <MaxHistory>365</MaxHistory>
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>100MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
        </rollingPolicy>
        <layout class="ch.qos.logback.classic.PatternLayout">
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [ %thread ] - [ %-5level ] [ %logger{50} : %line ] - %msg%n</pattern>
        </layout>
    </appender>
    <logger name="org.hibernate" level="error" />
    <logger name="o.s.w.s.m.m.a" level="debug" additivity="true"/>
    <logger name="com.fung.test" level="info" additivity="true">
        <appender-ref ref="appLogAppender" />
    </logger>
    <root level="info">
        <appender-ref ref="stdout" />
    </root>
</configuration>
```

#### 7.2 Add more dependency as 


```
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
</dependency>

<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-redis</artifactId>
    <version>1.7.4.RELEASE</version>
</dependency>

<dependency>
    <groupId>org.springframework.session</groupId>
    <artifactId>spring-session</artifactId>
    <version>1.2.2.RELEASE</version>
</dependency>

<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-pool2</artifactId>
    <version>2.4.2</version>
</dependency>

```


#### 7.3 Add more listner to web.xml

```
<filter>
    <filter-name>springSessionRepositoryFilter</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
</filter>
<filter-mapping>
    <filter-name>springSessionRepositoryFilter</filter-name>
    <url-pattern>/*</url-pattern>
    <dispatcher>REQUEST</dispatcher>
    <dispatcher>ERROR</dispatcher>
</filter-mapping>

```


#### 7.4 Define new bean

```
    <bean class="org.springframework.session.data.redis.config.annotation.web.http.RedisHttpSessionConfiguration"/>
    <bean class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory">
        <property name="hostName" value="fung-redis.gmit.com" />
        <property name="port" value="6379" />
        <property name="database" value="8" />
    </bean>
```

#### 7.5 Add anotation for session attribute 

##### 7.5.1 Session by ModelMap provided by Spring(Recommended)

* Add Session Attribute to MainController.java

```
...
@Controller
@SessionAttributes("user")
public class MainController {
...

```
* Add more function :

```
    @ResponseBody
    @RequestMapping(value = "/setSession")
    public UserModel setSession(ModelMap model){
        UserModel user = new UserModel();
        user.setName("fung");
        user.setValue("Model");
        model.addAttribute("user",user);
        return user;
    }


    @ResponseBody
    @RequestMapping(value = "/getSession")
    public UserModel getSession(ModelMap model){
        UserModel user;
        user = (UserModel) model.get("user");
        return user;
    }
```

##### 7.5.2 Session by HttpSession provided by Servelet

* introduce javax api in maven project 

```

    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>servlet-api</artifactId>
      <version>2.5</version>
      <scope>provided</scope>
    </dependency>

```

* Add more function :

```
    @ResponseBody
    @RequestMapping("/getHttpSession")
    public UserModel getHttpSession(HttpSession session){
        UserModel user = (UserModel) session.getAttribute("user");
        return user;
    }

    @ResponseBody
    @RequestMapping("/setHttpSession")
    public UserModel setHttpSession(HttpSession session){
        UserModel user = new UserModel();
        user.setName("fung");
        user.setValue("Session");
        session.setAttribute("user",user);
        return user;
    }
```



#### 7.6 Check with redis

![](http://newimg.globalmarket.com/group0/df/1d/6e22f3b75e99744f74e01371e3dd.png)


* if we mix above cases , we found this result. We visit the url one by one, and got something like `{"name":"fung","value":"Model"}`. We inspect the field value. 

|#  |Set|Get|value|Set|Get|value|
|----|----|----|----|---|---|:-----|
|1  |SetSession|GetSession|Model|SetHttpSession|GetHttpSession|Model|
|2  |SetSession|GetHttpSession|Model|SetHttpSession|GetSession|Model|
|3  |SetHttpSession|GetSession|Session|SetSession|GetHttpSession|Model|
|4  |SetHttpSession|GetHttpSession|Session|SetSession|GetSession|Model|


You can see , Set Value through HttpSession can be override by SetSessionAttribute 

### 8. Access with Database
---

#### 8.1 jdbc template

##### 8.1.1 Add dependency to `pom.xml`

```
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>${spring.version}</version>
    </dependency>
```

this will introduce:

  - spring-tx module supports programmatic and declarative transaction management for classes that implement special interfaces and for all your POJOs (Plain Old Java Objects).

  - spring-jdbc module provides a JDBC-abstraction layer that removes the need to do tedious JDBC coding and parsing of database-vendor specific error codes.

  - spring-oxm module provides an abstraction layer that supports Object/XML mapping implementations such as JAXB, Castor, XMLBeans, JiBX and XStream.

##### 8.1.2 Add one more Spring config XML file



