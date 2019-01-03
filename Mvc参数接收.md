*Mvc参数接受*

1.无注解(普通类型)

> 可接受post\get参数
>
> 不能接收json

> 参数可选 ，用interage  不用int     

2.无注解（对象）

> json对象接收不了
>
> 参数可选，对象里面变量可选匹配

无注解一般为表单提交，参数不是必须，没匹配到会给参数默认为null,所以普通类型用包装类

可以接受（类，普通类型）  发送方式 表单提交（类属性+普通类型属性）

3.@requestparme

> 参数默认必选(可设置不选)
>
> form表单

一般在普通类型前面，参数必选，在类上，不报错，不知如何传递(猜测可以用类型转换器，string->类)

@requestparme与不加注解区别

a.注解参数必选

b.注解放在类上，类不可以分为多个属性传递

4@requestbody

>Content-Type 为json
>
>post请求
>
>对象里参数可选，最少匹配一个
>
>若参数一个字符串接收json ,字符串能接收成功，所接收的是全部json包括｛

requestbody,传参数必须在http  body内，可与其他(requestparme、无注解[必须还是以表单提交方式])混用

可单独接受字符串（@requestbody String name）

*总结*：

json传参必须是加@requestbody

无注解，requestparme接收？xx==1&xx=2  ,requestbody接收json

不加注解  都可选

加注解默认必选



