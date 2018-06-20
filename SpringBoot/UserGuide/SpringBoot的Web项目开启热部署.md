
> Idea的版本是2018的版本

### 添加Maven的依赖


> 当整个项目添加了<parent> 整体依赖管理的情况，直接添加一下spring-boot-devtools的依赖

```
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.2.RELEASE</version>
    <relativePath/>
</parent>
```

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```

> 也可以自己添加上版本号如：

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
    <version>2.0.2.RELEASE</version>
</dependency>
```

> 添加<build>配置

```
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <fork>true</fork>
            </configuration>
        </plugin>
    </plugins>
</build>

```


### 设置Idea

> 点击 [File] --> [Settings...] --> [Build, Execution, Deployment] --> [Compiler] 找到 [Build project automatically]  这一行，把前面的选项按钮选上。点击右下角 [Apply] 之后点击 [OK]


> 同时按住Shift + Ctrl + Alt + / 组合快捷键，会出来一个页面 点击 [1. Registry...] 找到 Key为[compiler.automake.allow.when.app.runing] 选项 把Value选项打勾 然后点击右下角 [Close]


### 重新启动Idea即可

> 注意有些Idea的版本的快捷键不一样