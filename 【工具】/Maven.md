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

## 坐标和依赖

### 依赖范围

​	maven在项目编译、测试和执行的时候使用不同的classpath

- compile：默认，编译、测试和执行均有效

- test：只对测试有效

- provided：已提供依赖范围，对编译和测试classpath有效，在运行时无效

- runtime：运行时依赖范围，对于测试和运行classpath有效，编译无效，典型的时JDBC驱动实现

- system： 系统依赖范围。与provided一直，但system范围的依赖时必须通过systemPath元素显示指定依赖文件的路径，而不是maven仓库。

	```xml
	<scope>system</scope>
	<systemPath>${jave.home}/lib/rt.jar</systemPath>
	```

### 依赖调解

​	通过不同路径引入不同版本的依赖

- 路径最近者优先
- 第一声明者优先

### 排除依赖

​	传递性依赖会给项目隐是地引入很多依赖，需要调换或者排除该依赖。exclusions的时候只需要groupId和artifactId，因为版本是唯一的。

### 归类依赖

```xml
<properties>
    <org.springframwork.version>5.2.4.RELEASE</org.springframwork.version>
    <java.version>1.8</java.version>
</properties>
```

### 优化依赖

- mvn dependency:list
- mvn dependency:tree
- mvn dependency:analyze

## 仓库

### 中央仓库

id为central

### 远程仓库

- layout:default表示仓库的布局是Maven2及Maven3的默认布局，而不是Maven1布局
- releases和snapshots：
	- updataPolicy：从远程仓库检查更新的频率。
		- never
		- always
		- interval：X：每隔X分钟
	- checksumPolicy：验证校验和文件

```xml
<repositories>
    <repository>
        <id></id>
        <name></name>
        <url></url>
        <releases>
            <enabled>true</enabled>
        </releases>
        <snapshots>
            <enabled>true</enabled>
            <updatePolicy>daily</updatePolicy>
            <checksumPolicy>ignore</checksumPolicy>
        </snapshots>
    </repository>
</repositories>
```

### 远程仓库认证

```xml
<settings>
	<servers>
        <id></id>
        <username></username>
        <password></password>
	</servers>
</settings>
```

### 部署至远程仓库

- reposity:发布版本构建的仓库
- snapshotRepository:发布快照版本的仓库
- mvn clean deploy

```xml
<project>
    <distributionManagement>
        <reposity>
            <id></id>
            <name></name>
            <url></url>
        </reposity>
        <snapshotReposoty>
            <id></id>
            <name></name>
            <url></url>
        </snapshotReposoty>
    </distributionManagement>
</project>
```

### 快照版本

mvn clean install-U:让mvn前置检查更新

### 从仓库解析依赖的机制

- 当依赖范围是system，Maven直接从本地文件系统解析构件
- 根据依赖坐标计算仓库路径后，尝试从本地寻找构件
- 本地构件不存在，如果依赖的版本是显示的发布版本的构件，则遍历所有的远程仓库
- 若依赖的版本是RELEASE或则LATEST，则基于所有仓库的元数据groupId/artifactId/maven-metadata.xml，将其与本地仓库的对应元数据合并后，计算值
- 如果依赖的版本是SNATSHOT，同上，得到最新快照版本的值

###　镜像

mirrorOf:

- *：匹配所有远程仓库
- external:*，匹配所有缩成仓库
- repo1,repo2，匹配两个仓库
- *，!repo1，使用!排除

```xml
<settings>
    <mirrors>
        <mirror>
            <id></id>
            <name></name>
            <url></url>
            <mirrorOf>central</mirrorOf>
        </mirror>
    </mirrors>
</settings>
```

## 生命周期

​	三套生命周期

### clean

​	清理项目

- pre-clean
- clean
- post-clean

### default

​	真正构建时所需执行的所有步骤

- validate
- initialize
- generate-sources
- proces-sources
- generate-resouces
- process-resources:处理项目主资源文件，一般来说对src/main/resources目录的内容进行替换，复制到项目输出的主classpath目录
- compile：编译项目主源码，编译src/main/java目录下的Java文件至项目的输出的主classpath目录
- process-classes
- generate-test-sources
- process-test-sources
- generate-test-resources
- process-test-resources
- test-compile
- process-test-classes
- test
- prepare-package
- package：接受编译好的代码，打包成可发布的格式，如JAR
- pre-integration-test
- integration
- post-integration-test
- verif
- y
- install
- deploy

### site

- pre-site：执行生成项目站点之前需要完成的工作
- site：生成项目站点文档
- post-site
- site-depoly

### 命令行与生命周期

- mvn clean：调用clean周期的clean，pre-clean和clean
- mvn test：调用defautl生命周期的test
- mvn clean install
- mvn clean deploy site-deploy

## 插件

### 插件目标

- maven-dependency-plugin：
	- dependency:analyze
	- dependency:tree
	- dependency:list

### 插件绑定

- 内置绑定

	- clean：maven-clean-plugin:clean
	- site：maven-site-plugin:site
	- site-deploy：maven-site-plugin:deploy
	- process-resouces:maven-resources-plugin:resources
	- process-test-resources:maven-resources-plugin:testResources
	- compile:maven-compiler-plugin:compile
	- test-compile:maven-compiler-plugin:testCompile
	- test:maven-surcfire-plugin:test
	- package:maven-jar-plugin:jar
	- install:maven-install-plugin:install
	- deploy:maven-deploy-plugin:deploy  

- 自定义绑定

	- ```xml
		<build>
		    <plugins>
		        <plugin>
		            <groupId>org.apache.mavenk.plugins</groupId>
		            <artifactId>maven-source-plugin</artifactId>
		            <version>2.1.1</version>
		            <executions>
		                <id>attach-sources</id>
		                <phase>verify</phase>
		                <goal>jar-no-fork</goal>
		            </executions>
		        </plugin>
		    </plugins>
		</build>
		```

		- 配置id为attach-sources的任务

		- 通过phrase绑定verify生命周期阶段

		- 通过goals配置指定执行的插件目标

		- 删除phrase仍可正确执行，因为插件的目标在编写的时候已经定义了默认绑定阶段

			```shell
			mvn help:describe-Dplugin = org.apache.maven.plugins:maven-source-plugin:2.1.1-Ddetail
			```

### 插件配置

- 命令行插件配置：mvn install -Dmaven.test.skip = true

- POM中插件全局配置：

	```xml
	<build>
	    <plugins>
	        <plugin>
	            <groupId>org.apache.maven.plugins</groupId>
	            <artifactId>maven-compiler-plugin</artifactId>
	            <version>3.8.1</version>
	            <configuration>
	                <source>${java.version}</source>
	                <target>${java.version}</target>
	                <encoding>utf-8</encoding>
	            </configuration>
	        </plugin>
	    </plugins>
	</build>
	```

- POM中插件任务配置

	```xml
	<executions>
	    <execution>
	        <id></id>
	        <phase></phase>
	        <goals>
	            <goal></goal>
	        </goals>
	        <configuration>
	            <tasks>
	                <echo>I'm bound!</echo>
	            </tasks>
	        </configuration>
	    </execution>
	</executions>
	```

### 获取插件信息

可使用目标前缀-Dgoal=-compile

mvn help:describe-Dplugin=compiler -Ddtail

### 从命令行调用插件

- mvn -h
- help是maven-help-plugin的目标前缀
- dependency是maven-dependency-plugin的前缀：mvn dependency:tree

## 聚合

​	将两个模块用一条命令构建。打包方式packaging其值为POM。可在一个打包方式为pom的Maven项目中声明任意数量的module来实现模块的聚合。

```xml
<package>pom</package>
<name>Aggregator</name>
<modules>
    <module>module1</module>
    <module>module2</module>
</modules>
```

​	其中module为子模块相对聚合模块的相对路径

 ## 继承

​	元素relativePath表示父POM的相对路径。默认为../pom.xml

### 可继承的元素

- groupId
- version
- description
- organization
- inceptionYear
- url
- developers
- contributors
- distributionManagement
- issueManagement
- ciManagementscm：项目的持续集成系统信息
- scm：项目的版本控制信息
- mailingLists
- properties
- dependencies
- dependencyManagement
- repositories
- build
- reporting

### 依赖管理

​	在dependencyManagement元素下的依赖声明不会引入实际的依赖，它可以约束dependencies下的依赖使用。在子模块中需要配置groupId和artifactId

import依赖：指向打包类型为pom的模块

```xml
<dependencyManagement>
    <dependiencies>
        <dependency>
            <groupId></groupId>
            <artifactid></artifactid>
            <version></version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependiencies>
</dependencyManagement>
```

### 插件管理

​	pluginManagement元素中的配置不会造成实际的插件调用行为，当POM中配置真正的plugin元素，并且groupId和artifactId和pluginManagement相同才生效。

### 约定由于配置

​	使用约定可以大量减少配置。需要遵守如下约定

- 源码目录为src/main/java

	- 自定义源码

		```xml
		<build>
		    <sourceDirectory></sourceDirectory>
		</build>
		```

- 编译输出的目录为target/classes

- 打包方式为jar

- 包输出目录为target/

超级POM，任何一个Maven项目都隐式地继承自该POM，在maven 3，超级POM在文件$