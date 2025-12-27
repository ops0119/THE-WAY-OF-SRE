# 02-pip配置国内镜像

参考：

https://developer.aliyun.com/mirror/pypi?spm=a2c6h.13651102.0.0.2ac31b11t6nsNy

https://mirrors.huaweicloud.com/mirrorDetail/5ea148ce302e67c59c8fe161?mirrorName=pypi&catalog=language



## 阿里云

找到下列文件~/.pip/pip.conf

```bash
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host=mirrors.aliyun.com
```

```
pip config list
pip3 config set global.index-url https://mirrors.aliyun.com/pypi/simple/
```



## 华为云

~/.pip/pip.conf

```bash
[global]
index-url = https://mirrors.huaweicloud.com/repository/pypi/simple
trusted-host = mirrors.huaweicloud.com
timeout = 120
```

制作docker镜像时，请使用如下方式配置pip源

```bash
RUN pip config --user set global.index https://mirrors.huaweicloud.com/repository/pypi
RUN pip config --user set global.index-url https://mirrors.huaweicloud.com/repository/pypi/simple
RUN pip config --user set global.trusted-host mirrors.huaweicloud.com
```

