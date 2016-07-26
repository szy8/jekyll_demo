### 关于IOC
IOC容器最主要是完成了对象的创建和依赖的注入管理

### spring IOC 容器的几个主要抽象接口
1. Resource
2. BeanDefinition
3. BeanDefinitionReader
4. BeanFactory
5. ApplicationContext

### Resource
是对资源的抽象，每一个接口实现类都代表了一种资源类型，如`ClasspathResource`、`UrlResource`、`FileSystemResource` 等。每一个资源类型都代表了对特定资源的访问策略。它是spring资源访问策略的一个基础实现，应用在很多场景。

<img src="http://yuml.me/diagram/nofunky/class/[Resource]^-.-[ClasspathResource],[Resource]^-.-[UrlResource],[Resource]^-.-[FileSystemResource]" >

### BeanDefinition
用来抽象和描述一个具体 bean 对象。是描述一个 bean 对象的基础数据结构。

### BeanDefinitionReader
将外部资源对象描述的 bean 统一转化为统一的内部数据结构 `BeanDefinition`。对应不同的描述有不同的 Reader。如 `XmlBeanDefinitionReader`用来读取 xml 描述配置的 bean 对象。

<img src="http://yuml.me/diagram/nofunky/class/[BeanDefinitionReader]^-.-[XmlBeanDefinitionReader],[BeanDefinitionReader]^-.-[PropertiesBeanDefinitionReader]" >

### BeanFactory
用来定义一个很纯粹的 bean 容器。它是一个 bean 容器的必备结构。同时和外部应用环境隔离。`BeanDefinition`是它的基本数据结构。他维护一个 `BeanDefinition` Map，并可根据`BeanDefinition`的描述行为进行 bean 的创建和管理。

<img src="http://yuml.me/diagram/nofunky/class/[BeanFactory]^-[AutowireCapableBeanFactory],[AutowireCapableBeanFactory]^-[DefaultListableBeanFactory],[DefaultListableBeanFactory]^-.-[XmlBeanFactory],[DefaultListableBeanFactory]^-.-[XBeanXmlBeanFactory]" >

### ApplicationContext
是应用上下文，或者可以叫做 spring 容器。它的基本实现会持有一个 BeanFactory 对象，并基于此提供一些包装和功能扩转。`ApplicationContext` 读取配置文件，并将配置文件解析成 BeanDefinition，然后注册到 BeanFactory。`ApplicationContext` 和应用环境息息相关，常见实现有`ClasspathXmlApplicationContext`、`XmlFileSystemApplicationContext`、`XmlWebApplicationContext`等。ClassPath、Xml、FileSystem、Web 等词都代表了应用和环境相关的一些意思，从字面上不难理解各自代表的含义。
`ApplicationContext` 和 `BeanFactory` 有一下区别：
1. 资源访问支持：从 Resource 和 ResourceLoader 的基础上可以灵活的访问不同的资源。
2. 支持不同的信息源。
3. 支持应用事件：继承了接口 ApplicationEventPublisher，这样在上下文中为 bean 之间提供了事件机制。

<img src="http://yuml.me/diagram/nofunky/class/[ApplicationContext]^-.-[ClasspathXmlApplicationContext],[ApplicationContext]^-.-[XmlFileSystemApplicationContext],[ApplicationContext]^-.-[XmlWebApplicationContext]" >

### ClasspathXmlApplicationContext

<img src="http://yuml.me/diagram/nofunky/class/[ClasspathXmlApplicationContext]^-.-[ClassPathResource],[ClasspathXmlApplicationContext]^-.-[XmlBeanDefinitionReader],[ClasspathXmlApplicationContext]^-.-[DefaultListableBeanFactory]" >

### 探索bean的初始化过程
#### BeanDefinition 的家族图

<img src="http://yuml.me/diagram/nofunky/class/[BeanDefinition]^-.-[AbstractBeanDefinition],[BeanDefinition]^-.-[AnnotatedBeanDefinition],[AbstractBeanDefinition]^-.-[ChildBeanDefinition],[AbstractBeanDefinition]^-.-[GenericBeanDefinition],[AbstractBeanDefinition]^-.-[RootBeanDefinition],[AnnotatedBeanDefinition]^-.-[AnnotatedGenericBeanDefinition],[AnnotatedBeanDefinition]^-.-[ConfigurationClassBeanDefinition],[AnnotatedBeanDefinition]^-.-[ScannedGenericBeanDefinition],[GenericBeanDefinition]^-.-[AnnotatedGenericBeanDefinition],[GenericBeanDefinition]^-.-[ScannedGenericBeanDefinition],[RootBeanDefinition]^-.-[ConfigurationClassBeanDefinition]">
#### BeanDefinition 的数据结构
	parentName
	beanClassName
	factoryBeanName
	factoryMethodName
	scope
	lazyInit
	dependsOn
	autowireCandidate
	primary
	constructorArgumentValues
	propertyValues
	singleton
	prototype
	abstract
	role
	description
	resoureDescription
	originatingBeanDefinition
	
#### 单例（非懒加载） bean 的创建
1. 在 BeanFactory 注册完所有的 BeanDefinition，容器会实例化所有非懒加载的单例 Bean。
2. 容器会调用BeanFactory中的preInstantiateSingletons方法去实例化所有非懒加载的单例 Bean。
3. preInstantiateSingletons方法会遍历所有的 BeanDefinition，判断是否是非抽象的、单例的、非懒加载的，如果是则调用BeanFactory.getBean方法进行实例化。

#### 有依赖的单例（费懒加载） bean 的创建


#### 非单例 bean 的创建

#### 无依赖bean的初始化过程

#### 有依赖bean的初始化过程（需要注入变量）

#### 单例与非单例