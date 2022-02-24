---
layout: post
title: Harmony 开发基础
subtitle:  Harmony 学习笔记
date:   2022-2-24
author:  物联黄同学
header-img: img/20220223.jpg
catalog:   true
tags:
    - 华为鸿蒙
    - 学习
---
# Harmony 开发基础——Harmony 学习笔记

## 前言

> 最近跟着[3.6 HAR | 3.6 HAR | EBG2021CCHW1100031 课程页面 | Huawei iLearningX](https://ilearningx.huawei.com/courses/course-v1:HuaweiX+EBG2021CCHW1100031+2021111505/courseware/1514963ef078441096fd05a4d94c4129/bbb787db9c264999ad44a100cde6f3f3/)学习了鸿蒙开发基础，做了一下笔记。

### MindMap

![Lfx05y.png](https://s6.jpg.cm/2022/02/24/Lfx05y.png)



## 一 APP

> HarmonyOS 的应用软件包以APP Pack, 有一个或多个HAP 以及描述每个HAP数学的pack.info组成。HAP是Abilities的部署包，代码围绕Ability组件展开。

### 1. 组成

1. 一个HAP 有 代码、资源、第三方库及应用配置文件组成的模块包，有两种模块包 entry 和 feature。
   1. entry：主模块，一个APP中，对于同一设备类型有且只有一个entry类型的HAP，可以独立安装运行。
   2. feature：动态特新模块，一个APP可包含一个或多个feature类型的HAP，可不包含。只有包含Ability的HAP能独立运行。
2. HAP由零个或一个或多个Ability组成。

### 2. 结构

![Lfo2JD.png](https://s6.jpg.cm/2022/02/23/Lfo2JD.png)

## 二 Ability

> 1. 应用能力的抽象，一个应用包含一个或多个Ability。两种类型：FA和PA，是应用的基本单元，实现特定功能，FA有UI，PA无UI。（欸，是不是很像前端和后端啊）
> 2. 库文件是应用的第三方代码。
> 3. 配置文件包含Ability的配置信息，用于声明Ability，应用所需权限等。

### 1. pack.info

1. 描述HAP属性，由IDE生成，HAP具体属性包括：
   1. delivery-with-install: 表示HAP是否支持随应用安装。true表示支持。
   2. name：HAP文件名。
   3. module-type：模块类型，entry或feature。
   4. device-type：支持该HAP运行的设备类型。



## 三 库文件

HAR全名HarmonyOS Ability Resources，提供构建文件所需内容，包括源代码、资源文件、和config.json 文件。不同于HAP，不能独立安装运行，只能被引用。



## 四 资源文件

### 1. 应用资源文件

放在resourece目录下，便于开发者实用和维护。包括base目录和限定词目录。

### 2. resource 目录结构

![Lfyz84.png](https://s6.jpg.cm/2022/02/23/Lfyz84.png)

### 3. 限定词目录

由表征应用场景或设备特征的限定词组合，包括语言，文字，国家或地区，横竖屏，设备类型和屏幕密度六个维度。限定词之间通过下划线或者中划线“-” 连接。

限定词目录下包含限定词文件，分为三类，element，media，animation（动画资源）。



### 4. base目录限定词引用

1. 在应用开发的hml和js文件使用$r语法，可以对JS模块内的resources目录下的json资源进行格式化，获取相应的资源内容。
2. 对于js页面对象的属性$r, 可以使用string类型的key参数，返回值为string类型。

### 5. 系统资源文件

![LfyMTH.png](https://s6.jpg.cm/2022/02/23/LfyMTH.png)



## 五 配置文件

每个HAP根目录下存在一个“config.json” 配置文件。主要内容：

1. 应用的全局配置信息，包含应用的包名、生产厂商、版本号等。
2. 在具体设备的配置信息，包含应用的备份恢复、网络安全等。
3. HAP包的配置信息，每个Ability必须定义的基本属性，一些权限.

### 1. 配置文件的内部结构

由 app、deviceConfig、module 三个部分组成。

![Lf9up4.png](https://s6.jpg.cm/2022/02/23/Lf9up4.png)

### 2. app 对象的内部结构

包含应用的全局配置信息。

![Lf91aX.png](https://s6.jpg.cm/2022/02/23/Lf91aX.png)

### 3. deviceConfig 对象的内部结构

包含具体设备上的应用配置信息，可以包含以下图中的属性。

![Lf9kXS.png](https://s6.jpg.cm/2022/02/23/Lf9kXS.png)

#### phone 对象

![Lf9pa2.png](https://s6.jpg.cm/2022/02/23/Lf9pa2.png)

### 4. module 对象的内部结构

包含HAP包的配置信息。

![LfTLlX.png](https://s6.jpg.cm/2022/02/23/LfTLlX.png)

![LfTewS.png](https://s6.jpg.cm/2022/02/23/LfTewS.png)

#### 1） distro对象

HAP发布的具体描述。

![LfTlxW.png](https://s6.jpg.cm/2022/02/23/LfTlxW.png)

#### 2） js对象的内部结构

表示基于JS UI开发的JS模块集合

![LfTVl2.png](https://s6.jpg.cm/2022/02/23/LfTVl2.png)

#### 3） abilities 对象

##### 核心属性

![LfTgjD.png](https://s6.jpg.cm/2022/02/23/LfTgjD.png)

##### 次要属性

![LfTiyH.png](https://s6.jpg.cm/2022/02/23/LfTiyH.png)

##### skills 对象的内部结构

表示接收Ability能够接收的Intent的特征。

![Lfack4.png](https://s6.jpg.cm/2022/02/23/Lfack4.png)

##### forms 对象的内部结构

表示服务卡片的属性。

![LfaU9X.png](https://s6.jpg.cm/2022/02/23/LfaU9X.png)

### 5. HAP与HAR的配置文件的合并

如果应用模块中调用了HAR，在编译构建HAP时，需将HAP的config.json 文件与一个或多个HAR的config.json 文件，合并为一个config.json 文件。在合并过程中，不同文件的同一个标签的取值可能发生冲突，此时，需要通过配置mergeRule 来解决冲突。

#### 合并规则

1. HAP与HAR的config.json 文件合并时，需要将HAR的配置信息全部合并到HAP的配置文件。
2. HAP的优先级总是高于HAR，当HAP依赖多个HAR时，先加载的优先级高于后加载的HAR。



## 六 HAR

### 1. 在工程中添加Module

Module 是 HarmonyOS 应用的基本功能单元，包含源代码、资源文件、第三方库及应用清单文件，每一个Module都可以独立进行编译和运行。一个应用通常包含一个或多个Module，Module分Ability和Library（HarmonyOS Library 和 Java Library）两种类型。



### 2. Module 类型

1. 在一个APP中，对于同一类型设备有且只有一个Entry Module，其余Module 的类型均为Feature。
2. 创建Module遵循以下原则：
   1. 若新增Module的设备类型已经存在，则设为Feature。
   2. 否则，自动设为Entry。

### 3. 创建HarmonyOS库

#### 1）创建库Module

![LfiqfE.png](https://s6.jpg.cm/2022/02/23/LfiqfE.png)



#### 2）将库模块编译为HAR

1. 利用Gradle 可以将HarmonyOS Library库模块构建为HAR包。
2. 构建HAR包的方法：在Gradle 构建任务中，双击debugHarmonyHar 或 releaseHarmonyHar任务，构建Debug类型或Release类型的HAR。

#### 3）编译并输出库模块

![LfiWZQ.png](https://s6.jpg.cm/2022/02/23/LfiWZQ.png)



### 4. 发布Har包到Maven 仓

> 借助Gradle 提供的Mave-publish 插件，可以将Har包发布到本地或远程Maven 仓，方法如下：
>
> 1. 在工程根目录下，New > File，创建一个以 .grdale 结尾的文件。
> 2. 在创建的grdle文件中添加代码。
> 3. 在Har模块的build.grdle 中，添加HAR发布脚本，添加完成后，请点击Sync Now进行同步。
> 4. 同步完成后，在Gradle任务中添加publishing 的任务列表，并执行pulishMavenPublicationToMavenRepository 任务，将HAR包发布到指定的Maven地址。



### 5. 为应用模块添加依赖

在应用模块调用HAR，常用的添加依赖的方式有两种：

1. 调用同一个工程中的HAR：HAR包和应用模块在同一个工程，打开应用模块的build.gradle文 件，在dependencies闭包中，添加如下代码。添加完成后，请点击Sync Now同步工程。 

   1. ```
      dependencies {
      implementation project(":mylibrary")
      }
      ```

      

2. 调用Maven仓中的HAR：无论Har包是本地Maven仓还是远程Maven仓，均可以在工程的 build.gradle的allprojects闭包中，添加HAR所在的Maven仓地址。

   1. ```
      repositories {
      maven {
      url 'file://D:/01.localMaven/' }}
      ```



## 后话

> 爱护肠胃吧，本来能早点写完的，但是因为肠胃一直拖着。。。。

