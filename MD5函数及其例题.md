# MD5函数及其例题

[BJDCTF2020\]Easy MD5 (小宇特详解)_buuctf md5-CSDN博客](https://blog.csdn.net/xhy18634297976/article/details/122747034)

`Hint: select * from 'admin' where password=md5($pass,true)`

这里的关键在于**password=md5($pass,ture)**

## *MD5语法

**标准格式**

md5(string,raw)

| 参数   | 描述                                                         |
| ------ | ------------------------------------------------------------ |
| string | 必需。要计算的字符串。                                       |
| raw    | 可选。规定十六进制或二进制输出格式：<br/>**TRUE**-原始 16字符二进制格式 。<br/>**FALSE**-默认 32字符十六进制数。 |

这里的是原始16字符二进制格式

------

这里说一下两个的联系，这里的16位秘文和32位秘文的第8-24位子字符串时一样的，也就是中间的16位。

这里的原始16字符二进制格式一般会有乱码，如果想解决的话

**1.对输出的16位字节的二进制转换为十六进制。**

**2.取32位秘文的中间16位**

如果MD5值经过hex后，就构成万能密码进行了sql注入，这个就是这个题的关键

在mysql里面，在用作布尔型判断时，以数字开头的字符串会被当做整型数。

要注意的是这种情况是必须要有单引号括起来的，比如password=‘xxx’ or ‘1xxxxxxxxx’，那么就相当于password=‘xxx’ or 1 ，也就相当于password=‘xxx’ or true，所以返回值就是true。

如果只有数字的话就不用单引号

这里有个脚本能够跑出来

```php
<?php 
for ($i = 0;;) { 
 for ($c = 0; $c < 1000000; $c++, $i++)
  if (stripos(md5($i, true), '\'or\'') !== false)
   echo "\nmd5($i) = " . md5($i, true) . "\n";
 echo ".";
}
?>
```

这里用：**ffifdyop**, 这个MD5加密后会返回’or’6XXXXXXXXX(这里的XXXXX是一些乱码和不可见字符)

## ffifdyop

[ffifdyop——绕过中一个奇妙的字符串 - 你知道是我的 - 博客园](https://www.cnblogs.com/tqing/p/11852990.html)

经过md5加密后：276f722736c95d99e921722cf9ed621c

再转换为字符串：'or'6<乱码> 即 <font color=RedOrange> `'or'66�]��!r,��b`</font>

 **用途：**

select * from admin where password=''or'6<乱码>'

就相当于select * from admin where password=''or 1 实现sql注入

不光有 **ffifdyop** 还有 **129581926211651571912466741651878684928** 也可达同样的效果

总之，相当于 select * from admin where password=''or ture



## E.g

[BUUCTF在线评测](https://buuoj.cn/challenges#[BJDCTF2020]Easy MD5)  Easy MD5

`Hint: select * from 'admin' where password=md5($pass,true)`

根据上面分析，输入?password=ffifdyop

获得下一阶段源码

```php+HTML
<?php
$a=$GET['a'];
$b=$_GET['b'];

if($a != $b && md5($a) == md5($b))
{
    .....
}
?>
```

==弱比较则输入**?a=s878926199a&b=s155964671a** 至于为什么请看**.md的利用==弱比较漏洞**

下一阶段源码

```php+HTML
<?php
error_reporting(0);
include "flag.php";

highlight_file(__FILE__);

if($_POST['param1'] !== $_POST['param2'] && md5($_POST['paraml1'] === $_POST['param2'])
   echo $flag;
?>
```

这里用php数组绕过，由于**哈希函数无法处理php数组，在遇到数组时返回false(NULL)**，我们就可以利用false==false成立使条件成立。
<font color="RedOrange">`param1[]=1&param2[]=2`</font>