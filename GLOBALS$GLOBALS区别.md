# GLOBALS$GLOBALS区别

[global与$GLOBALS[''\]的区别及用法_$globals['argv']-CSDN博客](https://blog.csdn.net/Yeoman92/article/details/52681376)

[PHP中global与$GLOBALS的区别 - 知乎](https://zhuanlan.zhihu.com/p/88480122)

[BugkuCTF-Web-变量1_bugkuctf-变量1-CSDN博客](https://blog.csdn.net/qq_38603541/article/details/85106740)

```php+HTML
flag In the variable ! <?php  
 
 
error_reporting(0);         // 关闭php错误显示
include "flag1.php";        // 引入flag1.php文件代码
highlight_file(__file__);
if(isset($_GET['args'])){   // 通过get方式传递 args变量才能执行if里面的代码
    $args = $_GET['args'];
    if(!preg_match("/^\w+$/",$args)){
    // 这个正则表达式的意思是匹配任意 [A-Za-z0-9_] 的字符，就是任意大小写字母和0到9以及下划线组成
        die("args error!");
    }
    eval("var_dump($$args);");// 这边告诉我们这题是代码审计的题目
}
?>
```

提示说flag在变量里面，经分析只要运行 eval("var_dump($$args);");，flag很有可能就会出来

$$args====>我们可以猜想$args很有可能是一个数组，应该想到的就是超全局变量$GLOBALS

他是用存储全局变量的，全局变量的值在这个超级全局变量里面是一个键值，先当于hashmap的键值对

全局变量可以通过变量名在$GLOBALS找到相对应的值。

eval()这个函数的作用是字符串里面的php代码按正常的php代码被执行

通过构造一个GET参数，直接传GET一个全局变量即可

## Pay Attention !!!

- $GLOBALS['var'] 是外部的全局变量$var本身。
- global $var 是外部$var的同名引用或者指针。

### E.g

```php+HTML
<?php
$var1 = 1;
function test()
{
    unset($GLOBALS['var1']); //释放的是前面定义的全局变量
}
test();
echo $var1;
?>

//输出空 

<?php
$var1 = 1;
function test()
{
    global $var1;
    unset($var1);
}
test();
echo $var1;
?>

//打印 1
```

证明删除的只是别名，$GLOBALS['var']的引用，起本身的值没有受到任何的改变。

也就是说 global $var 其实就是$var = &$GLOBALS['var']。调用外部变量的一个别名而已。