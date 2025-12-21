# 01-load高但是CPU不高

## 1、找出系统中load高时处于运行队列的进程

```bash
curl -s 'https://gitee.com/ops0119/the_-way_-of_-sre/raw/master/03-OS-Linux/01-故障排查/01-linux通用/01-CPU+load/script/load.sh' | bash
```

## 2、查CPU使用率比较高的线程脚本

```bash
curl -s 'https://gitee.com/ops0119/the_-way_-of_-sre/raw/master/03-OS-Linux/01-故障排查/01-linux通用/01-CPU+load/script/cpu_check.sh' | bash
```

