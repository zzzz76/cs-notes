# Jetbrains



## 目录

1. MAC 删除 IntelliJ IDEA
2. IntelliJ IDEA 2021.2.4 破解激活



## 一、MAC 删除 IDEA

#### 步骤

1. 将应用移至废纸篓
1. 删除~/Library/Preferences/IntelliJIdea（IntelliJIdea后边跟版本发行日期）
1. 删除~/Library/Caches/IntelliJIdea（IntelliJIdea后边跟版本发行日期）
1. 删除~/Library/Application Support/IntelliJIdea（IntelliJIdea后边跟版本发行日期）
1. 删除~/Library/ApplicationSupport/IntelliJIdea（IntelliJIdea后边跟版本发行日期）
1. 删除~/Library/Logs/IntelliJIdea（IntelliJIdea后边跟版本发行日期）



#### 参考链接

1. [mac 上如何彻底删除 IDEA 等软件](https://zhuanlan.zhihu.com/p/96766913)
1. [mac app目录](https://segmentfault.com/a/1190000005035742)



## 二、IntelliJ IDEA 2021.2.4 破解激活

#### 步骤

试用：

1. 2.x版本之后，需要登入jetbrains 账号才能试用

2. 点击 [Start Trial] 开始使用

VM 配置：

3. 下载 agent 补丁

4. 修改 VM Options

```
-javaagent:/Users/xxxx/.jetbrains/FineAgent.jar
```

验证码：

5. 重启IDEA，让补丁生效

6. 在激活页面输入 Activation Code



#### 参考链接

1. [2021版本最新IDEA破解激活教程](http://www.itmind.net/14202.html)
