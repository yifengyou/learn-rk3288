# 编译完整Android系统

## 拉取Ubuntu 18.04 Docker镜像

```
docker pull ubuntu:18.04
```


## 启动docker

```
docker run -it --name android-build ubuntu:18.04 /bin/bash

添加本地路径映射，例如/data1目录

docker run -it --name android-build -v /data1:/data1 ubuntu:18.04 /bin/bash
```

## 修改软件源

```
sed -i 's/archive.ubuntu.com/mirrors.tencent.com/g' /etc/apt/sources.list
sed -i 's/security.ubuntu.com/mirrors.tencent.com/g' /etc/apt/sources.list
```

## 安装基本软件

```
apt-get update
apt-get install -y git-core gnupg flex bison build-essential \
  zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 \
  lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z1-dev \
  libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig \
  screen lrzsz openjdk-8-jdk openjdk-8-jre net-tools python python3 \
  vim bcc lzop rsync
```

## 配置repo工具

```
curl https://storage.googleapis.com/git-repo-downloads/repo > /bin/repo
chmod a+x /bin/repo
```

## 拉取android源码

* 初始化仓库

```
repo init -u https://bitbucket.org/TinkerBoard_Android/manifest.git -b /sbc/tinkerboard/asus/Android-7.1.2 -m android_20190515_v14.3.2.82.xml
```

* 拉取源码

```
repo sync -j8 -c -q
```

* -j8 specifies 4 download tasks at a time. I took down a corporate network running -j8 so please be careful.
* -c specifices "do not sync other branches." if i am looking for the 6.0 source code, do not also get the branch information for Android 7. This keeps the git info cleaner and speeds the sync up a little.
* -q is quiet otherwise you'll get your console window destroyed with output and it's really hard to tell what your progress is.




![20210521_154032_97](image/20210521_154032_97.png)

![20210521_154022_46](image/20210521_154022_46.png)


## 常见错误

1. SSL error when connecting to the Jack server

![20210520_173630_90](image/20210520_173630_90.png)

编译aosp 大概率会出现jack server 跑不起来然后抛一个类似这样的错误
原因就是编译时用的是open-jdk 8u292，默认禁用了TLSv1, TLSv1.1，


解决办法：

修改: /etc/java-8-openjdk/security/java.security

![20210520_174236_83](image/20210520_174236_83.png)


2. xmllint not found

```
apt-get install git-core gnupg flex bison build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig
```

解决办法：

* 参考：<https://source.android.com/setup/build/initializing>
* 务必先安装依赖
