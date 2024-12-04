# PHP弱比较

设num=1n即可解决
![微信截图_20241126172726](C:\Users\Administrator\Desktop\PICTURES\php\存放wp的文件，图片\微信截图_20241126172726.png)

[PHP弱类型比较总结_([0\] & 0x01) > 0 ? true : false;-CSDN博客](https://blog.csdn.net/u014029795/article/details/99709333)

### 0x01 字符串和数字比较

- var_dump('a' == 0);	//bool(true)
- var_dump('1a' == 1);	//bool(true)
- var_dump('12a' == 1);	//bool(false)

会出现上面的结果是因为字符串在和数字比较的时候会将字符串转化为数字，比如a转换失败成False，False又和0弱类型比较是相等的，所以第一个是true。
但是如果字符串是以数字开头的，那么就会转成这个数字再做比较，所以第二个也是true，第三个则是因为转成数字后变成了12，不等于1，则为false。

### 0x02 布尔true和任意比较

- var_dump(True == 0);	//bool(false)
- var_dump(True == 'False');	//bool(true)
- var_dump(True == 2);	//bool(true)

bool 1和任何比较都相等，除了0和false，因为0也认为是bool false，true是不等于false的，所以第一条是false，其余的全是true。

### 0x03 hash值和字符串“0”比较

```php+HTML
$str1 = "a";
echo md5($str1);	//0cc175b9c0f1b6a831c399e269772661
var_dump(md5($str1) == '0');	//bool(false)
---------------------------------------------------------
$str2 = "s224534898e";
echo md5($str2);	//0e420233178946742799316739797882
var_dump(md5($str2) == '0');	//bool(true)
---------------------------------------------------------
$str3 = 'a1b2edaced';
echo md5($str3);	//0e45ea817f33691a3dd1f46af81166c4bool
var_dump(md5($str3) == '0');	//bool(false)
---------------------------------------------------------
var_dump('0e111111111111' == '0');	//bool(true) 
```

其实我觉得这个不应该叫做hash值和字符串0的比较，应该叫做科学计数法和字符串和0的比较，只要是以0e开头，后面为数字的字符串和字符串0比较值都是相等的，因为不管0不论和多少相乘都是0。
所以当hash出来的32个值，开头前两个为0e，后面全部为数字的话，他们就会和字符串0相等的。
第一条只是0开头，所以只能当普通字符串，结果为false。
第二条0e后面全为数字，符合要求，结果为true。
第三条虽然为0e，但是后面不全为数字，所以结果为false。
最后一条就是告诉大家，不是只有hash才能和字符串0相等。