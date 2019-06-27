## IDEA三步走使用补丁永久激活

#### 第一步：下载补丁

附上百度云链接及提取码：

链接：https://pan.baidu.com/s/1EuzfX1IltHnbyV26HWuRkw 
提取码：p7qx 
下载到本地后解压，压缩包中有一个.jar文件，将该文件复制进行第二步。

#### 第二步：配置

2.1 将.jar文件复制到IDEA路径下的bin包中，如下图：

![1561623865939](E:\git\JAVA\IDEA破解\1.png)

**2.2打开bin包中两个后缀为.vmoptions的文件，在其最后一行加入.jar文件的路径，保存关闭即可。**

![1561623891521](E:\git\JAVA\IDEA破解\2.png)

##### idea.exe.vmoptions添加以下语句：

![1561623964424](E:\git\JAVA\IDEA破解\3.png)

##### idea64.exe.vmoptions添加一样的语句：

![1561624049166](E:\git\JAVA\IDEA破解\4.png)

#### 第三步：打开IDEA，将下列一段内容复制到Activation code中即可，点击OK后可正常使用IDEA。

##### 注意：用户名和邮箱随意修改一个即可。

```java
ThisCrackLicenseId-{
"licenseId":"ThisCrackLicenseId",
"licenseeName":"用户名",
"assigneeName":"",
"assigneeEmail":"邮箱",
"licenseRestriction":"For This Crack, Only Test! Please support genuine!!!",
"checkConcurrentUse":false,
"products":[
{"code":"II","paidUpTo":"2099-12-31"},
{"code":"DM","paidUpTo":"2099-12-31"},
{"code":"AC","paidUpTo":"2099-12-31"},
{"code":"RS0","paidUpTo":"2099-12-31"},
{"code":"WS","paidUpTo":"2099-12-31"},
{"code":"DPN","paidUpTo":"2099-12-31"},
{"code":"RC","paidUpTo":"2099-12-31"},
{"code":"PS","paidUpTo":"2099-12-31"},
{"code":"DC","paidUpTo":"2099-12-31"},
{"code":"RM","paidUpTo":"2099-12-31"},
{"code":"CL","paidUpTo":"2099-12-31"},
{"code":"PC","paidUpTo":"2099-12-31"}
],
"hash":"2911276/0",
"gracePeriodDays":7,
"autoProlongated":false}
```

修改后如以下：

![1561624094933](E:\git\JAVA\IDEA破解\5.png)

破解成功，IDEA正常使用。