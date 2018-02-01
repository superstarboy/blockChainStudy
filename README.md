# 关于bitcoin的编译部分

较为简单的一部分，通过vm虚拟机的ubuntu16.0.4系统编译bitcoin。[在windows 10上编译bitcoin源码](https://zhuanlan.zhihu.com/p/25021080)。

不过新版本有所改动和一些不能再次编译的bug，过程会有一些修改

## ubuntu 环境的搭建

将ubuntu下载源改为国内镜像，（我用的是阿里云）。
```bash
sudo vim /etc/apt/sources.list
```
将下面的代码复盖文件
```
# deb cdrom:[Ubuntu 16.04 LTS _Xenial Xerus_ - Release amd64 (20160420.1)]/ xenial main restricted
deb-src http://archive.ubuntu.com/ubuntu xenial main restricted #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted multiverse universe #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted multiverse universe #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe
deb http://mirrors.aliyun.com/ubuntu/ xenial multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse #Added by software-properties
deb http://archive.canonical.com/ubuntu xenial partner
deb-src http://archive.canonical.com/ubuntu xenial partner
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted multiverse universe #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-security multiverse
```
运行
```bash
sudo apt update
```

## 安装环境包
```bash
sudo apt-get install build-essential libtool autotools-dev automake pkg-config bsdmainutils curl
sudo apt install g++-mingw-w64-x86-64
sudo apt install software-properties-common
sudo add-apt-repository "deb http://archive.ubuntu.com/ubuntu zesty universe"
sudo apt update
sudo apt upgrade
sudo update-alternatives --config x86_64-w64-mingw32-g++ # 选择结尾为posix的包，我这是1.
cd /usr/src
sudo git clone https://github.com/bitcoin/bitcoin.git # 可以选择加入自己修改过的包
sudo chmod -R a+rw bitcoin
PATH=$(echo "$PATH" | sed -e 's/:\/mnt.*//g')
cd depends
sudo make HOST=x86_64-w64-mingw32 # 这步有在第二次编译的时候有时候会提示脚本命令不存在，试着使用 find ./bitcoin -type f | xargs -Ix sed -i.bak -r 's/\r//g' x 删除文件夹下的后缀脚本
cd ..
./autogen.sh
sudo CONFIG_SITE=$PWD/depends/x86_64-w64-mingw32/share/config.site ./configure --prefix=/
sudo make -j
```
## 编译exe文件
完成环境的配置之后接下来就是exe文件的编译，运行命令
```bash
sudo install DESTDIR=/usr/src/bitcoinexe # 后面加上你的目标编译目录
```


# 区块浏览器








