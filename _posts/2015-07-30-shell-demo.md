---
layout: post
title: shell 基本语法
---

### 变量定义
    
    x=5 # 注意不可以有空格
    y=6
    str=‘I want you’
    echo ${x} # 变量调用
    
    ## 变量类型定义
    declare –a name ：表示数组array。
    declare –f name ：表示是function的名字。
    declare –F name ：同上，但只显示function的名字。这个和上面的具体差异不太明白，但是这两者都很少使用，先不理会它们。
    declare –i name ：表示整数
    declare –r name ：表示只读。不能使用unset。对于只读变量，也可以使用readonly name 的方式，相当于declare –r name 。readonly可以带三个选项：-f表示这是个function的名字，-p表示打印所有的readonly的名字，-a表示这是个只读的数组。
    declare –x name ：同export，即不仅在当前的环境中起作用，也在外部的shell环境中起作用。

### 运算
    $((…))是用于进行整型运算的。在$((…))中，我们并不需要对变量加上$来表示它的值，也不需要预先声明这个变量是个整型。在双引号下也能进行有效运算。下面是个例子：
    #declare -i aa=13 
    aa=13 
    echo '$((aa-3))'=$((aa-3)) 
    echo '$(($aa-3))'=$(($aa-3))
    也支持$((x += 2))的格式，包括下面几种操作。在下面的例子中我们引用了上面aa=13。
    ++ ：$((aa++))为13，并将aa赋值为14，注意使用$(($aa++))会报错，无法解析13++的含义，所以为了简洁并且不产生错误，不在运算式中加入$，如果是$((++aa))为14，并将aa赋值为14，这与C语言是一样的。
    -- ：$((aa--))为13，并将aa赋值为12，如果是$((--aa))为12，并将aa赋值为12。
    +
    -
    *
    /
    %：求余：$((aa%5))=3
    **：这个在C语言中是没有的，表示Exponentiation，即取幂。例如上面例子中$(($aa**3))相当于13*13*13=2197
    <<
    >>
    &
    |
    ~
    !
    ^
    还支持逻辑运算，包括<, >, <=, >=,==,!=,&&,||， 这些也与C语言一样。例如$((3>1))为1
    在[ … ]，>,<,=等符号是用于判断字符串的，表示用于比较数字的，在[ … ]中，如果对数字进行比较，
    需要使用-lt, –gt, –le, –ge, –eq, –ne 。使用[ … ]，例如if [ 3 –gt 20 ]; then，条件不成立，但是[ 3 > 20 ]，则成立，因为此刻比较的是字符串
    对于数学运算的赋值，使用$((...))有时显的比较繁复，可以使用let，格式如下：
    let intvar =expression
    let表示expression是个数学运算，无须使用$(())来作进一步表明，这样的赋值方式简洁很多。 等号前后是没有空格的，在expression的表达式中也是没有空格的，如果有空格必须用引号引起来，可以是单引号，也可以是双引号，let x=1+4；let x='1 + 4'；let x="1 + 4"，这三个同样都是给x赋值为5

### 字符串
[http://justcoding.iteye.com/blog/1963463](http://justcoding.iteye.com/blog/1963463)


### 数据结构（只有数组）
    # 数组
    arr=(1 2 3) # 注意是空格分隔
    # 序列，这个可以自己实现
    生成序列：{1..6} {a..z} 或者使用seq工具 $seq 1 3 10    '从1开始，到10 间隔为3 结果是：1 4 7 10
    

### 控制语句

    ### if 
    if condition:thrn
        do thing
    elif condition;then
        do thing
    fi
    #### demo 
    if [ $x == 5 ];then
        echo 'Y'
    else
        echo 'N'
    fi
    ### for
    for var in list
        do
            do thing;
        done
        
    for (( initialisation ; ending condition ; update )) 
        do 
            statements ... 
        done
    ### demo    
    for (( i=1; i <= 9 ; i++ ))
        do
            echo $i
        done    
    ### while 
    while condition
        do
            do thing;
        done
        
    ### util
    until [ $var -eq 6 ];
        do
            let var++
            echo $var
        done
        
### 函数定义
    funName(){
        echo "$1 $2"获取参数值
        do thing
    }
    
    #调用
    funName() param_a param_b
### shell中$0,$?,$!等的特殊用法
    $$
    Shell本身的PID（ProcessID）
    $!
    Shell最后运行的后台Process的PID
    $?
    最后运行的命令的结束代码（返回值）
    $-
    使用Set命令设定的Flag一览
    $*
    所有参数列表。如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。
    $@
    所有参数列表。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。
    $#
    添加到Shell的参数个数
    $0
    Shell本身的文件名
    $1～$n
        



一个优秀的开发人员也是半个运维人员，既要懂得一门常用的开发语言，也要学会一门服务端脚本语言        
    
    
    
    









