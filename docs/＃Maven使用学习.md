＃Maven使用学习

作用：jar包放在仓库中，每次使用的时候不需要进行复制拷贝，只需要引用即可。

<groupId> cn.liyang.maven </groupId>  groupId是公司或者组织的域名倒序+当前项目名称

<artifactId>Hello</artifactId> artifactId 当前项目的模块名称

<version> 0.0.1-SNAPSHOT</version>version 当前模块的版本

不同技术官网的jar包下载形式五花八门的，有些技术官网是通过Maven或者SVN等工具来进行下载的

帮你打包文件，形成jar文件或者war文件，帮部署项目 

构建项目：

​	1.清理 把之前项目编译的东西删除，为新的编译代码做准备

​	2.编译  源代码为可执行代码，批量的 java—>class

​    3.测试 同时执行多个测试代码

​     4.报告 生成测试结果的文件 

​     5.打包 把项目中的所有class文件，配置文件等放到一个压缩文件中，Java项目通常是jar包 web文件压缩包war文件

​     6.安装  把生成的文件安装到本机仓库

​     7.部署 把程序安装好可以执行 

​     pom.xml 文件 项目对象模型文件 一个项目当作一个模型使用，控制maven构建项目的过程，管理jar依赖。/

项目Hello

​	Hello/

​	———/src

————/main 放java代码和配置文件

​						/java 程序包和包中的java文件

​						/resources  java程序中要使用的配置文件 

————/test 放测试程序代码和文件的（可以没有）、

​						/java 测试程序包和包中的java文件

​						/resources 测试java程序中要使用的配置文件 

———/pom.xml maven的核心文件（Maven项目必须有）

![image-20210520093644282](C:\Users\wang_\AppData\Roaming\Typora\typora-user-images\image-20210520093644282.png)

src下面有个pom.xml

Maven默认下载本地仓库

C: Users\登陆的用户名\ .m2\repository

管理依赖：

 1. dependencies和dependency相当于java代码中的import

    <dependencies>

    ​	<dependency>

    ​		<groupId>mysql</**>这些对应仓库中的mysql文件夹

    ​		<artifactId>mysql-connector-java</**>对应仓库中的mysql-connector-java文件夹

    ​		<version>5.1.9</**>对应5.1.9这个jar包

    ​	</dependency>

    </dependencies>

    添加这些依赖，就不用每次导包了

	2. 具体步骤

    	1. pom.xml中加入单元测试依赖，具体的可以去mvnrepository.com网站上搜索，并且加入到<dependencies>中

    	2. src/test/java目录下 创建测试程序  推荐测试类和方法的提示 

        	1. 测试类的名称是 Test+你要测试的类名

        	2. 测试的方法名称是 Test+ 方法名称

        	3. 例如要测试HelloMaven 创建测试类TestHelloMaven 

        	4. @Test

        	5. public void testAdd(){

            ​    测试HelloMaven的add方法是否正确

        	6. }

        	7. 其中测试方法 testAdd 必须是：

            	1. public 必须的
            	2. 没有返回值 必须的
            	3. 方法名称自定义 推荐Test+ 方法名称
            	4. 方法上面加上@Test

    	3. mvn clean 清理  删除原来编译和测试的目录，也就是target目录，但是已经install到仓库中的包不会删除

    	4. mvn compile编译主程序  会在当前目录下生成一个target 里面存放编译测试程序之后生成的字节码文件   会编译main/java下的所有Java为class文件，然后把所有class拷贝到target/classes目录下，把main/resources目录下面的所有文件都拷贝到target/classes目录下

        	1. 之后执行java class文件时候需要到target、classes下进行cmd指令 java 。。。类名

    	5. mvn test-compile 编译测试程序 会在当前目录下生成一个target 里面存放编译测试程序之后生成的字节码文件

    	6. mvn test 测试（生成一个目录 surefire-reports,保存测试结果） **前面的清理 编译过程都会做一遍** 修改后的文件也直接 mvn test即可

        	1. mvn package 打包主程序（会编译，编译测试，测试，并且按照pom.xml配置把主程序打包）  打包文件只包含src/main下面的所有内容

    	7. mvn install 安装主程序  会把本工程打包，并且按照本工程的坐标保存到本地仓库中

    	8. mvn deploy 部署主程序 会把本工程打包，按照本工程的坐标保存到本地库中，并且还会保存到私服仓库中，还会自动把项目部署到WEB仓库中  

    	9. **以上方法必须在命令行进入pom.xml文件所在目录**

    	10. 在 IDEA 的 Settings 窗口的 Build, Execution, Deployment > Build Tools > Maven > Runner 中对 VM Option 设置
         为 **-DarchetypeCatalog=internal **可选值为：remote，internal  ，local等，用来指定archetype-catalog.xml文件从哪里获取。  

         默认为remote，即从 http://repo1.maven.org/maven2/archetype-catalog.xml路径下载archetype-catalog.xml文件。

         Idea中 如果pom.xml文件没有自动刷新，可以右键pom.xml然后 Maven然后 Reimport

    	11. 依赖范围 <dependency>中有个<scope>  其中可以选 compile  test provided三个值 

         	1. scope表示依赖使用的范围，也就是maven构建项目的哪些阶段中起作用
             	1. maven构建项目  清理， 编译， 测试， 打包， 安装， 部署 过程（阶段）
             	2. 默认是compile![image-20210520165532042](C:\Users\wang_\AppData\Roaming\Typora\typora-user-images\image-20210520165532042.png) 
             	3. 比如说 项目开发过程中使用了jsp的依赖（Jsp相关的jar通过<dependency>加进来，）那么如果在里面《scope》写provided  那么在之后的打包，部署就不会加进来来这些jar包，否则 如打包时候 会出现lib文件夹，下有相关jar包 
             	4. provided部署时候 服务器提供，不需要自带
             	5. Maven使用自定义全局变量 
             	6. ![image-20210520170750498](C:\Users\wang_\AppData\Roaming\Typora\typora-user-images\image-20210520170750498.png)



![image-20210520170845518](C:\Users\wang_\AppData\Roaming\Typora\typora-user-images\image-20210520170845518.png)![image-20210520170901914](C:\Users\wang_\AppData\Roaming\Typora\typora-user-images\image-20210520170901914.png)

使用全局变量 便于多处修改	统一管理 



资源插件 <build>下的<resources>

![image-20210520172020830](C:\Users\wang_\AppData\Roaming\Typora\typora-user-images\image-20210520172020830.png) 



