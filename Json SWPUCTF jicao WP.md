## Json SWPUCTF jicao WP

[[SWPUCTF 2021 新生赛\]jicao-CSDN博客](https://blog.csdn.net/qq_49893996/article/details/131629557)



![微信截图_20241129214456](C:\Users\Administrator\Desktop\PICTURES\php\存放wp的文件，图片\微信截图_20241129214456.png)

```php+HTML
 <?php
highlight_file('index.php');
include("flag.php");
$id=$_POST['id'];
$json=json_decode($_GET['json'],true);
if ($id=="wllmNB"&&$json['x']=="wllm")
{echo $flag;}
?>
```

包含了flag.php文件，设定了一个POST请求的id和GET请求的json

语句会对GET请求的数据进行json解码

如果id和json变量的值都等于设定字符串，打印flag

## JSON

JSON对象是一种存储数据的方式，它使用键值对的形式表示数据。

在JSON中，键是一个字符串，值可以是字符串、数字、布尔值、数组、嵌套的JSON对象或null。=

JSON对象的语法规则如下：

> [!IMPORTANT]
>
> 使用花括号 {} 表示一个JSON对象。
>
> 键和值之间使用冒号 : 分隔。
>
> 每个键值对之间使用逗号 , 分隔。
>
> 键必须是一个字符串，需要用双引号或单引号括起来。
>
> 值可以是字符串、数字、布尔值、数组、嵌套的JSON对象或null。

------

下面是一个JSON对象的示例：

```c
{
   "name": "John",
   "age": 30,
   "isStudent": false,
   "interests": ["reading", "traveling"],
   "address": {
      "city": "New York",
      "country": "USA"
   },
   "phoneNumber": null
}
```

在上面的示例中，name、age、isStudent、address、phoneNumber都是键，对应的值分别是字符串、数字、布尔值、嵌套的JSON对象、null。

你可以使用各种编程语言（如JavaScript、Python、PHP等）来创建、访问和操作JSON对象。

所以我们传入

GET json={"x":"wllm"}

POST id= wllmNB

> [!TIP]
>
> POST /?json={"x":"wllm"}
>
> .......
>
> id = wllmNB 
>
> 记得BP要转换请求方式

