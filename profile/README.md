```text
      _____ _     _
  __ /__   \ |__ (_)_ __   __ _
 / _` |/ /\/ '_ \| | '_ \ / _` |
| (_| / /  | | | | | | | | (_| |
 \__,_\/   |_| |_|_|_| |_|\__, |
                          |___/

Just a Thing
```

# 最好用的阿里云IoT-Java客户端

![License](https://img.shields.io/badge/license-MIT-brightgreen)
![Language](https://img.shields.io/badge/language-java-brightgreen)

## 背景简介

物联网出现后，阿里云首先提出了物模型的概念。物模型将设备控制行为高度抽象归类为`属性`、`服务`和`事件`。阿里云团队将主要的精力投入到了Linux设备的支持和完善中，可以在他们的C/C++工程架构中看到非常完善的实现和优雅的工程架构，但其在Java上的支持则显得比较随性。在实际使用过程中发现阿里云封装的IoT-Java客户端有以下缺点：

- **缺少抽象：** 面向功能设计的API，在实际研发过程中为面向API编程而不是面向对象编程，无法发挥物模型的抽象优势；

- **缺少封装：** 需要深入了解alink协议，增加研发复杂度，无法聚焦在业务功能开发上；

所以我使用Java对阿里云的alink协议以及必要的IoT功能组件重新进行了实现和封装，沉淀为aThing框架。名字取名来自于`Just a Thing`和`aliyun thing`双关语。

## 框架特点

如果你和我一样：“基于阿里云IoT平台用Java语言研发，并对基于阿里云SDK的设备端和服务端开发感到繁琐和气馁”。aThing框架说不定能在一定程度上帮助到你降低对接阿里云IoT平台的成本。

### 框架优势

框架的定位为阿里云IoT设备端SDK和服务端SDK的Java重实现，运行在 **JRE[17,+)** 环境下。aThing具有以下优势：

- API高度封装alink协议，让你免除MQTT处理、JSON解析的繁琐，降低编码复杂度。
- 异步函数式编程，能用更少资源稳定的运行在弱网环境

### 部分实现了alink协议和功能组件

aThing对阿里云的alink协议（v1.5）和部分功能组件进行了实现，主要还是从我实际需要出发，有选择性的优先支持部分功能，后续的功能看实际需要实现。

|组件 / 功能|支持情况|模块地址|
|---|---|---|
|核心框架|支持|[athing](https://github.com/athingx/athing)|
|设备模型|支持|[athing-dm](https://github.com/athingx/athing-dm)|
|远程升级|支持|[athing-upgrade](https://github.com/athingx/athing-upgrade)|
|远程配置|支持|[athing-config](https://github.com/athingx/athing-config)|
|远程调试|支持|[athing-tunnel](https://github.com/athingx/athing-tunnel)|
|设备标签|联调|[athing-tag](https://github.com/athingx/athing-tag)|
|子设备|暂不支持||
|期望属性值|暂不支持||
|文件管理|暂不支持||
|设备分组|暂不支持||
|NTP服务|暂不支持||
|设备任务|暂不支持||
|设备分发|暂不支持||
|日志上报|暂不支持||
|设备影子|暂不支持||

## 简化代码例子

### 构建客户端

```java
final var thing = new ThingBuilder(new ThingPath(PRODUCT_ID,THING_ID))
        .client(new AliyunMqttClientFactory()
            .secret(SECRET)
            .remote(REMOTE))
        .build();
```

### 远程配置推送

```java
thing.op().binding("/sys/%s/%s/thing/config/push".formatted(PRODUCT_ID, THING_ID))
        .map(mappingJsonFromByte(UTF_8))
        .bind((topic,json)->{

        });
```
