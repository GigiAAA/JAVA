### intellij IDEA激活码错误时出现“this license K03CHKJCFT has been cancelled”的解决方法

解决方案：

#### 第一步：修改hosts文件

该文件Windows下路径为：**C:\Windows\System32\drivers\etc**

![1](C:\Users\14665\source\JAVA学习\intellij IDEA激活码错误时解决方法\1.png)

**修改hosts文件方法：**

因hosts所在路径非管理员不可修改，故我们可将hosts文件复制到桌面，用记事本打开，将“0**.0.0.0 account.jetbrains.com**”加至最后一行。最后将桌面hosts文件拖回原路径覆盖原hosts文件即可。

![2](C:\Users\14665\source\JAVA学习\intellij IDEA激活码错误时解决方法\2.png)

#### 第二步：打开IDEA，将激活码填入Activation code处即可激活成功。

最新激活码可在下方网址中获得

#### http://idea.lanyus.com/

![3](C:\Users\14665\source\JAVA学习\intellij IDEA激活码错误时解决方法\3.png)

#### 最后再次启动IDEA即可正常使用。

#### IDEA中创建新项目流程：

第一步：打开idea软件，可点击软件提示create new project或是File->New->Project.出现以下界面，选择Static Web,然后点击Next。

![4](C:\Users\14665\source\JAVA学习\intellij IDEA激活码错误时解决方法\4.png)

第二步：出现以下页面，可自行命名文件名称，之后点击Finish。

![5](C:\Users\14665\source\JAVA学习\intellij IDEA激活码错误时解决方法\5.png)

此时文件已经创建完毕，出现以下页面。

![6](C:\Users\14665\source\JAVA学习\intellij IDEA激活码错误时解决方法\6.png)

#### 创建Java文件流程

第一步：文件名称->src->New->Package

![7](C:\Users\14665\source\JAVA学习\intellij IDEA激活码错误时解决方法\7.png)

给包命名

![8](C:\Users\14665\source\JAVA学习\intellij IDEA激活码错误时解决方法\8.png)

第二步：包->New->Java class,然后给类命名即可。

![9](C:\Users\14665\source\JAVA学习\intellij IDEA激活码错误时解决方法\9.png)