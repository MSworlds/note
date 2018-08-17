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

3.@requestparme

> 参数默认必选(可设置不选)
>
> Content-Type为x-www-form-urlencoded 

4@requestbody

>Content-Type 为json
>
>对象里参数可选，最少匹配一个
>
>若参数一个字符串接收json ,字符串能接收成功，所接收的是全部json包括｛

*总结*：

json传参必须是加@requestbody

@requestbody 接收x-www-form-urlencoded以外类型

不加注解  都可选

加注解默认必选



