# Java命令行编译执行



> 项目路径
>
> Test
>
> ​    src
>
> ​        com
>
> ​            company
>
> ​                test
>
> ​                    Test.java
>
> ​                    TestDependency.java
>
> ​    bin
>
> ​    lib
>
> ​        abc.jar
>
> ​        xyz.jar



## 一、编译依赖

```shell
javac -d bin -cp lib\abc.jar:lib\xyz.jar src\com\company\test\TestDependency.java -encoding UTF-8 -verbose
```



## 二、编译好的依赖加入`classpath`，编译`main`源文件

```shell
javac -d bin -cp lib\abc.jar:lib\xyz.jar:bin src\com\company\test\Test.java -encoding UTF-8 -verbose
```



## 三、执行`main`类文件

```shell
java -cp lib\abc.jar:lib\xyz.jar:bin com.company.test.Test
```

