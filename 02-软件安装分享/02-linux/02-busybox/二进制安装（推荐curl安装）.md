下载地址：[https://www.busybox.net/downloads/binaries/](https://www.busybox.net/downloads/binaries/)

## 1、二进制安装
```bash
wget https://busybox.net/downloads/binaries/1.21.1/busybox-x86_64 --no-check-certificate
chmod +x busybox-x86_64
mv busybox-x86_64 /usr/local/bin/busybox
```

使用

```bash
busybox top
busybox ps
```



```bash
type busybox ||curl -o /usr/bin/busybox https://busybox.net/downloads/binaries/1.21.1/busybox-x86_64;chmod +x /usr/bin/busybox 
```

## 2、docker

```bash
sudo docker run -it --rm --privileged --pid=host busybox
```

这个命令将会在docker中运行一个busybox的容器，并且通过--privileged选项和--pid=host选项获取宿主机的资源。

```bash
top
```

