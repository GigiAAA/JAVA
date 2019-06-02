EE部分：Java精华 重在实践

Maven->前端(html css js)->Mybatis->JavaWeb(Servlet JSP)->Spring(SpringCore、SpringMVC、SpringBoot)

#### Maven--项目管理框架

项目管理：   

1.关于类库的管理(Jar包导入)    **jar包为所有编译好的class文件**

2.项目构建(打包发布)

3.项目标准结构--必须要求源代码、测试代码的存放位置，否则不识别

##### 所有源代码放在src/main/java包下面，测试代码放在src/test/java包下面



```
pom.xml--Maven配置文件
src--放置所有源代码与测试代码
   --main 源文件
     -- java
   --test 测试代码
```



```
windows找PID命令
netstat -ano | findstr "6666"
windows强制终止线程
taskkill /F /pid 5736
```



#### 1.Maven项目的创建

a.通过命令行构建

##### maven标准快速构建语法：

```java
mvn -B archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes
-DarchetypeArtfactId=maven-archetype-quickstart -DarchetypeVersion=1.1
//-groupId="公司名称" -artifactId="项目名称" -version="版本号"
-DgroupId=com.bittech.hello -DartifactId=hello-app -Dversion=1.0.0
```

创建方式：cmd->进入某个文件夹->运行上述语句->创建成功->cmd：cd hello-app/(进入该文件夹)->tree(该项目架构)

```
tree:
pom.xml
src->main->java->com->bittech->hello->App.java(main中放置所有的源代码)
src->test->java->com->bittech->hello->AppTest.java(test中放置所有的测试代码)
```

##### 命令行删除项目语句：

```
cd 路径
clear
rm -rf 项目名称/(如hello-app/)
```

##### maven命令行第二种构建方法：

```java
mvn archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes
-DarchetypeArtfactId=maven-archetype-quickstart -DarchetypeVersion=1.1
```

##### 该方法在命令行创建过程中，会卡住让自行输入groupId,artifactId,version(版本号不输入时默认为1.0)

##### b.使用开发工具IDEA(掌握)

创建流程：

```
1.File
2.new project
3.maven 勾选Create from archetype(通过模板构建项目)
4.org.apache.maven.archetypes:maven-archetype-quickstart->next
5.输入GroupId(公司/组织名称)、ArtifactId(项目名称)、Version(版本号)(如：GroupId:com.bittech.hello ArtifactId:hello-app Version:1.0.0)->next
//该页面初始为Bundled(Maven 3)->java自带的Maven工具，但我们需将Maven工具改为我们自己安装的版本
6.Maven home directory:Bundled(Maven 3)-->修改自己安装的Maven
```



#### 2.Maven中三个关键配置

```
groupId:项目所属的公司或组织名称--用于后续包名称定义
artifactId:项目名称
version:项目版本号
```

#### 3.pom.xml中的关键配置(使用maven最多的功能  重要)

```xml
  <dependencies>
    //在该标签内部配置所有项目需要依赖的第三方jar包
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
```

导入第三方jar包的方式为：

##### 在百度中搜索maven，之后点击首条查询结果，搜索jar包名称，选择版本复制<dependency>标签即可。

**在<dependencies>内部导入第三方Jar包，每个Jar包都对应一个<dependency>标签，通过groupId,artifactId,version来指定不同的jar包。**



#### 常用第三方jar包作用：

```
lombok：
      导入lombok包后，创建类后在该类前添加@Data注解可自动生成该类属性的setter、getter方法，但不可创建构造方法，只有默认的无参构造，其他构造方法还需手工创建。
junit:
	  在类中写入单元测试
	  写入方式为：选中类名称后ctrl+shift+t，勾选类中内容即可自动生成测试类，该测试类位于test包下。
	  为什么不是采用主方法写测试代码呢？原因是主方法为客户端，不可能在客户端处写代码进行测试，故需在被测试代码中写入测试代码。
	  Assert.assertEquals(预期值，实际值)//使用断言判断功能是否正确
```

#### 类中属性类型强制使用包装类，原因是避免数据库中的空指针异常。



#### 4.Maven生命周期

4.1 compile--创建target目录

```xml
mvn compile  (*.java->*.class)
```

![m1](C:\Users\14665\Desktop\maven\m1.png)

4.2 clean---清空target目录

```
mvn clean 
```

![m2](C:\Users\14665\Desktop\maven\m2.png)

4.3  test--执行测试用例

```
mvn test
```

![m3](C:\Users\14665\Desktop\maven\m3.png)

4.4 package--将项目打包

```
mvn package---打包生成结果(jar-默认，war)命名按照artifactId-version命名
在执行package将项目打包过程中会默认执行test命令运行所有测试用例，只有当所有测试用例均通过项目才会打包成功
mvn -DskipTests package
打包过程中不再执行测试代码直接将项目打包
执行方式如下图顺序：
```

![m4](C:\Users\14665\Desktop\maven\m4.png)

#### 构建可执行jar:

#### 1.在pom.xml中添加打包插件并指明主类来使得打包生成的jar具备可执行能力。

```xml
<build>
    <!--所有maven生命周期中需要的插件在此配置，插件是一个特殊的maven构件-->
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-jar-plugin</artifactId>
        <version>2.4</version>
        <configuration>
          <archive>
            <manifest>
              <!--依赖的jar包添加到classpath-->
              <addClasspath>true</addClasspath>
              <!--设置可执行jar的主类-->
              <mainClass>com.bittech.showlo.App</mainClass>
              <addExtensions>true</addExtensions>
              <!--指定可执行jar依赖包归档的目录前缀-->
              <classpathPrefix>lib</classpathPrefix>
            </manifest>
            <manifestEntries>
              <Implementation-Title>${project.name}</Implementation-Title>
              <Implementation-Version>${project.version}</Implementation-Version>
              <Implementation-Vendor-Id>${project.groupId}</Implementation-Vendor-Id>
            </manifestEntries>
          </archive>
        </configuration>
      </plugin>
    </plugins>
  </build>
```

2.使用mvn package将项目打包

3.java -jar 打包后的jar包

![m5](C:\Users\14665\Desktop\maven\m5.png)

4.5 install  --安装构建到本地仓库

```xml
mvn install
将项目打包放置在本地仓库，本机中的其他项目可以使用dependency来引入此项目
```

maven寻找jar包顺序：

先从本地仓库查找是否存在，若不存在去中央仓库中寻找，若中央仓库中都没找到报错。

匹配jar包的规则：根据pom文件中制定的dependency中的三个关键配置(groupId->artifactId->version)

![m6](C:\Users\14665\Desktop\maven\m6.png)

4.6 deploy --安装构建到中央仓库

4.7 site--生成网站

```
mvn site
将项目信息、依赖等生成网站
```

```xml
<name>hello-app</name>
<url>http://bittech.java.com</url>
<description>based on maven project</description>
<developers>
    <developer>
        <id>1</id>
        <name>zhangsan</name>
        <email>zhangsan@qq.com</email>
        <roles>
            <role>Master</role>
        </roles>
    </developer>
    <developer>
        <id>2</id>
        <name>lisi</name>
        <email>lisi@gmail.com</email>
        <roles>
            <role>Dev</role>
        </roles>
    </developer>
</developers>
```

**依赖关系的管理是maven最精髓的地方。一个工程可以依赖多个其它工程，通过工程的唯一标识(groupId+artifactId+version)可以明确指明依赖的库及版本，而且能够处理依赖关系的传递**。maven可以指定依赖的作用范围(scope)，包括以下几种：

|    scope     | 编译阶段 | 测试阶段 | 运行阶段 |                备注                 |
| :----------: | :------: | :------: | -------- | :---------------------------------: |
| **compile**  |    √     |    √     | √        |              默认scope              |
|   **test**   |          |    √     |          |      只在测试期依赖，如junit包      |
| **provided** |    √     |    √     |          | 运行期由服务器提供，如servlet-api包 |
|   runtime    |          |    √     | √        |       编译期间不需要直接引用        |
|    system    |    √     |    √     |          |     编译和测试时由本机环境提供      |



maven官方插件网站：http://maven.aoache.org/plugins/index.html

```xml
a.Apache Maven Compiler Plugin编译插件
b.Apache Maven JAR Plugin 打包jar插件
c.Apache Maven WAR Plugin 打包war插件
d.Apache Maven Assembly Plugin 装配发布包插件
e.Apache Maven Site Plugin 生成项目站点插件
```



### 发布构建到仓库服务

#### 1.中央仓库

a.申请中央仓库账号

b.settings中配置仓库服务的认证信息

c.pom.xml设置发布仓库地址

d.向中央仓库申请发布构件

e.申请通过之后，进行构件发布

#### 2.公司私服

a.申请公司私服账号

b.settings中配置仓库服务的认证信息

c.pom.xml设置发布仓库地址

d.发布构件

#### 3.配置操作

pom.xml中的配置信息

```xml
<distributionManagement>
    <repository>
        <id>mycompany-repository</id>
        <name>MyCompany Repository</name>
        <url>scp://repository.mycompany.com/repository/maven2</url>
    </repository>
</distributionManagement>
```

settings.xml中配置信息

```xml
<servers>
    
</servers>
```



maven:

1.重点掌握IDEA创建Maven项目

2.使用pom文件添加依赖

3.会使用插件构件可执行jar包