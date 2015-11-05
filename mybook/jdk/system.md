## System

说下Java里面常用的两个常用对象`System`、`Object`。

System对象，望文生义来讲，它应该和**系统**有关，这个系统不仅仅是jvm运行所依赖的操作系统，还代表着jvm本身这个系统，在这个对象中有几个很实用的属性个方法。比如获取操作系统的环境，这点类似于python的os模块。还可以获取启动时加入的外部jvm参数

### System.getProperty

访问外部JVM参数

当我们在启动Java虚拟机时，我们可以指定一些参数，比如：

    java -Dvar=123
    
    System.getProperty("var")  // 返回 123 

### System.getEnv

获取jvm所依赖运行的操作系统环境，例如在linux系统下，就好比执行env命令，获取操作系统环境参数，比如在苹果os系统下：返回下面这些信息

    {SHELL=/bin/bash, 
    TMPDIR=/var/folders/cz/5_96cf9n7kv7fq81ynx3r2sc0000gn/T/, 
    JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.7.0_65.jdk/Contents/Home,
    __CF_USER_TEXT_ENCODING=0x1F5:0x19:0x34, 
    PATH=.:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Library/Java/JavaVirtualMachines/jdk1.7.0_65.jdk/Contents/Home/bin, VERSIONER_PYTHON_VERSION=2.7, CLASS_PATH=/Library/Java/JavaVirtualMachines/jdk1.7.0_65.jdk/Contents/Home/lib, XPC_FLAGS=0x0, 
    JAVA_MAIN_CLASS_95606=com.intellij.rt.execution.application.AppMain, 
    USER=wangjun, 
    HOME=/Users/wangjun, 
    LOGNAME=wangjun, 
    XPC_SERVICE_NAME=com.jetbrains.intellij.50272, 
    Apple_PubSub_Socket_Render=/private/tmp/com.apple.launchd.yGKeQcY8EN/Render, 
    SSH_AUTH_SOCK=/private/tmp/com.apple.launchd.3Bxsg8uF1T/Listeners, 
    VERSIONER_PYTHON_PREFER_32_BIT=no}


### 输入流、标准输出流、错误输出流

    System.in
    System.out
    System.err

### Console

    Console console = System.console();

Console是操作系统提供命令交互的窗口，比如Windows的cmd 一个JVM是否有可用的Console依赖于底层平台和JVM如何被调用. 
如果JVM是在交互式命令行(比如Windows的cmd)中启动的,并且输入输出没有重定向到另外的地方,那么就可以得到一个可用的Console实例。

### 获取操作系统时间

    System.currentTimeMillis();
    System.nanoTime();


### 其它

    System.gc
    System.copyArray

    
        
    


