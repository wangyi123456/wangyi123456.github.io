### 1 java

JAVA_HOME  

CLASSPATH  
.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar

Path
%JAVA_HOME%\bin
%JAVA_HOME%\jre\bin

### 2 idea激活

https://shimo.im/docs/gqQDrjPkwwctk3VJ/read

### 3 Maven

#### 配置环境变量

1. M2_HOME     maven目录下的bin目录
2. MAVEN_HOME  maven的目录
3. %MAVEN_HOME%\bin

#### 修改本地仓库

建立文件夹repositsy

```xml
<localRepository>E://app/apache-maven-3.6.3/repositsy</localRepository>
mysql -u root -p

<mirror>
      <id>alimaven</id>
      <mirrorOf>central</mirrorOf>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
    </mirror>
```

#### idea下指定固定仓库

### 4 MySQL

https://www.runoob.com/mysql/mysql-install.html

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/2019103122095921.png)

https://pan.baidu.com/s/1PVLTL_-AugIWc0xMqVDk3Q  提取码：0ovs

添加环境变量，以管理员身份运行 **切记：8以后不用建立什么狗屁my.ini文件。**

```xml
sc delete mysql
mysqld install
mysqld --initialize-insecure --user=mysql
net start mysql
mysql -u root -p
use mysql
ALTER user 'root'@'localhost' IDENTIFIED BY '123456';
exit
```

### 5 Tomcat

简单，做好java的环境变量配置就好了

### 6 Git



### 7 redis

简单，环境变量,一路下一步

### 8 kafka

server+zookeepr

### 9 elestersch