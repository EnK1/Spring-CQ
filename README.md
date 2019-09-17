# Spring-CQ
[![Build Status](https://travis-ci.org/lz1998/spring-cq.png)](https://travis-ci.org/lz1998/spring-cq)
[![QQ群](https://img.shields.io/static/v1?label=QQ%E7%BE%A4&message=335783090&color=blue)](https://jq.qq.com/?_wv=1027&k=5BKAROL)

基于 酷Q、cqhttp、SpringBoot、反向websocket 的 QQ 机器人框架

## 编写插件
在plugin下写XXXPlugin，继承CQPlugin

```java
@Component // 加入容器
@Order(1) // 插件执行顺序，有多个插件，这个插件第几个执行
public class SayPlugin extends CQPlugin {
    @Override
    public int onPrivateMessage(CoolQ cq, CQPrivateMessageEvent event) {
        long user_id = event.getSender().getUserId();
        String msg = event.getMessage();
        if (msg.startsWith("/say")) {
            cq.sendPrivateMsg(user_id, msg.substring(4), false);
        }
        return MESSAGE_IGNORE; // 继续执行下一个插件
        // return MESSAGE_BLOCK; // 不执行下一个插件
    }
}
```
    
## 测试应用
1. 修改application.properties的端口，默认8081
2. 运行SpringCqApplication的main方法

## 打包应用
1. 使用maven打包应用
    ```bash
    mvn clean package
    ```
2. 在target目录下，spring-cq-0.0.1-SNAPSHOT.jar即为打包的jar

## 运行应用
1. 输入指令
    ```bash
    java -jar spring-cq-0.0.1-SNAPSHOT.jar
    ```

## 运行酷Q和cqhttp(Windows)
1. 点击 [下载](http://cq.lz1998.xin/CQA.zip) 配置好的 酷Q Air 
2. 解压后运行 CQA.exe 登录QQ账号 


## 运行酷Q和cqhttp(Docker)
1. 安装酷Q和CQHTTP插件
    ```shell
    docker run -d --name cq01 \
    -v $(pwd)/coolq:/home/user/coolq \
    -p 9000:9000 \
    -e VNC_PASSWD=你的VNC密码(不超过8位) \
    -e COOLQ_URL=http://dlsec.cqp.me/cqa-tuling \
    -e COOLQ_ACCOUNT=你的机器人QQ号 \
    -e CQHTTP_USE_HTTP=false \
    -e CQHTTP_USE_WS_REVERSE=true \
    -e CQHTTP_WS_REVERSE_API_URL=ws://127.0.0.1:8081/ws/api/ \
    -e CQHTTP_WS_REVERSE_EVENT_URL=ws://127.0.0.1:8081/ws/event/ \
    richardchien/cqhttp
    ```
2. 如果有问题可以访问 http://127.0.0.1:9000 使用noVNC查看酷Q日志