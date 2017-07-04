# Maven入门

## maven下载配置
1. maven官网下载配置 http://mirrors.hust.edu.cn/apache/maven/maven-3/3.5.0/binaries/apache-maven-3.5.0-bin.zip
2. 解压文件到指定目录
3. 新建系统变量 MAVEN_HOME  值: MAVEN的指定目录
4. 在系统path后面添加 ;%MAVEN_HOME%\bin;
5. 测试安装成功是否: mvn -v


## maven安装目录解析
1. bin目录: 命令所在的目录 mvn mvnDebug
2. boot 类加载器
3. lib 运行所需要的jar
4. conf  配置文件, 对当前用户生效将setting.xml复制到用户administrator目录下的.m2文件夹里面
5. 

## maven HelloWorld
0. 目录结构
    + main -> java ->package
    + test -> java -> package
    + resources
    + POM.xml
        - groupId 项目包名
        - artifactId 是模块名 建议使用项目名标识
        - version 0.0.1SNAPSHOT
        - dependencies 项目依赖 > dependency > groupId+artifactId+version
1. mvn archetype:create -DgroupId=com.test.maven -DartifactId=test1 -DpackageName=com.test.test1
2. main 目录下存放运行时所需要的类和资源, test存放测试的时候所需要的类和资源
3. cd test2 && mvn install 运行该文件
4. mvn test 执行测试的过程  mvn compile 对main目录下的资源进行处理  
    mvn test-compile 只对测试目录下的资源和类进行处理   
    mvn clean 对test下的target清除  
    maven package 对target目录下打包 不会上传到repository
5. maven的生命周期
    + Process-resources
    + Compile
    + Process-classes
    + Process-test-resources
    + Test-Compile
    + Test

6. 上传jar到本地仓库
    + maven的本地仓库, 如果有需要的包自动加载到本地仓库中, 而且进行版本管理
    + 在.m2文件目录下面执行命令 (需要安装的jar在D盘 antlr目录)
        ```bash
        mvn install:install-file -Dfile=D:\antlr\antlr\2.7.7\ant-.7.7.jar -DgroupId=antlr -DartifactId=antlr -Dversion=2.7.7 -Dpackaging=jar
        ```
    + 在项目根路径下, 转成eclipse项目 (导入到eclipse) :  mvn eclipse:eclipse
    + 删除依赖后, 要让其在eclipse里面生效  mvn eclipse:clean 再次导入 mvn eclipse:eclipse
    

7. 依赖的作用范围
    + compile 默认行为, 对编译测试 运行 三种classpath有效
    + test 针对测试有效 如junit
    + provided 在运行时无效 对编译和测试有效 如servlet-api
    + runtime: 如jdbc 对测试和运行有效 对编译无效

8. maven插件的帮助信息
    + maven help:describe -Dplugin=eclipse  (-Dfull | -Ddetail)


## POM文件
1. 查看有效POM:  mvn help:effective-pom (超级POM和本地POM的叠加)
2. 2. 三大生命周期: clean(清理) defau(默认) site(站点)
3. POM信息
    + 项目的总体信息
    + 项目构建build reporting
    + POM关系元素
    + 构建环境


## Maven移植
1. 不可移植
2. 环境可移植
3. 组织内部可移植 (公司项目)
4. 广义可移植 (开源)

5. maven移植
    + Profile允许为移植或者特殊需要自定义一个特殊的构建
    + profilie位于 pom.xml, 可以覆盖所有的pom元素
    + profile抽取到profiles.xml中

6. 通用profile激活方式
    + nvm install -Pprofile-id
    + 通过activation条件匹配激活
    + 通过activeByDefault方式激活
    + 在setting.xml中 <activaeProfile>方式激活


-----------------

## Maven 核心知识 

1. 进入项目根目录
	1. mvn compile 对该项目进行编译
	2. mvn test 运行测试用例
		- 根目录下, 默认生成target文件夹
	3. mvn package 生成一个jar包(在target目录下)

2. maven常用构建命令
    1. maven -v 查看maven版本
    2. compile 编译
    3. test 测试
    4. package 打包
    5. clean 删除target
    6. install 安装jar到本地仓库

3. 自动创建目录骨架
    1. archetype 用户创建符合maven规定的目录骨架
        + src->main->java->主代码
        + scr->test->测试代码
    2. cd mavenDirectory && mvn archetype:generate 选择version6
    2. mvn archetype:generate -Dgroupid=com.maven04 -DartifactId=maven04-demo -Dversion=1.0.0SNAPSHOT -Dpackage=com.maven.model044.demo
        + -Dgroupid= 组织名.公司网址.项目名
        + -DartifactId= 项目名-模块名
        + -Dversion=版本号
        + -Dpackage=代码所在的包名

4. maven中的坐标和仓库
    1. 构件通过坐标作为其唯一的标识
        + groupId, artifactId, version作为依赖的唯一标识
    2. 仓库用来管理项目的依赖
        + 本地仓库
        + 远程仓库 
            - 安装目录 lib下
            - (查看远程 maven-model-builder.jar)-> pom.4.0.0.xml
            - repository->url
    3. 镜像仓库
        + 国内的镜像仓库
        + 修改镜像仓库的位置: conf->setting.xml -> (第158行左右)
            ```xml
            <mirror>
                <id>maven.net.cn</id>
                <mirrorOf>centeral</mavenOf>
                <name></name>
                <url></url>
            </mirror>
            ```
        + 更改本地仓库的位置
            - 默认本地 .m2-> repository
            - 修改settings.xml -><settings><localResitory>
            - 查看仓库的位置 mvn clean compile
    4. 在eclipse中安装maven插件
        + preferences->maven->user setting-> 修改setting.xml的位置


5. maven的生命周期和插件
    1. 清理 编译 测试 打包 集成测试 验证 部署
    2. maven三种生命周期: 
        + clean 清理项目
            - pre-clean 执行清理前的工作
            - clean 清理上一次构建生成的所有文件
            - post-clean 执行清理后的文件
        + default 构建项目 (最核心)
            - compile test package install
        + site 生成项目站点
            - presite 在生成站点前完成的工作
            - site 生成项目的站点文档
            - post-site 在生成项目站点后要完成的工作
            - site-deploy 发布生成站点到服务器上

6. maven中的pom.xml解析
    + pom常用元素介绍
        - modelVersion是固定版本 指定当前pom的版本
        - groupId 反写公司网址+项目名
        - artifactId 模块的标识 项目名+模块名
        - version 表示当前项目的版本号 第一个0大版本好 第二个分支版本 第三个小版本, 0.0.1SNAPSHOT RELEASE alpha内部测试 beta公测 GA正式发布
        - packageing 默认是jar  war zip ppom
        - name: 项目描述名
        - url 表示项目的地址
        - description 项目的描述
        - developers开发人员信息
        - licenses 许可证信息
        - dependencies>dependency
            1. scope 表示依赖范围  test只在测试范围有用
            2. optional true/false 设置依赖是否可选 默认false
            3. exclusions>exclusion 排除依赖传递列表
        - dependencyManagement
            1. dependencies>dependency
        - build
            1. plugins>plugin 插件的配置 groupid+artifactId+version
        - parent 子模块中对父模块的一个继承
        - modules 聚合多个模块,指定多个的模块一起编译
    + maven的依赖范围scope
        + test 只在测试阶段有效
        + compile(默认级别)
        + runtime 测试和运行时有效
        + system 与provided相同 移植性查
        + import 只使用在dependency > depdendencyManagement中, 表示在其他pom中导入depdency的配置
        + provided 在编译和测试的时候有
    + maven的依赖传递
        + maven依赖具有传递性
        + exclusitions>exclusion>groupId+artifactId+version
    + maven的依赖冲突
        + 如果有A和B依赖相同的构建
            - 短路优先 A->B->C-X(jar) A->D->X(jar) 路径端优先
            - 路径相同的情况下, 谁先声明先解析谁
    
    + 聚合和继承
        + 聚合: 将项目放到一起运行
            - 新建一个项目 将其packaging改成pom
            - modules>module{../module}+module{../moduleB}+module{../moduleC}
            - mvn clean install 分别进行三次构建 安装
        + 继承
            - 新建一个父类的项目 修改POM
            - 定义属性和使用
                ```xml
                <properties>
                    <junit.version>3.8.1</junit.version>
                </properties>
                <dependencyManagement>
                    <dependency>
                        <version>${junit.version}</version>
                    </dependency>
                </dependencyManagement>
                ````
            - 删除其src目录 其仅仅作为一个父级的继承类
            - 子类继承
                ```xml
                <parent>
                    <groupid>com.parent</groupid>
                    <artifactId>parent-model</artifactId>
                    <version>0.0.1-SNAPSHOT</version>
                </parent>
                
                ```
7. 使用maven构建web项目
    + 新建maven project
    + Filter->web-> maven-archetype-webapp
    + build-path -> configure buildpath -> source ->确保 Output (target/target-classes目录下)
    + 项目转化为web项目 右键属性->Project Facets->Dynamic Web Module
    + tomcat (配置在finalName下面)
    ```xml
    <pluginManagement>
      <plugins>
        <plugin>
          <groupId>org.apache.tomcat.maven</groupId>
          <artifactId>tomcat6-maven-plugin</artifactId>
          <version>2.2</version>
        </plugin>
        <plugin>
          <groupId>org.apache.tomcat.maven</groupId>
          <artifactId>tomcat7-maven-plugin</artifactId>
          <version>2.2</version>
        </plugin>
      </plugins>
    </pluginManagement>
    ```

