# 爆破验证码wp

![微信截图_20241126154731](C:\Users\Administrator\Desktop\PICTURES\php\存放wp的文件，图片\微信截图_20241126154731.png)

用burp拦截，在payload中输入从10000到99999，进行爆破

![微信截图_20241126154740](C:\Users\Administrator\Desktop\PICTURES\php\存放wp的文件，图片\微信截图_20241126154740.png)

按长度排序，获得flag

![微信截图_20241126154746](C:\Users\Administrator\Desktop\PICTURES\php\存放wp的文件，图片\微信截图_20241126154746.png)

> [!WARNING]
>
> 但如果五位数验证码中存在0开头的数字，用bp不太好，因为生成不出00001这种数字，需要运用kali生成字典解决

**crunch 6 6 -t %%%%%% -o six.txt**  生成六位数字验证码字典
