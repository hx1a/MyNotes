# 在IDEA中运行IBM-fabric-java-sdk样例

## 打开工程

File -> open... 选择java文件夹

![image-20201104083409589](/../images/image-20201104083409589.png)

打开后package、import标红，因为这里package从org开始，所以源文件目录需要定位到org的上级目录

解决办法：File -> Project Structure -> Modules

找到main下的java文件夹，右键设为Sources，将原来的src删除

![image-20201104083534440](https://github.com/xiahan1997/MyNotes/Fabric/images/image-20201104083534440.png)

点击OK，此时package标红消失，import标红仍在。

重启IDEA，import标红消失。

## Maven Install

点击IDEA窗口最右侧的Maven侧边栏，调出Maven相关操作与信息的菜单后使用Lifecycle中的install（双击）进行安装

![image-20201104101849934](https://github.com/xiahan1997/MyNotes/Fabric/images/image-20201104101849934.png)

## 尝试运行与处理报错

启用network

```shell
cd network
./build.sh
```

在IDEA中打开/network/CreateChannel.java，右击run，出现报错

![image-20201104091005002](/images/image-20201104091005002.png)

这是由于依赖了log4j的jar包，而使用log4j需要先编写其配置文件log4j.properties

在main下新建文件夹，命名为resources，在其中新建文件命名为log4j.properties，添加如下内容（可自行更改想要的日志格式）

```xml
### set log levels ###
log4j.rootLogger = debug,stdout,R 

### console appender###
log4j.appender.stdout = org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target = System.out
log4j.appender.stdout.Threshold = Info
log4j.appender.stdout.layout = org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern = %d [%t] %p [%c] - %m%n

### output to files ###
log4j.appender.R = org.apache.log4j.DailyRollingFileAppender
log4j.appender.R.File = logs/log.txt
log4j.appender.R.Append = true
log4j.appender.R.Threshold = Info 
log4j.appender.R.layout = org.apache.log4j.PatternLayout
#log4j.appender.R.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss}-[%p]%m%n
log4j.appender.R.layout.ConversionPattern = %d [%t] %p [%c] - %m%n
```

打开Project Structure，将resources文件夹设置为Resources Folder

![image-20201104093845934](https://github.com/xiahan1997/MyNotes/blob/Fabric/images/image-20201104093845934.png)

再次运行/network/CreateChannel.java，log4j warning消失，留下空指针异常：

![image-20201104094512064](https://github.com/xiahan1997/MyNotes/Fabric/images/image-20201104094512064.png)

查看所给命令

```shell
cd ../../network_resources
java -cp blockchain-client.jar org.example.network.CreateChannel
```

发现是在network_resources文件夹下执行的，因此将network下的内容（chaincode、config、crypto-config三个文件夹）拷贝到blockchain-application-using-fabric-java-sdk/java目录下，再次运行，成功。

