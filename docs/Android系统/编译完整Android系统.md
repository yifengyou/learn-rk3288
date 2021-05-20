# 编译完整Android系统

## 常见错误

1. SSL error when connecting to the Jack server

![20210520_173630_90](image/20210520_173630_90.png)

编译aosp 大概率会出现jack server 跑不起来然后抛一个类似这样的错误
原因就是编译时用的是open-jdk 8u292，默认禁用了TLSv1, TLSv1.1，



解决办法：

![20210520_174236_83](image/20210520_174236_83.png)


2.  sdf

解决办法：
