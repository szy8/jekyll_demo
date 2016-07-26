### spring 自动装配的方式
1. no：默认的方式是不进行自动装配，通过显式设置ref 属性来进行装配。
2. byName：通过参数名 自动装配，Spring容器在配置文件中发现bean的autowire属性被设置成byname，之后容器试图匹配、装配和该bean的属性具有相同名字的bean。
3. byType:：通过参数类型自动装配，Spring容器在配置文件中发现bean的autowire属性被设置成byType，之后容器试图匹配、装配和该bean的属性具有相同类型的bean。如果有多个bean符合条件，则抛出错误。
4. constructor：这个方式类似于byType， 但是要提供给构造器参数，如果没有确定的带参数的构造器参数类型，将会抛出异常。
5. autodetect：首先尝试使用constructor来自动装配，如果无法工作，则使用byType方式。（spring 4.2.6 中已经没有这个选项。）

参考链接：http://www.cnblogs.com/leiOOlei/p/3548290.html

### @Component、@Controller、@Service、@Respository 注解
spring 扫描包时，将带有@Component、@Repository、@Service、@Controller标签的类自动注册到spring容器。拥有这些注解的类在 BeanDefinition Map 中的 key 值为类名（头字母小写）。如果要自定义 key 值，可以使用 @Component('yourName')。

### @Autowired
Spring 通过一个 BeanPostProcessor 对 @Autowired 进行解析，所以要让 @Autowired 起作用必须事先在 Spring 容器中声明AutowiredAnnotationBeanPostProcessor Bean。

@Autowired 可以对类成员变量、方法及构造函数进行标注。
@Autowired 按 byType 自动注入。

required 参数，标明属性是否需要强制注入。

### @Qualifier
@Qualifier 必须配合 @Autowired 一起使用。
@Qualifier 的标注对象是成员变量、方法入参、构造函数入参。
@Autowired 组合 @Qualifier 使用时，按 byName 自动注入。

### @Resource
@Resource 的作用相当于 @Autowired，只不过 @Autowired 按 byType 自动注入，面 @Resource 默认按 byName 自动注入罢了。@Resource 有两个属性是比较重要的，分别是 name 和 type，Spring 将 @Resource 注释的 name 属性解析为 Bean 的名字，而 type 属性则解析为 Bean 的类型。所以如果使用 name 属性，则使用 byName 的自动注入策略，而使用 type 属性时则使用 byType 自动注入策略。如果既不指定 name 也不指定 type 属性，这时将通过反射机制使用 byName 自动注入策略。

### bean 配置的选择，使用 xml，还是使用注解


### 装配方式的选择，使用 xml、@Autowired(@Qualifier) 还是 @Resource

