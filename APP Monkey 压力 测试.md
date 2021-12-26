### APP Monkey 压力 测试

### 1、环境搭建

- 安装JDK1.8 

  - 去Java官⽹下载JDK1.8（推荐这个)，安装 配置java 环境变量 ，配置完再终端输⼊**java -version** 或者 **java** 不报错即代表配置成功

    

- 安装 Android SDK

  - 去官⽹下载 Android SDK，安装并配置好android sdk环境变量，配置完在终端输⼊**adb** 或 **adb shell monkey** 不报错即代表配置成功

### 2、Monkey介绍

- 顾名思义，Monkey就是猴⼦， Monkey测试，就像⼀只猴⼦， 在手机⾯前，乱敲屏幕在测试。 我们可以通过Monkey程序模拟⽤户触摸屏幕、滑动Trackball、 按键等操作来对设备上的程序进⾏压⼒和稳定性测试，检测程序多久的时间会发⽣异常

### 3、monkey 用来做什么？

- Monkey 主要⽤于Android 的压⼒测试 ⾃动的⼀个压⼒测试⼩⼯具， 主要⽬的就是为了测试app 是否 会Crash。

### 4、Monkey 命令启动⽅式(介绍三种)

- 可以在电脑终端（Terminal）中执⾏: adb shell monkey ｛+命令参数｝来进⾏Monkey测试
- 可以在电脑终端（Terminal）adb shell 先进⼊Android系统，通过执⾏ monkey {+命令参数} 来进⾏Monkey 测试 
- 可以在Android机上安装Android终端模拟器，在Android机或者模拟器上直接执⾏monkey 命令（不常用，了解）

### 5、获取APP应⽤包名⽅式

- 获取哪个应用app包名，就打开哪个app，在终端输入以下命令
  Mac/Linux:  adb shell dumpsys window | grep mCurrentFocus 
  Windows ： adb shell dumpsys window windows | findstr mFocusedApp

- 示例:终端输入 adb shell dumpsys window | grep mCurrentFocus ，回车，输出如下：

  mCurrentFocus=Window{78c21e5 u0 com.example.admin.tenantframework/com.example.admin.tenantframework.mvp.ui.activity.HomeActivity}

  则com.example.admin.tenantframework为该包的包名。
  注意：包名一般以com 或者 cn开头

### 6、Monkey输出日志命令格式

- adb shell monkey -p 包名 执行点击次数 > 路径/log.txt
- 示例：adb shell monkey -p cn.goapk.market 100 > 路径/log.txt

### 7、Monkey常用参数介绍

- -p参数

   

  <允许的包名列表> 

  ⽤此参数指定⼀个或多个包。指定包之后，monkey将只允许系统启动指定的app。如果指定包， monkey将允许系统启动设备中的所有app。

  - 指定⼀个包：adb shell monkey -p cn.goapk.market 100 

  - 指定多个包：adb shell monkey -p fishjoy.control.menu –p cn.goapk.market 100 

    

- -v 参数 

  ⽤于指定输出⽇志级别（级别就是⽇志的详细程度），总共分3个级别，分别对应的参数如下 表所示：

  - **Level 0 : adb shell monkey -p cn.goapk.market -v 100 // 缺省值，仅提供启动提示、测试完 成和最终结果等少量信息 
    **
  - **Level 1 : adb shell monkey -p cn.goapk.market -v -v 100 // 提供较为详细的⽇志，包括每个发 送到Activity的事件信息（通常两个-v就够用）
    **
  - **Level 2 : adb shell monkey -p cn.goapk.market -v -v -v 100 // 最详细的⽇志，包括了测试中选中/ 未选中的Activity信息
    **
    **
    **

- **--throttle <毫秒>** 
  **⽤于指定⽤户操作（即事件）时间的时延，单位是毫秒；如果指定这个参数，monkey会尽可能快的 ⽣ 成和发送消息。 示例：adb shell monkey -p cn.goapk.market --throttle 3000 100 
  **
  **
  **

- **--ignore-timeouts <忽略超时时间，毫秒>**
  **通常⽤于应⽤程序发⽣任何超时错误（如“Application Not responding”对话框）Monkey将停⽌运 ⾏，设置此项，Monkey将继续发送事件给系统，直到事件计数完成。示例：adb shell monkey -p cn.goapk.market --ignore-timeouts 100 
  **
  **
  **

- **-s <随机种⼦数>**

   ⽤于指定伪随机数⽣成器的seed值，如果seed相同，则两次Monkey测试所产⽣的事件序列也相同的。 

  示例：

  - **monkey测试1：adb shell monkey -p cn.goapk.market –s 10 100 
    **
  - **monkey测试2：adb shell monkey -p cn.goapk.market –s 10 100 
    **

### 8、Monkey⽇志分析

- 正常情况

  - 如果Monkey测试顺利执⾏完成， 在log的最后， 会打印出当前执⾏事件的次数和所花费的时 间； // Monkey finished 代表执⾏完成

  - Monkey执⾏中断，在log的最后也能查看到当前已执⾏的次数

  - 示例：执行下列命令在发送第824次随机事件时发生崩溃停止

    adb shell monkey -p com.example.admin.tenantframework 100

    ![img](http://wiki.corp.shiqiao.com/download/thumbnails/47415318/image2021-12-10_14-7-25.png?version=1&modificationDate=1639116448820&api=v2)

- 异常情况：在⽇志中搜索以下关键字

  - ⽆响应问题可以在⽇志中搜索 “ANR” 。 
  - 崩溃问题搜索 “CRASH” 。 
  - 内存泄露问题搜索"GC"（需进⼀步分析） 
  - 异常问题搜索 “Exception”（如果出现空指针， NullPointerException，需格外重视）。