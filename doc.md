# Java设计模式

创建型：简单工厂模式、抽象工厂模式、单例模式、多例模式、构建者模式。

结构型：适配器、装饰器、代理模式、桥接模式、外观模式。

-代理模式：代理模式在不改变原始类接口的条件下，为原始类定义一个代理类，主要目的是控制访问，而非加强功能，这是它跟装饰器模式最大的不同。  
-装饰器模式：装饰者模式在不改变原始类接口的情况下，对原始类功能进行增强，并且支持多个装饰器的嵌套使用。  
-适配器模式：适配器模式是一种事后的补救策略。适配器提供跟原始类不同的接口，而代理模式、装饰器模式提供的都是跟原始类相同的接口。  
-桥接模式：桥接模式的目的是将接口部分和实现部分分离，从而让它们可以较为容易、也相对独立地加以改变。桥接模式通过组合关系来替代继承关系，避免继承层次的指数级爆炸。

**代理、桥接、装饰器、适配器 4 种设计模式的区别:**  
代理、桥接、装饰器、适配器，这 4 种模式是比较常用的结构型设计模式。它们的代码结构非常相似。笼统来说，它们都可以称为 Wrapper 模式，也就是通过 Wrapper 类二次封装原始类。

行为型：策略模式、责任链模式、观察者模式、访问者模式。

## 1 工厂方法模式
### 1.1 工厂方法模式-案例1
AOP获取AopProxy，进一步获取目标代理类
| 模块 | spring-aop |
| ---- | ---------- |
| 客户类(调用工厂) | org.springframework.aop.framework.ProxyFactory |
| 工厂接口 | org.springframework.aop.framework.AopProxyFactory |
| 工厂实现类 | org.springframework.aop.framework.DefaultAopProxyFactory |
| 产品接口 | org.springframework.aop.framework.AopProxy |
| 产品接口实现类 | org.springframework.aop.framework.JdkDynamicAopProxy、  org.springframework.aop.framework.ObjenesisCglibAopProxy |

### 1.2 工厂方法模式-案例2
slf4j获取Logger实例
| 模块 | slf4j-api |
| ---- | ---------- |
| 客户类(调用工厂) | org.slf4j.LoggerFactory |
| 工厂接口 | org.slf4j.ILoggerFactory |
| 工厂实现类 | org.apache.logging.slf4j.Log4jLoggerFactory、  ch.qos.logback.classic.LoggerContext |
| 产品接口 | org.slf4j.Logger |
| 产品接口实现类 | org.apache.logging.slf4j.Log4jLogger、  ch.qos.logback.classic.Logger |

### 1.3 工厂方法模式-案例3
mybatis获取Transaction实例
| 模块 | slf4j-api |
| ---- | ---------- |
| 客户类(调用工厂) | - |
| 工厂接口 | org.apache.ibatis.transaction.TransactionFactory |
| 工厂实现类 | org.apache.ibatis.transaction.jdbc.JdbcTransactionFactory、  org.apache.ibatis.transaction.managed.ManagedTransactionFactory、  org.mybatis.spring.transaction.SpringManagedTransactionFactory |
| 产品接口 | org.apache.ibatis.transaction.Transaction |
| 产品接口实现类 | org.apache.ibatis.transaction.jdbc.JdbcTransaction、  org.apache.ibatis.transaction.managed.ManagedTransaction、  org.mybatis.spring.transaction.SpringManagedTransaction |


## 2 责任链模式
### 2.1 责任链模式-案例1
AOP执行链
| 模块 | Spring-Aop |
| ---- | ---------- |
| 链管理类 | org.springframework.aop.framework.ReflectiveMethodInvocation |
| 链节点类型 | org.springframework.aop.framework.InterceptorAndDynamicMethodMatcher、  org.aopalliance.intercept.MethodInterceptor |
| 链节点 | org.springframework.transaction.interceptor.TransactionInterceptor、  org.springframework.aop.aspectj.AspectJAroundAdvice、  org.springframework.aop.framework.adapter.MethodBeforeAdviceInterceptor、  org.springframework.aop.framework.adapter.AfterReturningAdviceInterceptor、  …… |


# 3 构建者模式
特点：
- (1) 目标类不设置setter方法。
- (2) 构建类放在与目标类同一个包内（或构建类直接放在目标中），然后通过点(.)运算符给目标类属性赋值。

关于Builder模式，我们一定要分清和模板方法的区别，其实就是到底谁承担了"监工"的责任，
在模板方法中父类承担了这个责任，而在Builder中，有另外一个专门的类来完成这样的操作，
这样做的好处是类的隔离，比如说在Main中，用户根本就不知道有Builder这个抽象类，
同样的Director这个监工的根本就不管到底是哪一个实现类，因为任何一个都会被转换为父类，
然后进行处理（面向抽象编程的思想），因此很好的实现了隔离，同样的这样设计的好处是复用了，
隔离的越好复用起来就越方便，我们完全可以思考，假如还有另外一个监工，使用了不同的construct方法来组装
那么对于原来的代码我们不用做任何的修改，只用增加这样的一个监工类，然后定义好相应的方法就好了，
之后再Main中使用，这样的一种思想使得我们不用修改源代码，复用（Builder以及其子类）就很方便了，
同样的，如果想增加一个新的Builder的子类，只要照着父类的方法进行填充，再加上自己的方法就好了，
完全不用修改代码，这也是一种复用，因此这种复用（组件）的思想在设计模式中随处可见，
本质就是高内聚低耦合，组件开发，尽量不修改原来的代码，有可扩展性，理解了这一点，我们再看看模板方法，责任全放
如果责任需要改变，则必须要修改父类中的责任方法了，这样就修改了原来的代码，不利于复用，这也是两者的本质区别。


### 3.1 构建者模式-案例1
mybatis环境类Environment的创建
| 模块 | Mybatis |
| ---- | ------- |
| 目标类 | org.apache.ibatis.mapping.Environment |
| 构建类 | org.apache.ibatis.mapping.Environment.Builder |


## 4 适配器模式
分两种：
- 类适配器
- 对象适配器

适用场景：
- 重构第三方登录自由适配场景

### 4.1 对象适配器模式1
#### 4.1.1 对象适配器模式-案例1
log4j适配到slf4j
| 模块 | Log4j |
| ---- | ------- |
| 目标接口 | org.slf4j.spi.LocationAwareLogger |
| 现有类 | org.apache.log4j.Logger |
| 适配器类 | org.slf4j.impl.Log4jLoggerAdapter |

### 4.2 类适配器模式
#### 4.2.1 类适配器模式-案例1
log4j2适配到slf4j
| 模块 | Log4j |
| ---- | ------- |
| 目标接口 | org.slf4j.spi.LocationAwareLogger |
| 现有类 | org.apache.log4j.Logger |
| 适配器类 | org.slf4j.impl.Log4jLoggerAdapter |


## 5 装饰器模式
装饰器模式又称为包装（Wrapper）模式。装饰器模式以多客户端透明的方式扩展对象的功能，是继承关系的一个替代方案。

装饰器模式的结构:  
通常给对象添加功能，要么直接修改对象添加相应的功能，要么派生子类来扩展，抑或是使用对象组合的方式。显然，直接修改对应的类的方式并不可取，在面向对象的设计中，我们应该尽量使用组合对象而不是继承对象来扩展和复用功能，装饰器模式就是基于对象组合的方式的。  
装饰器模式以对客户端透明的方式动态地给一个对象附加上了更多的责任。换言之，客户端并不会角色对象在装饰前和装饰后有什么不同。装饰器模式可以在不用创建更多子类的情况下，将对象的功能加以扩展。

装饰器模式中的角色有：
- (1) 抽象构件角色
  给出一个抽象接口，以规范准备接受附加责任的对象
- (2) 具体构件角色
  定义一个将要接受附加责任的类
- (3) 装饰角色
  持有一个构建对象的实例，并定义一个与抽象构件接口一致的接口
- (4) 具体装饰角色
  负责给构建对象贴上附加的责任

### 5.1 装饰器模式模式-案例1
mybatis缓存Cache使用装饰器模式
| 模块 | Mybatis |
| ---- | ------- |
| 目标接口 | org.apache.ibatis.cache.Cache |
| 装饰器类 | org.apache.ibatis.cache.decorators.LoggingCache、  org.apache.ibatis.cache.decorators.SynchronizedCache、  org.apache.ibatis.cache.decorators.BlockingCache、  ... |


## 6 观察者模式
具体案例可查看JDK中的应用
java.util.Observer（观察者）
java.util.Observable（主题）


## 7 代理模式
代理模式的定义很简单：  
给某一对象提供一个代理对象，并由代理对象控制对原对象的引用。

代理模式的结构:  
有些情况下，一个客户不想或者不能够直接引用一个对象，可以通过代理对象在客户端和目标对象之间起到中介作用。  
代理模式中的角色有：
- (1) 抽象对象角色  
  声明了目标对象和代理对象的共同接口，这样一来在任何可以使用目标对象的地方都可以使用代理对象
- (2) 目标对象角色  
  定义了代理对象所代表的目标对象
- (3) 代理对象角色  
  代理对象内部含有目标对象的引用，从而可以在任何时候操作目标对象；代理对象提供一个与目标对象相同的接口，以便可以在任何时候替代目标对象


### 7.1 静态代理
示例：  
这里模拟的是作为访问网站的场景，以新浪网举例。  
我们通常访问新浪网，几乎所有的Web项目尤其是新浪这种大型网站，是不可能采用集中式的架构的，使用的一定是分布式的架构。
分布式架构对于用户来说，我们发起链接的时候，链接指向的并不是最终的应用服务器，而是代理服务器比如Nginx，用以做负载均衡。
所以，我们的例子，简化来说就是用户访问新浪网-->代理服务器-->最终服务器

静态代理的缺点：  
静态代理的特点是静态代理的代理类是程序员创建的，在程序运行之前静态代理的.class文件已经存在了。
从静态代理模式的代码来看，静态代理模式确实有一个代理对象来控制实际对象的引用，并通过代理对象来使用实际对象。这种模式在代理量较小的时候还可以，但是代理量一大起来，就存在着两个比较大的缺点：

- 静态代理的内容，即NginxProxy的路由选择这几行代码，只能服务于Server接口而不能服务于其他接口，如果其它接口想用这几行代码，比如新增一个静态代理类。久而久之，由于静态代理的内容无法复用，必然造成静态代理类的不断庞大
- Server接口里面如果新增了一个方法，比如getPageData(String url)方法，实际对象实现了这个方法，代理对象也必须新增方法getPageData(String url)，去给getPageData(String url)增加代理内容（假如需要的话）


### 7.2 动态代理
动态代理的优点：
- 最直观的，类少了很多
- 代理内容也就是InvocationHandler接口的实现类可以复用，可以给A接口用、也可以给B接口用，A接口用了InvocationHandler接口实现类A的代理，不想用了，可以方便地换成InvocationHandler接口实现B的代理
- 最重要的，用了动态代理，就可以在不修改原来代码的基础上，就在原来代码的基础上做操作，这就是AOP即面向切面编程



# Mybatis

## 1 原生mybatis执行流程
1. 解析mybatis配置文件，调用SqlSessionFactoryBuilder().build()方法生成一个DefaultSqlSessionFactory对象。
    - 1.1 通过XmlConfigBuilder创建一个Configuration对象。
    - 1.2 通过XmlConfigBuilder.parse()读取mybatis配置文件。
    - 1.3 通过XmlMapperBuilder.parse()读取并解析Mapper文件。
        - 1.3.1 调用configurationElement(this.parser.evalNode("/mapper"));解析Mapper文件中的各种元素节点，如/mapper/resultMap、/mapper/sql、select|insert|update|delete节点。解析生成的内容都会存入Configuration对象中，如保存每条sql语句对应的MappedStatement对象。
        - 1.3.2 调用bindMapperForNamespace();里面会继续调用Configuration.addMapper(boundType)为每个配置文件生成一个对应的MapperProxyFactory对象，并注册到Configuration中mapperRegistry变量中。
    - 1.4 SqlSessionFactoryBuilder().build(configuration);创建一个DefaultSqlSessionFactory对象。

2. 调用DefaultSqlSessionFactory#openSession()方法获取到一个DefaultSqlSession对象。

3. 调用SqlSession#getMapper()方法获取到一个Dao接口的代理对象。

4. Dao接口函数调用，最终执行代理对象MapperProxy#invoke()方法。
    - 4.1 MapperProxy#invoke()中首先创建一个MapperMethod对象，并注入到MapperProxy.PlainMethodInvoker中生成一个MapperProxy.PlainMethodInvoker对象。
    - 4.2 调用org.apache.ibatis.binding.MapperProxy.PlainMethodInvoker#invoke()方法，该方法体中继续调用this.mapperMethod.execute(sqlSession, args);
    - 4.3 MapperMethod#execute()中首先会对传入的参数进行包装，然后根据sql类型选择调用传入的sqlSession对象的insert|delete|update|select方法。
      如果传入的是sqlSession是SqlSessionTemplate，则会再生成一个sqlSession代理对象，进一步执行org.mybatis.spring.SqlSessionTemplate.SqlSessionInterceptor#invoke()方法。
        - (1) 调用SqlSessionUtils.getSqlSession()获取一个DefaultSqlSession对象。实际调用传入的sessionFactory的openSession(executorType)方法，而openSession(executorType)最终调用的是DefaultSqlSessionFactory#openSessionFromDataSource()方法。
        - (2) 通过反射执行获取到的DefaultSqlSession对象的增删改查方法。

5. DefaultSqlSession增删改查方法的执行
    - 5.1 通过this.configuration.getMappedStatement(statement)获取到一个MappedStatement对象;
    - 5.2 调用DefaultSqlSession中的Executor实例对象(CachingExecutor)的update、query等方法完成增删改查。

6. 通过传入参数MappedStatement的getBoundSql()获取到一个boundSql对象。该方法中实现了sql语句的动态化参数解析。
   mybatis动态化参数解析：
   入口：`org.apache.ibatis.scripting.xmltags.DynamicSqlSource#getBoundSql`
    - (1) 将Mapper文件中select|insert|delete|update节点内的<where>、<if>、<include>等节点替换为相应的文本内容。
    - (2) 创建一个SqlSourceBuilder对象。
    - (3) 调用SqlSourceBuilder对象的parse()方法。该方法中创建了ParameterMappingTokenHandler和GenericTokenParser两个对象。
        - GenericTokenParser.parse(): 将sql语句中的所有#{}|\${}参数占位符替换为？。
        - ParameterMappingTokenHandler.handleToken():为每一个参数占位符#{}|\${}生成一个ParameterMapping对象，并存入到这个对象内的List集合中。ParameterMapping对象用于记录参数的javaType、jdbcType、typeHandler、resultMap等信息。
    - (4) 返回一个StaticSqlSource对象。该对象包含了第3步中将#{}|${}替换为?后的sql语句，及第3步中为参数占位符生成的ParameterMapping列表。
    - (5) 调用StaticSqlSource的getBoundSqlgetBoundSql()方法返回一个BoundSql对象。这个对象最后传入作为StatementHandler的属性，并在StatementHandler#parameterize()方法中使用具体的参数值替换掉sql语句中的?占位符。

7. CachingExecutororg.apache.ibatis.executor.CachingExecutor#query()方法
    - 7.1 先从二级缓存中获取结果集，如果存在则直接返回，不存在则再从一级缓存中获取。一级缓存中如果存在，则直接返回，不存在则从数据库中查询。
        - 二级缓存获取内容（命名空间级别，多个connection间共享）。首先通过事务缓存管理器TransactionalCacheManager获取到当前查询方法所属命名空间对应的事务缓存对象TransactionalCache，然后再通过事务缓存对象获取到与当前方法对应的缓存。
        - 一级缓存（connection级别，单个connection独享）获取内容。

8. 数据库中查询获取数据。
    - 8.1 通过传入的MappedStatement获取到全局Configuration对象。
    - 8.2 通过Configuration的newStatementHandler()方法获取一个StatementHandler对象。
    - 8.3 调用SimpleExecutor#prepareStatement()方法获取一个Statement对象。
        - 8.3.1 通过transaction对象获取一个connection连接。
        - 8.3.2 通过获取到的connection获取到一个PreparedStatement对象。
        - 8.3.3 使用具体的参数值替换掉PreparedStatement对象sql语句中的?占位符。
    - 8.4 调用StatementHandler对象的update、query方法。

9. 执行StatementHandler对象的query方法。
    - 9.1 调用的是Statement对象的execute方法通过JDBC完成SQL执行。
    - 9.2 调用ResultSetHandler.handleResultSets()方法处理结果集。


## ２ mybatis-spring
MapperFactoryBean(实现了FactoryBean)，该对象在getObject()中借助SqlSessionTemplate的Configuration的MapperResigty获取到一个和Dao接口关联的MapperProxyFactory对象，
然后通过获取到的MapperProxyFactory对象创建一个Dao接口的代理MapperProxy（该代理实现了InvocationHandler接口）。

SessionHolder包含sqlSession，sqlSession包含transaction，transaction包含connection。  
**事务传播方法执行链**：   
外围方法的代理调用 -》 匹配事务方法 -》 检索符合方法的拦截器 -》 创建代理方法调用链 -》 执行事务拦截器TransactionInterceptor -》获取事务管理器 -》 创建事务对象
-》判断事务对象中是否存在Connection，不存在则通过数据源创建新的connection -》 修改事务为手动提交 -》 将connection绑定到当前线程 -》 初始化事务同步 -》 代理调用下一个内嵌的事务方法。

1. 通过org.mybatis.spring.SqlSessionFactoryBean完成一些基本初始化。如：mybatis配置文件的解析、Mapper配置文件解析、spring事务对象引入、DefaultSqlSessionFactory对象创建等。
2. org.mybatis.spring.mapper.MapperScannerConfigurer中使用org.mybatis.spring.mapper.ClassPathMapperScanner扫描Dao接口所在包，生成BeanDefination。
    - 2.1 扫描完成后，遍历生成的BeanDefination，修改BeanDefination中的部分属性值。
        - 2.1.1 修改BeanDefination中beanClass属性值为org.mybatis.spring.mapper.MapperFactoryBean。用于在实例化时创建MapperFactoryBean对象。
        - 2.1.2 为BeanDefination中的PropertyValues添加属性("sqlSessionFactory", new RuntimeBeanReference(this.sqlSessionFactoryBeanName))。用于在实例化时注入DefaultSqlSessionFactory实例。

3. 为Service类注入的Dao接口实现类。
    - 3.1 通过扫描到的BeanDefination为每个Dao接口实例化一个MapperFactoryBean对象。由于MapperFactoryBean实现里FactoryBean<T>接口。因此为Service类注入的Dao接口实现类为MapperFactoryBean.getObject()返回的对象。
    - 3.2 实例化MapperFactoryBean对象时会在其父类org.mybatis.spring.support.SqlSessionDaoSupport中注入DefaultSqlSessionFactory。并为sqlSession属性创建一个org.mybatis.spring.SqlSessionTemplate对象。
    - 4.1 MapperFactoryBean调用this.getSqlSession().getMapper(this.mapperInterface);获取Mapper对象。即，通过其中的SqlSessionTemplate属性获取到一个MapperProxy对象。
    - 4.2 SqlSessionTemplate#getMapper()实际调用的是this.getConfiguration().getMapper(type, this);
    - 4.3 org.apache.ibatis.session.Configuration#getMapper实际调用的是this.mapperRegistry.getMapper(type, sqlSession);
    - 4.4 org.apache.ibatis.binding.MapperRegistry#getMapper()中首先获取到之前注册的MapperProxyFactory对象，然后通过MapperProxyFactory.newInstance(sqlSession)创建一个包含MapperProxy对象的代理对象，注入到Service类中。



# Windows平台安装Rabbit MQ

**相关软件下载链接：**  
Erlang和Rabbit MQ版本对应：[链接](https://www.rabbitmq.com/which-erlang.html)  
Erlang下载链接：[链接](https://github.com/erlang/otp)  
Rabbit MQ下载链接：[链接](https://github.com/rabbitmq/rabbitmq-server/releases)

## 1 下载Erlang，并安装
安装完成后进行如下配置：
- (1) 为Erl配置环境变量：ERLANG_HOME=${ERLANG安装目录}
- (2) Path变量新建项：$ERLANG_HOME$\bin


## 2 下载Rabbit MQ，并安装
注意：Erlang和Rabbit MQ有强烈的版本对应关系。
版本不对应，将导致Rabbit MQ启动失败，报错如下错误 Error: {:unable_to_load_rabbit, {‘no such file or directory’, ‘rabbit.app’}}


## 3 启动Web管理插件
进入${RabbitMQ安装根目录}\sbin\目录下，打开CMD，执行以下命令:
```shell
rabbitmq-plugins.bat enable rabbitmq_management
```

##４ 启动Rabbit MQ
```shell
rabbitmq-plugins.bat start
```
> 访问地址：localhost:15672
>
> 默认用户名：guest
>
> 默认密码：guest

## 5.其他常用命令
| 操作 | 命令 |
| - | ---- |
| 查看所有的用户信息 | rabbitmqctl.bat list_users |
| 新增用户 | rabbitmqctl.bat add_user ${username} ${password} |
| 为用户添加所有权限 | rabbitmqctl.bat set_permissions ${username} ".*" ".*" ".*" |
| 修改用户密码 |　rabbitmqctl.bat change_password ${username} ${new_password} |



# Redis常用命令

帮助文档（HELP命令）  
HELP @{group} | 查看某个分组所有命令，如 help @string。   
HELP {command} |  查看某个命令详情，如 help get。

## 0 generic
| 命令 | 说明 |
| ---- | --- |
| SELECT index | Change the selected database for the current connection |
| KEYS pattern | 查找所有符合匹配模式的key |
| TTL key | 获取key的生存时间 |
| DEL key [key ...] | Delete a key |
| RENAME key newkey | Rename a key |
| RENAMENX key newkey | Rename a key, only if the new key does not exist |


## 1 STRING
### 1.1 create
| 命令 | 说明 |
| ---- | --- |
| SET key value [EX seconds] [PX milliseconds] [NX &#124; XX] | Set the string value of a key（增/改） |
| SETNX key value | Set the value of a key, only if the key does not exist |
| SETEX key seconds value | Set the value and expiration of a key |

### 1.2 delete
| 命令 | 说明 |
| ---- | --- |
| DEL key [key ...] | Delete a key |

### 1.3 update
| 命令 | 说明 |
| ---- | --- |
| SETBIT key offset value | Sets or clears the bit at offset in the string value stored at key |
| SETRANGE key offset value | Overwrite part of a string at key starting at the specified offset |

### 1.4 read
| 命令 | 说明 |
| ---- | --- |
| GET key | Get the value of a key |
| GETBIT key offset | Returns the bit value at offset in the string value stored at key |
| GETRANGE key start end | Get a substring of the string stored at a key |
| GETSET key value | Set the string value of a key and return its old value |


## 2 LIST
### 2.1 create
| 命令 | 说明 |
| ---- | --- |
| LPUSH key value [value ...] | Prepend one or multiple values to a list |
| RPUSH key value [value ...] | Prepend one or multiple values to a list |
| LPUSHX key value | Prepend a value to a list, only if the list exists |
| RPUSHX key value | Prepend a value to a list, only if the list exists |
| LINSERT key BEFORE &#124; AFTER pivot value | Insert an element before or after another element in a list |

### 2.2 delete
| 命令 | 说明 |
| ---- | --- |
| LREM key count value | Remove elements from a list |

### 2.3 update
| 命令 | 说明 |
| ---- | --- |
| LSET key index value | Set the value of an element in a list by its index |
| LTRIM key start stop | Trim a list to the specified range（截取保留指定范围元素，其余元素删除） |

### 2.4 read
| 命令 | 说明 |
| ---- | --- |
| LLEN key | Get the length of a list |
| LPOP key | Remove and get the first element in a list |
| RPOP key | Remove and get the first element in a list |
| LINDEX key index | Get an element from a list by its index |
| LRANGE key start stop | Get a range of elements from a list |


## 3 HASH
### 3.1 create
| 命令 | 说明 |
| ---- | --- |
| HSET key field value | Set the string value of a hash field |
| HMSET key field value [field value ...] | Set multiple hash fields to multiple values |
| HSETNX key field value | Set the value of a hash field, only if the field does not exist |

### 3.2 delete
| 命令 | 说明 |
| ---- | --- |
| HDEL key field [field ...] | Delete one or more hash fields |

### 3.3 update
| 命令 | 说明 |
| ---- | --- |
| HINCRBY key field increment | Increment the integer value of a hash field by the given number |
| HINCRBYFLOAT key field increment | Increment the float value of a hash field by the given amount |

### 3.4 read
| 命令 | 说明 |
| ---- | --- |
| HGET key field | Get the value of a hash field |
| HMGET key field [field ...] | Get the values of all the given hash fields |
| HKEYS key | Get all the fields in a hash |
| HVALS key | Get all the values in a hash |
| HGETALL key | Get all the fields and values in a hash |
| HEXISTS key field | Determine if a hash field exists |
| HLEN key | Get the number of fields in a hash |
| HSTRLEN key field | Get the length of the value of a hash field |
| HSCAN key cursor [MATCH pattern] [COUNT count] | Incrementally iterate hash fields and associated values |


## 4 SET
### 4.1 create
| 命令 | 说明 |
| ---- | --- |
| SADD key member [member ...] | Add one or more members to a set |
| SMOVE source destination member | Move a member from one set to another (增&删) |

### 4.2 delete
| 命令 | 说明 |
| ---- | --- |
| SREM key member [member ...] | Remove one or more members from a set |
| SPOP key [count] | Remove and return one or multiple random members from a set |

### 4.3 update
| 命令 | 说明 |
| ---- | --- |
| SDIFF key [key ...] | Subtract multiple sets |

### 4.4 read
| 命令 | 说明 |
| ---- | --- |
| SISMEMBER key member | Determine if a given value is a member of a set |
| SCARD key | Get the number of members in a set |
| SMEMBERS key | Get all the members in a set |
| SRANDMEMBER key [count] | Get one or multiple random members from a set |
| SSCAN key cursor [MATCH pattern] [COUNT count] | Incrementally iterate Set elements |

### 4.5 Intersect交集
| 命令 | 说明 |
| ---- | --- |
| SINTER key [key ...] | Intersect multiple sets |
| SINTERSTORE destination key [key ...] | Intersect multiple sets and store the resulting set in a key |

### 4.6 UNION并集
| 命令 | 说明 |
| ---- | --- |
| SUNION key [key ...] | Add multiple sets |
| SUNIONSTORE destination key [key ...] | Add multiple sets and store the resulting set in a key |

### 4.7 DIFF差集
| 命令 | 说明 |
| ---- | --- |
| SDIFF key [key ...] | Subtract multiple sets |
| SDIFFSTORE destination key [key ...] | Subtract multiple sets and store the resulting set in a key |


## 5 ZSET
### 5.1 create
| 命令 | 说明 |
| ---- | --- |
| ZADD key [NX &#124; XX] [CH] [INCR] score member [score member ...] | Add one or more members to a sorted set, or update its score if it already exists |

### 5.2 delete
| 命令 | 说明 |
| ---- | --- |
| ZREM key member [member ...] | Remove one or more members from a sorted set |
| ZREMRANGEBYRANK key start stop | Remove all members in a sorted set within the given indexes |
| ZREMRANGEBYSCORE key min max | Remove all members in a sorted set within the given scores |
| ZREMRANGEBYLEX key min max | Remove all members in a sorted set between the given lexicographical range |

### 5.3 update
| 命令 | 说明 |
| ---- | --- |
| ZINCRBY key increment member | Increment the score of a member in a sorted set |

### 5.4 read
| 命令 | 说明 |
| ---- | --- |
| ZCARD key | Get the number of members in a sorted set |
| ZSCORE key member | Get the score associated with the given member in a sorted set |
| ZCOUNT key min max | Count the members in a sorted set with scores within the given values |
| ZLEXCOUNT key min max | Count the number of members in a sorted set between a given lexicographical range |
| ZRANK key member | Determine the index of a member in a sorted set |
| ZREVRANK key member | Determine the index of a member in a sorted set, with scores ordered from high to low |
| ZRANGE key start stop [WITHSCORES] | Return a range of members in a sorted set, by index |
| ZREVRANGE key start stop [WITHSCORES] | Return a range of members in a sorted set, by index, with scores ordered from high to low |
| ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count] | Return a range of members in a sorted set, by score |
| ZREVRANGEBYSCORE key max min [WITHSCORES] [LIMIT offset count] | Return a range of members in a sorted set, by score, with scores ordered from high to low |
| ZRANGEBYLEX key min max [LIMIT offset count] | Return a range of members in a sorted set, by lexicographical range |
| ZREVRANGEBYLEX key max min [LIMIT offset count] | Return a range of members in a sorted set, by lexicographical range, ordered from higher to lower strings. |
| ZSCAN key cursor [MATCH pattern] [COUNT count] | Incrementally iterate sorted sets elements and associated scores |

### 5.5 Intersect交集
| 命令 | 说明 |
| ---- | --- |
| ZINTERSTORE destination numkeys key [key ...] [WEIGHTS weight] [AGGREGATE SUM &#124; MIN  &#124;  MAX] | Intersect multiple sorted sets and store the resulting sorted set in a new key |

### 5.6 UNION并集
| 命令 | 说明 |
| ---- | --- |
| ZUNIONSTORE destination numkeys key [key ...] [WEIGHTS weight] [AGGREGATE SUM &#124; MIN &#124; MAX] | Add multiple sorted sets and store the resulting sorted set in a new key |


## 6 TRANSACTIONS
| 命令 | 说明 |
| ---- | --- |
| MULTI | Mark the start of a transaction block |
| EXEC | Execute all commands issued after MULTI |
| DISCARD | Discard all commands issued after MULTI |
| WATCH key [key ...] | Watch the given keys to determine execution of the MULTI/EXEC block |
| UNWATCH | Forget about all watched keys |



# Spring Bean生命周期

## 1 Bean初始化方法执行顺序
构造函数 -> @PostConstruct -> InitializingBean.afterPropertiesSet() -> xml配置(init-method)

## 2 Bean销毁方法执行顺序
@PreDestroy -> DisposableBean.destroy() -> xml配置(destroy-method)



# 日志框架

## 1 桥接包冲突
### 1.1 slf4j-log4j12.jar与log4j-over-slf4j.jar
执行流程如下：
| slf4j桥接到log4j正确执行路径 | slf4j桥接到log4j错误执行路径 | log4j桥接到slf4j正确执行路径 |
| --------------------------- | -------------------------- | -------------------------- |
| 桥接包：slf4j-log4j12.jar  slf4j桥接log4j(实现) | - | 桥接包：log4j-over-slf4j.jar  log4j桥接slf4j(通过重写log4j包中的类和接口) |
| org.slf4j.LoggerFactory(slf4j-api) | - | <font color="red">org.apache.log4j. LogManager(log4j-over-slf4j)</font> |
| org.slf4j.impl.Log4jLoggerFactory(slf4j-log4j12) | - | org.apache.log4j.Log4jLoggerFactory(log4j-over-slf4j) |
| <font color="red">org.apache.log4j.LogManager(log4j-1.2.17)</font> | <font color="red">org.apache.log4j.LogManager(log4j-over-slf4j)</font> | org.apache.log4j.Logger(log4j-over-slf4j) |
| org.apache.log4j.Logger(log4j-1.2.17) | org.apache.log4j.Log4jLoggerFactory(log4j-over-slf4j) | org.apache.log4j.Category(log4j-over-slf4j)(Logger父类) |
| - | org.apache.log4j.Logger(log4j-over-slf4j) | org.slf4j.LoggerFactory |
| - | org.apache.log4j.Category(log4j-over-slf4j)(Logger父类) | - |
| - | org.slf4j.LoggerFactory | - |


## 2 常见日志适配与日志桥接
| 日志门面(接口) | 日志实现 | 日志桥接/适配包 | 说明 |
| ------------- | ------- | -------------- | ---- |
| slf4j | log4j | slf4j-log4j12 | - |
| slf4j | log4j2 | log4j-slf4j-impl | - |
| log4j | slf4j | log4j-over-slf4j | 重写log4j相关接口及实现类 |
| log4j2 | slf4j | log4j-to-slf4j | - |
| jcl | slf4j | jcl-over-slf4j | 重写jcl相关接口及实现类 |

Slf4j日志级别(日志显示量 大->小)：trace, debug, info, warn, error
Log4j日志级别(大->小)：all, trace, debug, info, warn, error, fatal, off
JCL日志级别(大->小)：trace, debug, info, warn, error, fatal
logback日志级别(大->小)：all, trace, debug, info, warn, error, off
