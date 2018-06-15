---
layout: master
title:  Spring Boot 多数据源配置
description:  yuminglang.com,提供web前端、java、linux、数据库相关技术知识共享。
categories: Java
syntax: 9
---
在Spring Boot中配置使用多数据源示例代码和简要说明。

### 1.Maven引入数据库对应的驱动包,
```sh
oracle：
        <dependency>
            <groupId>com.oracle</groupId>
            <artifactId>ojdbc6</artifactId>
            <version>12.1.0.1-atlassian-hosted</version>
        </dependency>
mysql：
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
sqlserver:
        <dependency>
            <groupId>com.microsoft.sqlserver</groupId>
            <artifactId>mssql-jdbc</artifactId>
        </dependency>
```
### 2.Spring数据源连接application.yml配置
```sh
spring:
  datasource:
    emr:
      driver-class-name: oracle.jdbc.driver.OracleDriver
      jdbc-url: jdbc:oracle:thin:@192.168.0.128:1521:xe
      username: system
      password: oracle
    lis:
      driver-class-name: com.microsoft.sqlserver.jdbc.SQLServerDriver
      jdbc-url: jdbc:sqlserver://192.168.0.126:1433;DatabaseName=test
      username: sa
      password: 1qaz!QAZ
    mysql:
      driver-class-name: com.mysql.jdbc.Driver
      jdbc-url: jdbc:mysql://localhost/dbgirl?characterEncoding=utf-8&useSSL=false
      username: root
      password: 1qaz!QAZ
```
### 3.Spring Jpa连接application.yml配置
```sh
  jpa:
    show-sql: true
    open-in-view: true
    hibernate:
      naming:
        physical-strategy: org.springframework.boot.orm.jpa.hibernate.SpringPhysicalNamingStrategy
```
### 4.Oracle配置:
```java
@Configuration
@EnableTransactionManagement
@EnableJpaRepositories(
        entityManagerFactoryRef = "oracleEntityManagerFactory",
        transactionManagerRef = "oracleTransactionManager",
        basePackages = {"com.yuminglang.repository.oracle"}
)
public class OracleConfig {
    @Primary
    @Bean(name="oracleDataSource")
    @ConfigurationProperties(prefix="spring.datasource.oracle")
    public DataSource dataSource() {
        return DataSourceBuilder.create().build();
    }
    @Primary
    @Bean(name="oracleEntityManagerFactory")
    public LocalContainerEntityManagerFactoryBean oracleEntityManagerFactory(
            EntityManagerFactoryBuilder builder,
            @Qualifier("oracleDataSource") DataSource dataSource
    ) {
        return builder.dataSource(dataSource).packages("com.yuminglang.domain").persistenceUnit("oracle").build();
    }
    @Primary
    @Bean(name="oracleTransactionManager")
    public PlatformTransactionManager oracleTransactionManager(
            @Qualifier("oracleEntityManagerFactory") EntityManagerFactory oracleEntityManagerFactory ) {
        return new JpaTransactionManager(oracleEntityManagerFactory);
    }
}
```
### 5.Mysql配置
```java
@Configuration
@EnableTransactionManagement
@EnableJpaRepositories(
        entityManagerFactoryRef = "mysqlEntityManagerFactory",
        transactionManagerRef = "mysqlTransactionManager",
        basePackages = {"com.yuminglang.repository.mysql"}
)
public class MysqlConfig {
    @Bean(name = "mysqlDataSource")
    @ConfigurationProperties(prefix = "spring.datasource.mysql")
    public DataSource dataSource() {
        return DataSourceBuilder.create().build();
    }
    @Bean(name = "mysqlEntityManagerFactory")
    public LocalContainerEntityManagerFactoryBean oracleEntityManagerFactory(
            EntityManagerFactoryBuilder builder,
            @Qualifier("mysqlDataSource") DataSource dataSource
    ) {
        return builder.dataSource(dataSource).packages("com.yuminglang.domain").persistenceUnit("mysql").build();
    }
    @Bean(name = "mysqlTransactionManager")
    public PlatformTransactionManager oracleTransactionManager(
            @Qualifier("mysqlEntityManagerFactory") EntityManagerFactory mysqlEntityManagerFactory) {
        return new JpaTransactionManager(mysqlEntityManagerFactory);
    }
}
```
### 6.SqlServer配置
```java
@Configuration
@EnableTransactionManagement
@EnableJpaRepositories(
        entityManagerFactoryRef = "sqlserverEntityManagerFactory",
        transactionManagerRef = "sqlserverTransactionManager",
        basePackages = {"com.yuminglang.repository.sqlserver"}
)
public class SqlserverConfig {
    @Bean(name="sqlserverDataSource")
    @ConfigurationProperties(prefix="spring.datasource.sqlserver")
    public DataSource dataSource() {
        return DataSourceBuilder.create().build();
    }
    @Bean(name="sqlserverEntityManagerFactory")
    public LocalContainerEntityManagerFactoryBean sqlserverEntityManagerFactory(
            EntityManagerFactoryBuilder builder,
            @Qualifier("sqlserverDataSource") DataSource dataSource
    ) {
        return builder.dataSource(dataSource).packages("com.yuminglang.domain").persistenceUnit("sqlserver").build();
    }
    @Bean(name="sqlserverTransactionManager")
    public PlatformTransactionManager sqlserverTransactionManager(
            @Qualifier("sqlserverEntityManagerFactory") EntityManagerFactory sqlserverEntityManagerFactory ) {
        return new JpaTransactionManager(sqlserverEntityManagerFactory);
    }
}
```
### 6.根据以上配置，数据源将分别作用以下三个包中:
```sh
com.yuminglang.repository.oracle
com.yuminglang.repository.mysql
com.yuminglang.repository.sqlserver
```
### 7.简要说明：  
a.使用多数据源时，其中一个连接需要添加`@Primary`注解。  
b.根据`@EnableJpaRepositories`注解中的`basePackages`配置数据源作用范围,  
c.`@ConfigurationProperties`中解析`application.yml`中的连接数据源的用户名/密码/连接url/驱动信息。  
   