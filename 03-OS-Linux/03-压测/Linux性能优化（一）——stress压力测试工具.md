> 参考：[https://blog.51cto.com/u_11529070/9255448](https://blog.51cto.com/u_11529070/9255448)
>

# 1、stress简介
## 1.1、stress
stress是Linux的一个压力测试工具，可以对CPU、Memory、IO、磁盘进行压力测试。

## 1.2 stress
```bash
sudo yum install stress
```

# 2、stress使用
## 2.1 stress命令
```bash
stress [OPTION [ARG]]
```

-c, --cpu N：产生N个进程，每个进程都循环调用sqrt函数产生CPU压力。

-i, --io N：产生N个进程，每个进程循环调用sync将内存缓冲区内容写到磁盘上，产生IO压力。通过系统调用sync刷新内存缓冲区数据到磁盘中，以确保同步。如果缓冲区内数据较少，写到磁盘中的数据也较少，不会产生IO压力。在SSD磁盘环境中尤为明显，很可能iowait总是0，却因为大量调用系统调用sync，导致系统CPU使用率sys 升高。

-m, --vm N：产生N个进程，每个进程循环调用malloc/free函数分配和释放内存。

    --vm-bytes B：指定分配内存的大小

    --vm-stride B：不断的给部分内存赋值，让COW(Copy On Write)发生

    --vm-hang N ：指示每个消耗内存的进程在分配到内存后转入睡眠状态N秒，然后释放内存，一直重复执行这个过程

    --vm-keep：一直占用内存，区别于不断的释放和重新分配(默认是不断释放并重新分配内存)

-d, --hdd N：产生N个不断执行write和unlink函数的进程(创建文件，写入内容，删除文件)

    --hdd-bytes B：指定文件大小



--hdd-noclean：不要将写入随机ASCII数据的文件Unlink

-t, --timeout N：在N秒后结束程序        

--backoff N：等待N微秒后开始运行

-q, --quiet：程序在运行的过程中不输出信息

-n, --dry-run：输出程序会做什么而并不实际执行相关的操作

--version：显示版本号

-v, --verbose：显示详细的信息

## 2.2 CPU测试
开启2个CPU进程执行sqrt计算，60秒后结束

```bash
stress --cpu 2 --timeout 60
```

## 2.3 IO测试
开启2个IO进程，执行sync系统调用，刷新内存缓冲区到磁盘

```bash
stress --io 2 --timeout 60s
```

使用stress无法模拟iowait升高，但sys升高。stress -i参数表示通过系统调用sync来模拟IO问题，但sync是刷新内存缓冲区数据到磁盘中，以确保同步。如果内存缓冲区内没多少数据，读写到磁盘中的数据也就不多，没法产生IO压力。使用SSD磁盘的环境中尤为明显，iowait一直为0，但因为大量系统调用，导致系统CPU使用率sys升高。

```bash
stress --io 2 --hdd 2 --timeout 60s
```

开启2个IO进程，2个磁盘IO进程

## 2.4 Memory测试
开启2个进程分配内存，每次分配1GB内存，保持100秒后释放，100秒后退出。

```bash
stress --vm 2 --vm-bytes 1G --vm-hang 100 --timeout 100s
```

## 2.5 磁盘IO测试
开启2个磁盘IO进程，每次写10GB数据到磁盘

```bash
stress --hdd 2 --hdd-bytes 10G --backoff 2000000
```

# 3、stress测试场景
## 3.1 CPU密集型进程
```bash
stress --cpu 2 --timeout 600
```

模拟启动2个CPU密集型进程

```bash
uptime
```

查看CPU使用情况，如下：

```bash
mpstat -P ALL 5 1
```

查看进程负载情况，如下：

```bash
pidstat -u 5
```

（1）通过uptime可以观察系统平均负载较高。

（2）通过mpstat观察到CPU0和CPU2的用户态CPU使用率很高，而iowait为0，说明进程是CPU密集型。进程使用CPU密集导致系统平均负载变高、CPU使用率变高。

（3）可以通过pidstat查看是stress进程导致CPU使用率较高。

## 3.2 IO密集型进程
模拟1个worker调用sync刷新内存缓冲区write到磁盘。

```bash
stress -i 1 --hdd 1 --timeout 600
```

查看CPU使用情况，如下：

```bash
mpstat -P ALL 5
```

（1）可以通过uptime观察到，系统平均负载很高。

（2）通过mpstat观察到内核态CPU使用率很低，但iowait很高，一直在等待IO处理，说明进程是IO密集型。进程频繁进行IO操作，导致系统平均负载很高而CPU使用率不高。

## 3.3 等待CPU进程
本机4个逻辑CPU，模拟8个进程。

```bash
stress -c 8 --timeout 600
```

查看系统平均负载，如下：

```bash
uptime
```

查看CPU使用率情况，如下：

```bash
mpstat -P ALL 5
```

查看进程的CPU使用情况，如下：

```bash
pidstat -u 5
```

（1）通过uptime观察到系统平均负载很高

（2）通过mpstat观察到用户态CPU使用率很高，iowait为0，说明进程是CPU密集型或者进程间存在CPU争用。

（3）通过pidstat观察到wait指标很高，说明进程间存在CPU争用，系统中存在大量进程在等待使用CPU。

# 4、stress-ng简介
## 4.1 stress-ng简介
stress-ng完全兼容stress, 并且在stress基础上增加数百个选项参数，支持产生各种复杂的压力。

## 4.2 stress-ng安装
stress-ng源码下载：



编译：



```bash
make
```

安装：

```bash
sudo make install
```

3、stress-ng命令

```bash
stress-ng [OPTION [ARG]]

 


 

 


stress-ng --cpu 2 --cpu-method pi
```



产生2个worker做圆周率算法压力

```bash
stress-ng --cpu 2 --cpu-method all
```

产生2个worker迭代使用30多种不同的压力算法，包括pi, crc16, fft等

```bash
stress-ng --sock 2
```

产生2个worker调用socket相关函数产生压力

```bash
stress-ng --tsc 2
```

产生2个worker读取tsc产生压力

```bash
stress-ng --sock 4 --taskset 0-1,3
```

strss-ng将压力指定到指定CPU上



















