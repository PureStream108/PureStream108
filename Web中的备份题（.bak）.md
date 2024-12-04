# Web中的备份题（.bak）+ 一些字符串函数

看到备份题，可以试试在后面加上.bak，比如index.php.bak

- **常见的备份文件后缀名**：`.git .svn .swp .~ .bak .bash_history .rar .zip .7z .txt .html .tar.gz .old .temp`

[BUGKU-WEB 备份是个好习惯-CSDN博客](https://blog.csdn.net/qq_36292543/article/details/136249593)

```php+HTML
<?php
/**
 * Created by PhpStorm.
 * User: Norse
 * Date: 2017/8/6
 * Time: 20:22
*/

include_once "flag.php";
ini_set("display_errors", 0); // 将PHP的错误显示设置为0，即不显示错误信息
$str = strstr($_SERVER['REQUEST_URI'], '?'); //$str为从？开始后面的url
$str = substr($str,1); // 从第一位开始，即去掉第零位的？
$str = str_replace('key','',$str); //在$str中查找key这个字符串，如果用则用“ ”替换
parse_str($str); //解析为变量 由下文可知，应该解析成了$key1和$key2两个变量
echo md5($key1);

echo md5($key2);
if(md5($key1) == md5($key2) && $key1 !== $key2){
    echo $flag."取得flag";  //如果key1和key2值相同但类型不同，就得到flag
}
?>

```

### 1.strstr()

：strstr(str1,str2)函数用于判断字符串str2是否是str1的子串。如果是，则该函数返回str2在str1中首次出现的地址；否则，返回NULL。

```php
$email   =  '1235698556@qq.com' ;
$domain  =  strstr ( $email ,  '@' );
echo  $domain ;  // 打印 @qq.com
 
$user  =  strstr ( $email ,  '@' ,  true );  // 从 PHP 5.3.0 起 true = 1
echo  $user ;  // 打印 1235698556
```

### 2.strpos()

**strpos — 查找字符串在另一个字符串中首次出现的位置**

mixed strops(string $haystack,$mixed $needle,[int $offset=0])
返回needle在haystack中首次出现的数字位置，从 **0** 开始查找，区分大小写。

参数：

haystack，在该字符串中进行查找。

needle，如果needle不是一个字符串，那么它将被转化为整型并被视为字符的顺序值。

offset，如果提供了此参数，搜索会从字符串该字符数的起始位置进行统计。和strrpos()、strripos()不一样，这个偏移量不能是负数。

```php
<? php
 
    echo strpos("You love php, I love php too!","php");
 				 ↑第0位   ↑第9位
    结果：9
 
?>
```

------

**stripos()**函数，与strpos()函数类似，不过其  **不区分大小写**。

**strripos()** - 查找字符串在另一字符串中最后一次出现的位置（**不区分**大小写）

**strrpos()** - 查找字符串在另一字符串中最后一次出现的位置（**区分**大小写）

```php
<?php
 
    echo strrpos("You love php, I love php too!","php");
 
    结果 21
?>
```

没有[找到要用 ===false 做判断](http://www.php.cn/php-weizijiaocheng-339119.html)

> [!CAUTION]
>
> 如果这个字符串中没有找到相应的子字符串 就返回false 
>
> 如果这个子字符串位于字符串的开始处 就会返回0 
>
> 为了区分 0 和 false 就必须使用等同操作符 === 或者 ！== 



### 3、substr

substr() 函数返回字符串的一部分

substr(string,start,length)

参数：

1，string 即你要截取的字符串

2，start 即要截取的开始位置（**0表示从从前往后数 第一个字符开始，负数表示从从后往前数**）

　　eg:start=1,表示从从前往后开始的第二个数开始截取，start=-1,表示从从后往前开始的第一（是第一不是第二哦）个数开始截取，

3，length 当为正数时，为需要截取的长度；当为负数时，即理解为去掉末尾的几个字符

　　eg:length=3,表示截取三个长度；length=-2，即为去掉末尾的两个字符


```php+HTML
<?php
 
$rest = substr("abcdef", 0, -1);  // 返回 "abcde"
 
$rest = substr("abcdef", 2, -1);  // 返回 "cde"
 
$rest = substr("abcdef", 4, -4);  // 返回 ""
 
$rest = substr("abcdef", -3, -1); // 返回 "de"
 
 
// 访问字符串中的单个字符
 
// 也可以使用中括号
 
  $string = 'abcdef';
 
  echo $string[0];                 // a
 
  echo $string[3];                 // d
 
  echo $string[strlen($string)-1]; // f
 
 
//中文字符串的截取和获取长度 mb_substr()
 
$str = '我abc是谁';  //utf-8编码的字符串
 
echo mb_substr($str, 0, 2, 'utf-8'); //输出 我a
 
$str = '我是谁';  //gbk编码的字符串
 
echo mb_substr($str, 0, 1, 'gbk'); //输出 我
 
substr_replace($val['geval_frommembername'], '******', 2, 10);
 
 
?>
```

### 4.str_replace

在PHP中，字符串函数 str_replace() 用来替换字符串中的一些字符（区分大小写）。

**函数语法：**

str_replace ( mixed $search , mixed $replace , mixed $subject [, int &$count ] ) : mixed

**函数参数说明：**

| 参数    | 描述                                 |
| ------- | ------------------------------------ |
| search  | 必需。规定要查找的值。               |
| replace | 必需。规定替换 *search* 中的值的值。 |
| subject | 必需。规定被搜索的字符串。           |
| count   | 可选。一个变量，对替换数进行计数。   |

str_replace() 用来替换字符串中的一些字符，该函数返回一个字符串或者数组。返回的字符串或数组是将 subject 中全部匹配 search 的值都被 replace 替换之后的结果。该转换不会改变大小写。

 **替换的规则如下：**

    1. 如果 search 和 replace 为数组，那么 str_replace() 将对 subject 做二者的映射替换。
    
    2. 如果 replace 的值的个数少于 search 的个数，多余的替换将使用空字符串来进行。
    
    3. 如果 search 是一个数组而 replace 是一个字符串，那么 search 中每个元素的替换将始终使用这个字符串。
    
    4. 如果 search 和 replace 都是数组，它们的值将会被依次处理。
**E.g 1   替换字符串中的子串(search 和 replace 都为单个字符串):**

```php+HTML
<?php
// 替换字符串中的子串(search 和 replace 都为单个字符串)
$res = str_replace('l', 'i', 'hello world', $count);

// 输出
echo $res;

echo '<br>';

// 替换的次数
echo $count;
    以上代码输出如下（字符串中的 l 被替换为 i）：

heiio worid
3
```

**E.g 2，替换字符串中的子串(search 为数组，replace 为单个字符串)：**

```php+HTML
<?php
// 替换字符串中的子串(search 为数组，replace 为单个字符串)
$res = str_replace(['l', 'o'], 'i', 'hello world', $count);

// 输出
echo $res;
echo '<br>';

// 替换的次数
echo $count;
    以上代码输出如下（字符串中的 l 和 o 被替换为 i）：

heiii wirid
5
```

**E.g 3，替换字符串中的子串(search 和 replace 都为数组)：**  

```php+HTML
<?php
// 替换字符串中的子串(search 和 replace 都为数组)
$res = str_replace(['l', 'o'], ['o', 'b'], 'hello world', $count);

// 输出
echo $res;
echo '<br>';

// 替换的次数
echo $count;
    以上代码输出如下（字符串中匹配的子串被依次处理）：

hebbb wbrbd
8
```

### 5.parse_str()

在PHP中，字符串函数 parse_str()  将字符串解析成多个变量。

**函数语法：**

parse_str ( string $encoded_string [, array &$result ] ) : void

**函数参数说明：**

| 参数           | 描述                                                       |
| -------------- | ---------------------------------------------------------- |
| encoded_string | 必需。规定要解析的字符串。                                 |
| result         | 可选。规定存储变量的数组名称。该参数指示变量存储到数组中。 |

parse_str() 函数将字符串解析成多个变量。

如果 encoded_string 是 URL 传递入的查询字符串（query string），如果未设置 result 参数，则该函数设置的变量将覆盖当前作用域中已存在的同名变量；如果提供了 result 则会设置到该数组里。       

**注意：该函数没有返回值**



**E.g 1 将字符串解析成当前作用域中的多个变量 :**

```php+HTML
<?php
// 将字符串解析成当前作用域中的多个变量
$id = 0;
$name = '';
$favorite = [];

// 解析字符串
$str = 'id=1&name=zhangsan&favorite[]=rice&favorite[]=noodle';
$res = parse_str($str);

// 输出
print($id);
echo '<br>';
print($name);
echo '<br>';
print_r($favorite);
    以上代码输出如下：

1
zhangsan
Array ( [0] => rice [1] => noodle )
```

**E.g 2 将字符串解析存储到指定数组：**

```php+HTML
<?php
// 将字符串解析存储到指定数组
$str = 'id=1&name=zhangsan&favorite[]=rice&favorite[]=noodle';
$params = [];
parse_str($str, $params);

// 输出
print_r($params);
    以上代码输出如下：

Array ( [id] => 1 [name] => zhangsan [favorite] => Array ( [0] => rice [1] => noodle ) )
```

### 6.strcmp

比较两个字符串（区分大小写）

```php
<?php
echo strcmp("Hello world!","Hello world!");
?>
```

**定义和用法**

**strcmp()** 函数比较两个字符串。

**注释：**strcmp() 函数是二进制安全的，且对大小写敏感。

**提示：**该函数与 strncmp()函数类似，不同的是，通过 strncmp() 您可以指定每个字符串用于比较的字符数。

**语法**

```php
strcmp(string1,string2)
```

| 参数      | 描述                             |
| :-------- | :------------------------------- |
| *string1* | 必需。规定要比较的第一个字符串。 |
| *string2* | 必需。规定要比较的第二个字符串。 |

**技术细节**

> [!TIP]
>
> 本函数返回：0 - 如果两个字符串相等
>
> <0 - 如果 *string1* 小于 *string2*
>
> 0 - 如果 *string1* 大于 *string2*

