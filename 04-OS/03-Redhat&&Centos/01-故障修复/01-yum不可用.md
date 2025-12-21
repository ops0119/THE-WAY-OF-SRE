# 01-yum不可用

说明：因为自行安装了python等原因导致yum无法正常使用

```bash
1、先在相同环境的正常机器获取yum所需要的全部包
--------------------------------------------------------------------------------------
mkdir fixyum
cd fixyum
type repotrack yum|yum -y install yum-utils
repotrack yum
 #这条命令会自动下载yum相关的所有包以及依赖
 cd .. && tar -vcf fixyum.tgz fixyum
2、拷贝文件到异常机器，解压进入后执行
-----------------------------------------------------------------------------------------
tar -xf fixyum.tgz
cd fixyum
rpm -Uvh --nodeps --force *     #foce是为了解决环形依赖问题
```

