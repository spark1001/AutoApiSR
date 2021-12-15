# AutoApiSR-模仿人为开发版

AutoApi系列：AutoApi、AutoApiSecret、AutoApiSR

# 置顶 #

### 项目说明 ###
* 不定时调用

  在每天**9，13，16**点的时候自动启动（**周六日休息**），持续一到两小时左右，**期间不定时调用api**，模仿人为应用开发

### 特别说明/Thanks ###
* 原教程博主-黑幕（酷安id-Paran）
        

### 区别 ###
   项目用的是公共仓库（开放代码），所有人都能看到你的代码内容。

   所以你的应用id、机密、令牌都会显示出来，不安全。

   加密版，我把应用id、机密都隐藏了，令牌因为需要实时更新，隐藏不了（我不会！），安全性会高很多！
   
   模仿人为应用开发版，顾名思义。
   
--------------------------------------------------------------

### 步骤 ###
教程开始:
1.首先去https://portal.azure.com/#home 注册一个应用,这一步网上的教程实在是太多了,我就不详细写了,大致写一下流程
先用e5管理员账号登录网站,然后在主页找到Azure Active Directory点进去，再在左侧目录找到点击应用注册，再点上方的新注册就会跳出一个新建应用的界面，应用名字随意填写,然后选择任何组织目录(任何 Azure AD 目录 – 多租户)中的帐户，重定向url选web，填入`http://localhost:53682/`,最后点注册即可

2.注册好应用会跳转到应用概述界面,你会看到一个应用程序(客户端) ID,复制这个Id记录下来，后面要用到,然后点击左侧目录的API权限,依次点击`添加权限`、`Microsoft Graph`、`委托的权限`，然后依次搜索以下这12个权限并勾选:

`Files.Read.All`、`Files.ReadWrite.All`、`Sites.Read.All`、`Sites.ReadWrite.All`、
`User.Read.All`、`User.ReadWrite.All`、`Directory.Read.All`、`Directory.ReadWrite.All`、
`Mail.Read`、`Mail.ReadWrite`、`MailboxSettings.Read`、`MailboxSettings.ReadWrite`

全部勾选好后点击底部的添加权限，然后又返回到了API权限界面，这时候你一定要再点一下`代表xxx授予管理员同意`,如果不点这个,outlook api会无法调用

3.点击左侧证书和密码,点+新客户端密码,说明随便填，年限随便选多久都行,现在时间最长只能选24个月，到时候再改一下就行了，然后点添加,添加好后,客户端密码下面会有一个值,复制值下面的那一串代码，这是应用秘钥，后面会用到,到这一步，注册应用已经结束了

4.windows下载rclone获取token,[点击这里下载](https://downloads.rclone.org/v1.57.0/rclone-v1.57.0-windows-amd64.zip"rclone-v1.57.0-windows-amd64.zip") ,随意下载到电脑的任意一个目录,下载后`不要双击rclone.exe安装!`,而是在rclone.exe同目录下,`按住shift后点鼠标右键`，选择在`此处打开cmd窗口或在此处打开power shell窗口`,弹出窗口后,CMD窗口就执行:
`rclone authorize "onedrive" "之前保存的应用id" "之前保存的应用秘钥"`
请自行将双引号内的替换为之前我们保存的应用id和秘钥,示例:
```java
rclone authorize "onedrive" "729xx16f-8x70-4xb8-8fd6-1xxx9b582b1f" "?@P@4u/fxxcxxx28:B-3i_QxxFxc6_ZO"
```
如果是power shell的窗口请执行:
```java
.\rclone authorize "onedrive" "729xx16f-8x70-4xb8-8fd6-1xxx9b582b1f" "?@P@4u/fxxcxxx28:B-3i_QxxFxc6_ZO"
```
执行后电脑浏览器会弹出一个界面,登陆自己的e5账号,然后看到浏览器显示`Success!`，说明获取token成功了。然后返回cmd窗口或者power shell窗口，你会看到一大段`Paste the following into your remote machine --->`开头,`<---End paste`结尾的代码，找到`"refresh_token":"`复制后面的代码直到`","expiry"`，说白了就是复制`refresh_token`，不要带双引号,类似格式如下:

`OAQABAAAAAABeAFzDwllzTYxxxx_qYbH8UALCVjtv_6YeHHOwXExxxxxywOKSg2Hd_GSjW1vcLzqLhDC51Sl4T2ZYfK1p64_ps3qidrodIZLkz-4f-21IfUUgQdEi-g-jIw-La9FjREuUuQnSSKgOlBAKpiwVjwPGdaO_G9yB5cLvX5zi3MZ-_ZwEVHEp-ldDGYqQiZFSnpD6G-cjQIzuN0w8lxl_9laIH0dkA1uUOKtA64qbC976OHSIaidaF4oZi_ntQIsMHWnUssYbR-2X446apxxMupLRM5oaHb8bKMTDlzk6_zUOw23y1jcb8gzyzL5IZdBVVX9UIuPrR-yuzyTd24v39OGk-I9xxhRms5vM6-vUPgxKzuIwFq_CYothdbo8ZvBuMJebl21D1UeaBerjPzxxxxxxxxxVQakxjMBHPC1ueyxR2UvRrlhHhNs8qYFBe5lzceofNWvy1QYRObT8DqCENyLa4Nb08jVTcA6Eh7oxkXtflg_xEY8ECRTWGIZ2qo4ziW70xxxxxxxvq6MCubQgOdt0qdWrc15LVV99YAl9L0KtC3ql0tMPVJBvodTNrvVqcxD-LNtnpxxx1J2tmDuc15xxxxxxTPp5MjXDhSbq8MACmRQh4dR09QqmqXps1c80pxyVkQbr8O669MQ1FMqlICTKJQ8c54_U9NWBo1rAU_zPmE841mDEFjy7dXakFkYR9IIthPNBr2nCQDdvjTitCiIwcT-NTitAd7TseSpiWg9zBsd6Q1OOcL83anZnaJ4sHy68XupeFydmjIYWZw83m96xRaw5MMHJAoyeTkwkHH9qqaSZ0mNM_PN09-tj6nUVYWf5lajMNzd_0GPfwqxxxx9LC0deo43zNTZq20f94_-HNTscKg5dJOA8jUeddxxxxLQa-ZXZV38-lxxxYL_ZDvPu5-0FP-aDTwvxxxx0F7g97o3wTrHSZw14Ra9uxniTh4gAA`
![image](https://spark2077-my.sharepoint.com/personal/spark_spark2077_onmicrosoft_com/_layouts/52/download.aspx?share=EXvYxPqBn2JMgg3DDFjSp_UBfiMj6iOAonRN5niZ_EOIzw
)
5.保存你的应用id、秘钥、refresh_token 3样东西

6.登陆/新建github账号，回到本项目页面，点击右上角fork本项目的代码到你自己的账号，然后你账号会出现一个一模一样的项目，接下来的操作均在你的这个项目下进行。
根据原教程获取应用id、机密、refresh_token（自己复制保存，注意区分id机密，别弄混了）

然后在线编辑你项目里的1.txt，将整个refresh_token覆盖粘贴进去（里面是我的数据，先删掉或者覆盖掉）。（千万不要改1.py）

1.txt内容的应该是开头OAQ….AA结尾，当然也有别的格式 ，结尾不要留空格或者空行
7.依次点击上栏 Setting > Secrets > Add a new secret，新建两个secret如图：CONFIG_ID、CONFIG_KEY。

内容分别如下: ( 把你的应用id改成你的应用id , 你的应用机密改成你的机密，单引号不要动 )

`CONFIG_ID`
`id=r'你的应用id'`

`CONFIG_KEY`
`secret=r'你的应用秘钥'`
![image](https://spark2077-my.sharepoint.com/personal/spark_spark2077_onmicrosoft_com/_layouts/52/download.aspx?share=ER_B5xsugmBJn7nae9e20bsB7Tz3DGB7v3TrlI_E0BfFkQ)

最终的格式应该类似这样
![image](https://spark2077-my.sharepoint.com/personal/spark_spark2077_onmicrosoft_com/_layouts/52/download.aspx?share=EfmyEM353VxPqNOAzxNUydwBjP9wCUqnNKZaCxpwxo11Aw)

8.进入你的个人设置页面(`右上角头像 Settings`，不是仓库里的 Settings)，选择 Developer settings > Personal access tokens > Generate new token,
![image](https://spark2077-my.sharepoint.com/personal/spark_spark2077_onmicrosoft_com/_layouts/52/download.aspx?share=EegowuLBPXZAln_Z8orR37QBKfyDinbzTJt6-m0fuG0mRg)

![image](https://spark2077-my.sharepoint.com/personal/spark_spark2077_onmicrosoft_com/_layouts/52/download.aspx?share=EbqziymKzgxIiD8dttycaJEBpek9-c-ie8g4zgmtgAgeYw)

9.设置名字为GITHUB_TOKEN , 然后勾选 repo , admin:repo_hook , workflow 等选项，最后点击Generate token即可。
repo
![image](https://spark2077-my.sharepoint.com/personal/spark_spark2077_onmicrosoft_com/_layouts/52/download.aspx?share=EcCPXHe8wTJNgblJeYGg9_gBxK4f0XQAn59K-RaCN-Tcfg)
admin:repo_hook
![image](https://spark2077-my.sharepoint.com/personal/spark_spark2077_onmicrosoft_com/_layouts/52/download.aspx?share=EemQ1hH4FQRBoA6gF-4ZjmUBEUlElk_48Qq2Om19koN_Vg)
workflow
![image](https://spark2077-my.sharepoint.com/personal/spark_spark2077_onmicrosoft_com/_layouts/52/download.aspx?share=Ebrst7Bz2XhMoiajeDCZWw4BgmgReoYG_J2kJOa-Xh783w)

10.点击右上角星星/star立马调用一次，再点击上面的Action就能看到每次的运行日志，看看运行状况

（必需点进去Test Api看下，api有没有调用到位，有没有出错。外面的Auto Api打勾只能说明运行是正常的，我们还需要确认10个api调用成功了，就像图里的一样。如果少了几个api，就是注册应用的时候赋予api权限没弄好）
![image](https://spark2077-my.sharepoint.com/personal/spark_spark2077_onmicrosoft_com/_layouts/52/download.aspx?share=ETpxwJuYNXdPjvO7dR2RXFsBiN1vwt3mY2JXL4IyRa2vlA)


11.没出错的话，就搞定啦！！再看看下面的定时次数要不要修改，不打算改就不用管了。

作者设定的每6小时自动运行一次，每次调用3轮（点击右上角星星/star也可以立马调用一次），你们自行斟酌修改（我也不知道保持活跃要调用多少次、多久）：
定时自动启动修改地方：（在.github/workflow/autoapi.yml文件里，自行百度cron定时任务格式）
![image](https://spark2077-my.sharepoint.com/personal/spark_spark2077_onmicrosoft_com/_layouts/52/download.aspx?share=EeC7daKUgb5EvfWEvwWx3DABDbVRYh3VLgb-N-ivmYJ3XQ)

每次轮数修改地方：（在1.py最后面）
![image](https://spark2077-my.sharepoint.com/personal/spark_spark2077_onmicrosoft_com/_layouts/52/download.aspx?share=ETBuwRJzkZJAij_X2OVR-WEBzL0KIq7r5l6P8o-WWkqrxw)

**如果想从AutoApiSecret项目直接升级**

  可以把本项目代码下载，然后把里面部分文件更新进AutoApiSecret:
  * 把 AutoApiSecret 的 1.py 删除，再把本项目的1.py 上传上去
  
  * 把 update.py 上传到 AutoApiSecret
  
  * 把 .github/workflow/AutoupdateToken.yml 上传到 AutoApiSecret 的 .github/workflow/ 文件夹下
  
  * 把 AutoApiSecret 的.github/workflow/autoapi.yml 删除，再把本项目的 .github/workflow/AutoApiSR.yml 上传上去

### 题外话 ###
> Api调用
  你们可以自己去graph浏览器看一下，学着自己修改要调用什么api(最重要的是调用outlook、onedrive)
  https://developer.microsoft.com/zh-CN/graph/graph-explorer/preview

### GithubAction介绍 ###
提供的虚拟环境：

· 2-core CPU
· 7 GB RAM 内存
· 14 GB SSD 硬盘空间

使用限制：
* 每个仓库只能同时支持20个 workflow 并行。
* 每小时可以调用1000次 GitHub API 。
* 每个 job 最多可以执行6个小时。
* 免费版的用户最大支持20个 job 并发执行，macOS 最大只支持5个。
* 私有仓库每月累计使用时间为2000分钟，超过后$ 0.008/分钟，公共仓库则无限制。

（我们这里用的公共仓库，按理，你们可以设定无限循环调用，然后6小时启动一次，保证24小时全天候调用）

### 最后 ###

  
  最后的最后，再次感谢黑幕/paran大佬
  
  ————wangziyingwen/酷安id-卷腿毛菌

