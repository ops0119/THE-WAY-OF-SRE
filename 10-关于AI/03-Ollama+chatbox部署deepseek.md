## 03-Ollama+chatbox部署deepseek

参考：https://www.cnblogs.com/youring2/p/18799312

## 1、ollama安装

### 1.1 在线安装

1、下载安装

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

查看版本

```bash
ollama --version
```

### 1.2 离线安装

参考：https://blog.csdn.net/sunyuhua_keyboard/article/details/144692663

```bash
wget https://ollama.com/install.sh

#查找
-----------------------------------------------------------
curl --fail --show-error --location --progress-bar \
    "https://ollama.com/download/ollama-linux-${ARCH}.tgz${VER_PARAM}" | \
    $SUDO tar -xzf - -C "$OLLAMA_INSTALL_DIR"

------------------------------------------------------------
找到后，修改为
$SUDO tar -xzf /root/ollama-linux-${ARCH}.tgz -C "$OLLAMA_INSTALL_DIR"

然后安装
bash install.sh
```



## 2、模型安装模型

### 2.1、查询支持下载的大模型

web方式查看https://ollama.com/library

命令查看

```bash
ollama list
```

### 2.2、下载运行模型

```bash
ollama run xxxx
```

## 3、chatbox

下载chatbox

https://chatboxai.app/zh 