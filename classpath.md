##1.classpath

 1、src不是classpath, WEB-INF/classes,lib才是classpath，WEB-INF/ 是资源目录, 客户端不能直接访问。

2、WEB-INF/classes目录存放src目录java文件编译之后的class文件，xml、properties等资源配置文件，这是一个定位资源的入口。

3、引用classpath路径下的文件，只需在文件名前加classpath:

```
<param-value>classpath:applicationContext-*.xml</param-value> 
<!-- 引用其子目录下的文件,如 -->
<param-value>classpath:context/conf/controller.xml</param-value>123
```

4、lib和classes同属classpath，两者的访问优先级为: lib>classes。

5、classpath 和 classpath* 区别：

```
classpath：只会到你的class路径中查找找文件;
classpath*：不仅包含class路径，还包括jar文件中(class路径)进行查找。
```

###JVM查找类文件的顺序：

在doc下使用set classpath=xxx，

如果没有配置classpath环境变量，JVM只在当前目录下查找要运行的类文件。

如果配置了classpath环境，JVM会先在classpath环境变量值的目录中查找要运行的类文件。

值的结尾处如果加上分号，那么JVM在classpath目录下没有找到要指定的类文件，会在当前目录下在查找一次。

值的结尾出如果没有分号，那么JVM在classpath目录下没有找到要指定的类文件，不会在当前目录下查找，即使当前目 录下有，也不会运行。

**建议：**配置classpath环境变量时，值的结尾处不要加分号，如果需要访问当前目录可以用“.”表示。





###jdk

rt.jar 默认就在 根classloader的加载路径里面,不用放在classpath内





### ide-ecplice执行原理

1.Eclipse下的java工程都有哪些文件夹？

答：new java project时，会默认创建SRC源代码目录，并默认创建一个bin目录作为输出目录，输出目录是指生成的class文件和配置文件地址。

所以Eclipse创建的java工程，默认就两个文件夹，src和bin。

2.当点击Eclipse运行时候java jdk会默认执行编译，并将编译后的java文件，生成class文件放到项目目录下的bin文件夹里，以.class命名结尾。

注：即使某个类有bug错误，不能编译通过。但只要点击了编译运行，就会在bin文件夹下生成这个类的class文件。

3.最重要的目录是bin目录，而非src目录。bin目录是整个项目的输出目录，输出目录，意味着不论是编译后的class文件还是项目用到的propertier文件，最终都会输出到bin目录下。

项目最后的结果是jar文件，jar文件里面也只有class文件夹，并不会有src文件夹，而是将src下的所有包名转换为文件夹保存在bin目录下，而其他Test根目录下的比如自己创建的config文件夹并不会在jar包的bin目录下存在，但是会将所有的非src文件夹下的其他文件夹所有东西都保存到bin目录下。

4.java编译器（jdk）能进行编译项目和组织项目的一切前提是：classpath。java.exe虚拟机有个cp参数，eclipse生成的java工程，也会有一个classpath参数，最终eclipse会将自己的classpath参数传给java.exe的参数cp,用于java虚拟机运行操控。比如，你在项目Test下创建的文件夹config,是不会被读取到的，因为eclipse默认的classpath只包括src目录，bin目录jdk目录，和依赖的jar包目录。这也就是为什么我们引进jar包时，一定要add to build path，包括创建文件夹时，也要add to source。这一切都是为了添加进claspath路径里面。

5.jvm最红会根据classpath下的路径，将全部输出，输出到bin目录下。包括引进的jar包等等。

6.所以classpath，是虚拟机编译项目的基础，是虚拟机编译组织项目的基础。

7.classpath是虚拟机编译组织项目的基础。而项目根目录是创建文件，引进路径的基础。

8.buildpath就是classpath，buildpath就是classpath。是jvm编译组织生成项目的根本。只有添加进buildpath(classpath)，才能被jvm读取到，也就是才能被代码读取到。

8.每个项目都有一个默认的根路径。Eclipse下默认根目录是Test下，直接就是工程目录下。而生成的Jar包，默认根目录是bin下。

9.看一下工程文件夹下的.classpath文件：4部分，src问价，bin文件，jdk路径，jar包路径

<classpath><classpathentry kind="src" path="src"/><classpathentry kind="con" path="org.eclipse.jdt.launching.JRE_CONTAINER/org.eclipse.jdt.internal.debug.ui.launcher.StandardVMType/JavaSE-1.8"/><classpathentry kind="output" path="bin"/></classpath>

 

10.项目代码里面，又是怎样获取项目或者文件或者类的绝对路径的呢？

答：因为有了classpath的存在，所以我们在读取配置文件或者涉及文件路径操作的时候，在代码里只需要写相对 相对路径就可以，相对路径就是参照classpath的路径，也就是参照最终的bin文件夹路径。如果想获取绝对路径，可以通过类的加载器，随时获取所在类的绝对路径，class.getclassload().getResource("");即可

 

11.顺便说一下Eclipse是怎么调用本地jdk的及本地jdk的虚拟机的，是依靠你本地配置的JAVA_HOME环境变量，Eclipse会自动读取这个环境变量地址。进而编译运行项目的。进而也就是把Eclipse自己的classpath传递给jvm的cp参数的



##2. 相对路径、绝对路径

#### [通过虚拟路径或相对路径读取一个XML文件，避免硬编码](http://www.blogjava.net/flysky19/articles/90394.html)

####[Java相对路径/绝对路径总结(转）](https://www.cnblogs.com/jianming-chan/p/3536954.html)

#### new File(".")相对路径

```java
File file=new File("a") //a为相对路径时，当前位置为项目根目录下，即System.getProperty("user.dir")的路径-工作目录

```

> 为什么无论在哪都是项目根目录下？
>
> 经测试当前位置 java.exe运行的目录非当前classpath目录 



测试如下

```java
//此程序为
package com.meng;
import java.io.File;
public class testpath {
    public static void main(String[] args) {
        File f=new File(".");//.为当前位置
        System.out.println(f.getAbsolutePath());
        System.out.println(testpath.class.getClassLoader().getResource(""));

    }
}
所在目录
：C:\Users\mengshuai\Desktop\testFile\src\com\meng
项目所在
：C:\Users\mengshuai\Desktop\testFile
包名：com.meng

```



cmd运行

```shell
//java命令在src下执行
C:\Users\mengshuai\Desktop\testFile>cd src
C:\Users\mengshuai\Desktop\testFile\src>java com.meng.testpath
	//结果
C:\Users\mengshuai\Desktop\testFile\src\.
file:/C:/Users/mengshuai/Desktop/testFile/src/


//java命令在项目根目录下执行
C:\Users\mengshuai\Desktop\testFile\src>cd ../
	//设置classpath
C:\Users\mengshuai\Desktop\testFile>set classpath=./src  
C:\Users\mengshuai\Desktop\testFile>java com.meng.testpath
	//结果
C:\Users\mengshuai\Desktop\testFile\.
file:/C:/Users/mengshuai/Desktop/testFile/src/
```

> 结论：
>
> new File (),尽量使用决绝路径，相对路径会变得，一般运行是在项目根目录
>
> 为System.getProperty("user.dir")所在目录   工作目录

####获取各种路径

```java
FileTest.class.getResource("")
//得到的是当前类FileTest.class文件的URI目录。不包括自己！
//如：file:/D:/java/eclipse32/workspace/jbpmtest3/bin/com/test/
FileTest.class.getResource("/")
//得到的是当前的classpath的绝对URI路径。
//如：file:/D:/java/eclipse32/workspace/jbpmtest3/bin/
Thread.currentThread().getContextClassLoader().getResource("")
//得到的也是当前ClassPath的绝对URI路径。
//如：file:/D:/java/eclipse32/workspace/jbpmtest3/bin/
FileTest.class.getClassLoader().getResource("")
//得到的也是当前ClassPath的绝对URI路径。
//如：file:/D:/java/eclipse32/workspace/jbpmtest3/bin/
ClassLoader.getSystemResource("")
//得到的也是当前ClassPath的绝对URI路径。
//如：file:/D:/java/eclipse32/workspace/jbpmtest3/bin/
```



#### 获取项目路径

```java
//获取项目所在目录，原生servlet
httpRequest.getServletContext().getRealPath("/")
httpRequest.getSession().getServletContext().getRealPath("/")  
//获取项目名
 request.getContextPath();

假定你的web application 名称为news,你在浏览器中输入请求路径：

http://localhost:8080/news/main/list.jsp

则执行下面向行代码后打印出如下结果：

1、 System.out.println(request.getContextPath());

打印结果：/news
2、System.out.println(request.getServletPath());

打印结果：/main/list.jsp
3、 System.out.println(request.getRequestURI());

打印结果：/news/main/list.jsp
4、 System.out.println(request.getRealPath("/"));

打印结果：F:\Tomcat 6.0\webapps\news\test
```

