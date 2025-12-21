# 01-load高但是CPU不高

## 1、找出系统中load高时处于运行队列的进程

这个脚本主要用于：

- **排查高 CPU load（注意：不是 CPU 使用率！）**
  - Linux 的 **load average** 包含 **R 状态（就绪队列） + D 状态（不可中断睡眠）** 的任务数。
  - 如果 load 很高但 CPU 使用率不高，很可能是大量进程卡在 **D 状态（比如磁盘 I/O 慢、NFS 卡住等）**。
- **长时间记录系统行为**（24 小时），便于事后分析性能瓶颈。
- **定位“幽灵”负载来源**：通过 `comm` 和 `pid/tid` 找出具体是哪个程序/线程导致高负载。

```bash
curl -s 'https://gitee.com/ops0119/the_-way_-of_-sre/raw/master/03-OS-Linux/01-故障排查/01-linux通用/01-CPU+load/script/load.sh' | bash
```

## 2、查CPU使用率比较高的线程脚本

持续（24 小时）采集系统中 CPU 占用最高的线程，并计算当前所有线程的 CPU 使用率总和（即“总 CPU 利用率”），同时记录 load average，用于深入分析高负载或性能异常问题。

```bash
curl -s 'https://gitee.com/ops0119/the_-way_-of_-sre/raw/master/03-OS-Linux/01-故障排查/01-linux通用/01-CPU+load/script/cpu_check.sh' | bash
```

