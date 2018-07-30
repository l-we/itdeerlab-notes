
### 问题一：

> 问题描述

```
lambda expressions are not supported at language level '5'
```

![2018730132512](http://note.itdeer.cn/2018730132512.png)

> 问题解决方案

```
在Idea的左上角 【File】 --> 【Project Structure...】 --> 【Project Settings】 --> 【Modules】 -->
【对应的项目】 --> 【Source】 --> 【Language level:】{5 - 'enum' keyword,generics,autoboxing etc.} 改成 {8 - Lambdas,type annotations etc.} --> 【Apply】 --> 【OK】
```

![2018730132636](http://note.itdeer.cn/2018730132636.png)