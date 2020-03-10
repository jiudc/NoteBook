# Maven

## Maven简介

​	Apache Maven是一套软件工程管理和整合工具。基于工程对象模型POM概念，通过一个中央信息管理模块，Maven能够管理项目的构建、报告和文档。是一个项目管理和整合工具，能帮助开发者完成：

- 构建
- 文档生成
- 报告
- 依赖
- SCM
- 发布
- 分发
- 邮件列表

​	工程被定义在一个xml文件中，pom.xml，Project Object Model（POM）。Maven使用约定而不是配置，开发者不再需要自己构建过程。只需要合理放置文件，而不需要再Pom.xml中定义任何配置。

## 配置

### 设置环境变量

1. 使用系统属性设置环境变量
	1. M2_HOME=D:\SoftConfig\apache-maven-3.5.3\
	2. M2=%M2_HOME%\bin
	3. MAVEN_OPTS=-Xms256m -Xmx512m
2.  将%M2%添加到系统“Path”变量末尾

### 使用阿里云仓库
配置setting.xml

```xml
<mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>central</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
	</mirror>
```

### Maven 依赖机制

```xml
<dependencies>
    <dependency>
	<groupId>log4j</groupId>
	<artifactId>log4j</artifactId>
	<version>1.2.14</version>
    </dependency>
</dependencies>
```

​	构建依赖自动下载，若未指定version，则有新版时会自动升级

## 常用功能

### 定制库到Maven本地资源库

​	Kaptcha，流行的第三方JAVA库，被应用来生成验证码图片，用来阻止垃圾文件。其不在Maven中央仓库中。将其安装到Maven本地资源库中。

1. Kaptcha下载：http://www.softpedia.com/get/Programming/Other-Programming-Files/Kaptcha.shtml

2. 执行命令：

	```shell
	mvn install:install-file -Dfile=D:\SoftConfig\kaptcha-2.3-jdk14.jar -DgroupId=com.google.code -DartifactId=kaptcha -Dversion={2.3} -Dpackaging=jar
	```

3. POM中加入依赖关系：

	```xml
	<dependency>
	   <groupId>com.google.code</groupId>
	   <artifactId>kaptcha</artifactId>
	   <version>2.3</version>
	 </dependency>
	```

### 从Maven模板创建项目

```shell
mvn archetype:generate -DgroupId={project-packaging} -DartifactId={project-name}-DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

​	这告诉 Maven 来从 maven-archetype-quickstart 模板创建 Java 项目。如果忽视 archetypeArtifactId 选项，一个巨大的 Maven 模板列表将列出。

​	创建JAR项目：

```shell
mvn archetype:generate -DgroupId=com.ldc -DartifactId=NumberGentor -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

​	创建Web项目（WAR）

```shell
mvn archetype:generate -DgroupId=com.ldc -DartifactId=CounterWebApp -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false
```

### 项目打包

​	在项目路径下，mvn package，最终在target路径下生成jar

### 运行Jar文件

```shell
java -cp NumberGentor-1.0-SNAPSHOT.jar com.ldc
```

### 部署进行生产

​	mvn clean package

### 单元测试

- mvn test
- mvn -Dtest=TestApp2 test

### 项目资源安装到本地资源库

- Mvn install，自动部署到本地资源库
- Mvn clean install，部署最新的资源

### 生成基于Maven的项目文档站点

​	mvn site

### 部署

​	mvn site:deploy

### 查看依赖

​	mvn dependency:tree

### 导出所有依赖的jar包

mvn dependency:copy-dependencies -DoutputDirectory=lib