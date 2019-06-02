### 使用IDEA创建Maven项目图文详细教程

##### 前言：

##### 1.使用IDEA开发工具创建Maven项目之前，必须已下载且配置好Maven(官网即可下载)。且Maven项目创建时需联网。

Maven下载：

![1](C:\Users\14665\Desktop\maven\1.png)



![2](C:\Users\14665\Desktop\maven\2.png)

##### 2.自定义本地仓库

##### 自定义本地仓库方法：

（1）在Maven工具安装目录下创建maven_repository文件夹(Maven的bin文件上一层)。

![9](C:\Users\14665\Desktop\maven\9.png)

(2).**打开Maven工具安装包中conf文件夹下的setting.xml**：如：D:\Program Files (x86)\apache-maven-3.6.1-bin\apache-maven-3.6.1\conf\setting.xml。寻找以下一行代码：

```html
<localRepository/path/to/local/repo</localRepository>
```

##### 该代码默认被注释，我们需将此行代码粘贴复制后将路径改为本地仓库路径。修改完成后保存退出即可

![10](C:\Users\14665\Desktop\maven\10.png)

(3)打开cmd，运行以下语句。

```html
mvn help:system
```

tips：若之前步骤正确，则在cmd中执行该语句需花费较长时间，执行结束后本地仓库文件夹中会出现很多文件。至此自定义仓库创建完毕。

![11](C:\Users\14665\Desktop\maven\11.png)

##### 3.在IDEA中配置Maven

File->Settings->**Build,Execution,Deployment**->Build Tools->Maven->**将Maven home directory选项路径修改为Maven安装路径->将User settings file选项修改为Maven安装包中settings.xml的路径->ok**

![12](C:\Users\14665\Desktop\maven\12.png)







#### 使用IDEA创建Maven项目教程：

1.打开IDEA，File->New->Project->Maven,勾选Create from archetype(通过模板构建项目)，找到名称为：**org.apache.maven.archetypes:maven-archetype-quickstart**的框架选中->next

![3](C:\Users\14665\Desktop\maven\3.png)

2.输入GroupId(公司/组织名称)、ArtifactId(项目名称)、Version(版本号)(如：GroupId:com.bittech.hello ArtifactId:hello-app Version:1.0.0)->next

![4](C:\Users\14665\Desktop\maven\4.png)

##### 3.此步骤需要我们将java自带的maven工具Bundled(Maven 3)修改为我们自己安装的Maven工具(因Bundled为默认配置)。

A.Maven home directory:Bundled(Maven 3)-->修改路径为自己安装的Maven-->ok,**之后我们可以看到Local repository项路径自动变为Maven工具安装路径下的本地仓库。**

![5](C:\Users\14665\Desktop\maven\5.png)

B.接下来勾选User setting file项后的Override，覆写setting file。路径为maven安装路径下conf/settings.xml，接下来ok-->next.

![6](C:\Users\14665\Desktop\maven\6.png)

##### 4.注意此页面会自动过滤掉项目名称中间的横杠，一定一定需我们自行手动添加即可。最后点击Finish即可创建成功。

![7](C:\Users\14665\Desktop\maven\7.png)

5.创建成功后的页面。

![8](C:\Users\14665\Desktop\maven\8.png)

##### 创建仓库注意事项：

若创建完毕后src文件下没有main文件和test文件，则很大可能是setting.xml文件覆写有误，再次检查修改正确即可正常创建Maven项目。



#### 扩展两种使用命令行创建Maven项目的方法：linux环境下只能使用命令行创建

1.命令行执行语句为：

执行下面命令之前需建立一个文件夹，进入该文件夹后再执行以下语句。

```java
mvn -B archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes
-DarchetypeArtfactId=maven-archetype-quickstart -DarchetypeVersion=1.1
//-groupId="公司名称" -artifactId="项目名称" -version="版本号"
-DgroupId=com.bittech.hello -DartifactId=hello-app -Dversion=1.0.0
```

##### 需要注意的是前两行代码均相同，最后一行代码为公司/组织名称的命名，项目名称、版本号的命名需自定义。

2.执行语句为：

```java
mvn archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes
-DarchetypeArtfactId=maven-archetype-quickstart -DarchetypeVersion=1.1
```

第二种方式相比第一种语句去掉了第三行代码及第一行代码中的-B，-B存在的意义是自动创建文件，第二种方式需在执行过程中输入组织名称，项目名称，版本号才可创建成功。

##### 若要使用命令行创建，建议使用第一种自动创建项目。











