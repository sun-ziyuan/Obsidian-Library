# 1. Lombok概述

> 官方：[https://projectlombok.org/](https://projectlombok.org/)

    Project Lombok is a java library that automatically plugs into your editor and build tools, spicing up your java.  Never write another getter or equals method again, with one annotation your class has a fully featured builder, Automate your logging variables, and much more.

	白话：Lombok 是一种 Java 实用工具，可用来帮助开发人员消除 Java中的冗长代码，尤其是对于简单的Java对象（POJO），它通过注解实现这一目的。


# 2. Lombok原理

	JSR 269：插件化注解处理API  (Pluggable Annotation Processing API)
	JDK6提供的特性，在javac编译期利用注解搞事情

> **官方：**[https://www.jcp.org/en/jsr/detail?id=269](https://www.jcp.org/en/jsr/detail?id=269)

![[Lombok原理树|187]]

JDK5引入了注解的同时，也提供了两种解析方式：

**运行时解析**

	运行时能够解析的注解，必须将@Retention设置为RUNTIME，这样就可以通过反射拿到该注解。java.lang.reflect反射包中提供了一个接口AnnotatedElement，该接口定义了获取注解信息的几个方法，Class、Constructor、Feld、Method、Package等都实现了该接口，对反射熟悉的朋友应该都会很熟悉这种解析方式。

**编译时解析**

	编译时解析有两种机制，分别是：
	1） Annotation Processing Tool
		apt自JDK5产生，JDK7已标记为过期，不推荐使用，JDK8中已彻底删除，自JDK6开始，可以使用Pluggable Annotation Processing API来替换它，apt被替换主要有2点原因
		1. api都在com.sun.mirror非标准包下
		2. 没有集成到javac中，需要额外运行

	2） Pluggable Annotatioin Processing API
		JSR 269自JDK6加入，作为apt的替代方案，它解决了apt的两个问题，javac在执行的时候会调用实现了该API的程序，这样我们就可以对编译器做一些增强，这是javac执行的过程如下：

 ![[Pasted image 20230531175045.png]]

	Lombok本质上就是一个实现了 “JSR 269 API”的程序。在使用javac的过程中，它产生作用的具体流程如下：
	1. javac对源代码进行分析，产生了一颗抽象语法树（AST）
	2. 运行过程中调用实现了“JSR 269 API”的Lombok程序
	3. 此时Lombok就对第一步骤得到的AST进行处理，找到@Data注解所在类对应的语法树（AST），然后修改该语法树（AST），增加getter和setter方法定义的相应树节点
	4. javac使用修改后的抽象语法树（AST）生成字节码文件，即给class增加新的节点(代码块)

	拜读了Lombok源码，对应注解的实现都在HandleXXX中，比如@Getter注解的实现时HandleGetter.handle()。还有一些其他类库使用这个方式实现，比如 Google Auto、Dagger等等。


# 3. Lombok安装

## 3.1 IDEA插件安装

![[Pasted image 20230529154833.png|475]]

## 3.2 maven依赖

```xml
	<dependency>  
		<groupId>org.projectlombok</groupId>  
		<artifactId>lombok</artifactId>  
		<version>1.18.28</version>  
		<scope>provided</scope>  
	</dependency>
```

# 4. Lombok基本使用

| 注解                       | 功能                                                                                 | 参数 |
|:-------------------------|:-----------------------------------------------------------------------------------|:---|
| @Getter/@Setter          | 修饰类：属性，自动生成get()/set()方法                                                           |  AccessLevel.{public,propecped,package,private,none}  |
| @ToString                | 修饰类：可以动态生成相应的ToString方法                                                            |  exclude，of，callSuper，includeFieldNames  |
| @EqualsAndHashCode       | 修饰类：默认情况下：会使用所有非静态和非瞬态属性来生成equals、hashCode                                         |    |
| @NonNull                 | 修饰方法参数、属性：自动判断参数是否为null，为nulll则抛出NullPointerException                              |    |
| @NoArgsConstructor       | 修饰类：可以动态生成无参构造器方法                                                                  |    |
| @AllArgsConstrutor       | 修饰类：可以动态生成全参构造器方法                                                                  |    |
| @RequiredArgsConstructor | 修饰类：生成private构造方法，使用staticName选项生成指定名称的static方法，与@NonNull联用，指定那些属性是本方法参数           |    |
| @Data                    | 修饰类：集合了@ToString、@EqualsAndHashCode、@Getter/@Setter、@RequiredArgsConstructor的所有特性。 |    |
| @Value                   | 修饰类：和@Data类似，区别在于它会把所有成员变量默认定义为private final修饰，并且不会生成set方法                         |    |
| @Slf4j                   | 修饰类：当项目中使用Slf4j日志架构的时候自动创建log日志实例                                                  |    |
| @Log4j                   | 修饰类：当项目中使用Log4j日志架构的时候自动创建log日志实例，需要创建log4j.properties配置文件，推荐使用@Log4j2             |    |
| @Log4j2                  | 修饰类：当项目中使用Log4j日志架构的时候自动创建日志实例，不需要配置文件                                             |    |
| @Synchronized            | 修饰方法：自动生成同步代码块锁                                                                    |    |
| @Cleanup                 | 修饰流变量：自动调用close()方法                                                                |    |

| 高级注解                       | 功能                                                                                 |
|:-------------------------|:-------------------------------------|
| @Accessors(chain=true)   | 修饰类：链式风格，在调用set方法时，返回当前实例              |
| @Builder                 | 修饰类：构建者模式，不可与@NoArgsConstructor同时使用           |
| @Delegate                | 修饰类：代理模式                                                                  |  


| 属性                       | 作用                                                                                 |
|:-------------------------|:-------------------------------------|
| exclude |  不包含哪些属性，示例：@ToString(exclude = {"username"} ) |
| of        |  包含哪些属性，示例：@ToString(of = {"username"} )  |
| callSuper  | 是否包含超类，示例：@ToString(callSuper=true) |  


```java

123

```

# 5. Lombok的问题

## 5.1 @Data和@Builder导致无参构造丢失

+ 单独使用@Data注解，是会生成无参构造方法；
+ 单独使用@Builder注解，发现生成了全属性的构造方法
@Data和@Builder一起用：我们发现没有了默认的构造方法。如果手动添加无参数构造方法或者用@NoArgsConstructor注解都会报错！
```java
	@Data  
	@Builder  
	class UserDTO1 implements Serializable {  
		private Long id;  
		private String name;  
		private Integer age;  
		private String address;  
		private Date createTime;  
	}
```

	这个其实不是坑，一般来讲，自己写了构造器，无参构造器就不会自动生成，builder模式生成了全参构造器，无参的自然不会生成。  
	自己写的无参构造器不行，是因为builder模式的特殊要求，只能有一个全参的私有构造器，builder的build完全接管创建对象的过程。自己写的公有构造器打破了builder模式。也就是说这样的builder模式存在其他创建对象的入口，是有漏洞的builder模式。这是设计模式范畴的事情。

## 5.2 解决办法
> 构造方法加上 @Tolerate 注解，让Lombok假装它不存在(不感知)
```java
	@Data  
	@Builder  
	public class TestLombok {  
		private Long id;  
		private String name;  
		private Integer age;  
		private String address;  
		private Date createTime;  
	  
		@Tolerate  
		public TestLombok(){  
		  
		}  
	}
```
> 直接加上4个注解

```java
	@Data  
	@Builder  
	@AllArgsConstructor  
	@NoArgsConstructor  
	public class TestLombok {  
		`private Long id;  
		private String name;  
		private Integer age;  
		private String address;  
		private Date createTime;  
	}
```
## 5.3 @Builder注解导致默认值无效
> 使用@Builder注解会发现对象的默认值摸出了
```java
	@Data  
	@Builder  
	@AllArgsConstructor  
	@NoArgsConstructor  
	public class TestLombok {  
	
		private String aa = "zzzz";  
		  
		public static void main(String[] args) {  
			TestLombok build = TestLombok.builder().build();  
			System.out.println(build);  
		}  
	}
```
> 输出：TestLombok(aa=null)
> 解决：在字段上加上 @Builder.Default 注解即可
```java
	@Builder.Default  
	private String aa = "zzzz";  
```
## 5.4 @Builder无效分析原因
> 我们使用注解的方式，底层本质就是反射帮我们生成了一系列的getter、setter，所以我们直接打开编译后的target包下面的.class文件，上面的所有原因一目了然！

> 源文件
```java
	@Data
	@Builder
	@NoArgsConstructor
	@AllArgsConstructor
	public class TestLombok {
	
	    private String aa = "zzzz";
	
	    public static void main(String[] args) {
	        TestLombok build = TestLombok.builder().build();
	        System.out.println(build);
	    }
	}
```
> 字节码文件：
```java
	  
public class TestLombok {  
	private String aa = "zzzz";  
	  
	public static void main(String[] args) {  
		TestLombok build = builder().build();  
		System.out.println(build);  
	}  
	  
	public static TestLombokBuilder builder() {  
		return new TestLombokBuilder();  
	}  
	  
	public String getAa() {  
		return this.aa;  
	}  
	  
	public void setAa(String aa) {  
		this.aa = aa;  
	}  
	  
	public boolean equals(Object o) {  
		if (o == this) {  
			return true;  
		} else if (!(o instanceof TestLombok)) {  
			return false;  
		} else {  
			TestLombok other = (TestLombok)o;  
			if (!other.canEqual(this)) {  
				return false;  
			} else {  
				Object this$aa = this.getAa();  
				Object other$aa = other.getAa();  
				if (this$aa == null) {  
					if (other$aa != null) {  
						return false;  
					}  
				} else if (!this$aa.equals(other$aa)) {  
					return false;  
				}  
				return true;  
			}  
		}  
	}  
	  
	protected boolean canEqual(Object other) {  
		return other instanceof TestLombok;  
	}  
	  
	public int hashCode() {  
		int PRIME = true;  
		int result = 1;  
		Object $aa = this.getAa();  
		result = result * 59 + ($aa == null ? 43 : $aa.hashCode());  
		return result;  
	}  
	  
	public String toString() {  
		return "TestLombok(aa=" + this.getAa() + ")";  
	}  
	  
	public TestLombok(String aa) {  
		this.aa = aa;  
	}  
	  
	public TestLombok() {  
	}  
	  
	public static class TestLombokBuilder {  
		private String aa;  
	  
		TestLombokBuilder() {  
		}  
	  
		public TestLombokBuilder aa(String aa) {  
			this.aa = aa;  
			return this;  
		}  
		  
		public TestLombok build() {  
			return new TestLombok(this.aa);  
		}  
		  
		public String toString() {  
			return "TestLombok.TestLombokBuilder(aa=" + this.aa + ")";  
		}  
	}  
}
```
> 查看源码会发现，@Builder的时候，aa属性并没有默认值，所以为空！！
```java
public TestLombokBuilder aa(String aa) {  
	this.aa = aa;  
	return this;  
}
```



## 5.5 总结

	如果想使用@Builder，最简单的方法就是直接写上这四个注解，有默认值加上 @Builder.Default，正常情况下就可以解决了！！
```java
	@Data  
	@Builder  
	@AllArgsConstructor  
	@NoArgsConstructor  
	public class TestLombok {  
		@Builder.Default  
		private String aa = "zzzz";  
		  
		public static void main(String[] args) {  
			TestLombok build = TestLombok.builder().build();  
			System.out.println(build);  
		}  
	}
```

# 6. Lombok的优缺点

优点：
1. 能通过注解的形式自动生成构造器、getter/setter、equals、hashcode、toString等方法，提高了一定的开发效率
2. 让代码变得简洁，不用过多的去关注相应的方法
3. 属性做修改时，也简化了维护为这些属性所生成的getter/setter方法等
缺点：
1. 不支持多种参数构造器的重载
2. 虽然省去了动手创建getter/setter方法的麻烦，但大大降低了源代码的可读性和完整性，降低了阅读源代码的舒适度

# 7. 总结

	Lombok虽然有很多优点，但Lombok更类似于一种IDE插件，项目也需要依赖相应的jar包。Lombok依赖jar包是因为编译时要用它的注解，在使用时，eclipse或IntelliJ IDEA都需要安装相应的插件，在编译器编译时通过操作AST（抽象语法树）改变字节码生成，变向的就是说它在改变java语法。
	它不像spring的依赖注入或者mybatis的ORM一样是运行时的特性，而是编译时的特性。这里我个人最感觉不爽的地方就是对插件的依赖！因为Lombok只是省去了一些人工生成代码的麻烦，但IDE都有快捷键来协助生成getter/setter等方法，也非常方便。

**知乎上有位大神发表过对Lombok的一些看法：**

	这是一种低级趣味的插件，不建议使用。JAVA发展到今天，各种插件层出不穷，如何甄别各种插件的优劣？能从架构上优化你的设计的，能提高应用程序性能的 ，  
	实现高度封装可扩展的...， 像lombok这种，像这种插件，已经不仅仅是插件了，改变了你如何编写源码，事实上，少去了代码你写上去又如何？   
	如果JAVA家族到处充斥这样的东西，那只不过是一坨披着金属颜色的屎，迟早会被其它的语言取代。

Lombok有它的得天独厚的优点，也有它避之不及的缺点，熟知其优缺点，在实战中灵活运用才是王道。