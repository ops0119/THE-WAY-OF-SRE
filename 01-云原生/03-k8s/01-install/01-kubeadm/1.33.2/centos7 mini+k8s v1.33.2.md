# centos7 mini+k8s v1.33.2

## 1、准备环境

### 1.1 配置静态地址
### 1.2 更换yum源+安装基础软件包+升级内核
```shell
mkdir /etc/yum.repos.d/bak
mv /etc/yum.repos.d/* /etc/yum.repos.d/bak/
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
curl -o /etc/yum.repos.d/epel.repo https://mirrors.aliyun.com/repo/epel-7.repo
yum clean all && yum makecache

yum install -y device-mapper-persistent-data lvm2 wget net-tools nfs-utils lrzsz gcc gcc-c++ make cmake libxml2-devel openssl-devel curl curl-devel unzip sudo libaio-devel wget vim-enhanced ncurses-devel autoconf automake zlib-devel  chrony  epel-release openssh-server socat  ipvsadm conntrack telnet 
```

### 1.3 设置主机名+配置hosts
```shell
hostnamectl set-hostname master1 && bash
hostnamectl set-hostname node1 && bash


vim /etc/hosts

192.168.8.188 master1
192.168.8.186 node1
```

### 1.4 配置认证
```shell
ssh-keygen
ssh-copy-id  node1
ssh-copy-id  master1
```

### 1.5 关闭交换分区
```shell
swapoff -a
sed -i '/swap/s/^/#/' /etc/fstab
```

### 1.6 关闭防火墙
```shell
#关闭firewalld
systemctl disable firewalld --now
#关闭selinux
setenforce 0
sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config
```

### 1.7 设置时间同步
```shell
yum -y install chrony
vim /etc/chrony.conf
#文件最后加上
------------------------------------------------------
server ntp1.aliyun.com iburst
server ntp2.aliyun.com iburst
server ntp1.tencent.com iburst
server ntp2.tencent.com iburst
-------------------------------------------------------
systemctl enable chronyd --now
crontab -e
* * * * * /usr/bin/systemctl restart chronyd
```

### 1.8 配置内核参数
```shell
modprobe br_netfilter
echo "br_netfilter" > /etc/modules-load.d/br_netfilter.conf

cat > /etc/sysctl.d/k8s.conf <<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF
sysctl -p /etc/sysctl.d/k8s.conf
```

## 2、部署k8s环境
### 2.1 安装containerd
```shell
yum -y install yum-utils
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
yum install containerd.io-1.6.22*  -y
mv /etc/containerd/config.toml /etc/containerd/config.toml.bak
sudo containerd config default | sudo tee /etc/containerd/config.toml

vim /etc/containerd/config.toml
----------------------------------------------------------------------------------------
#搜索config_path加入/etc/containerd/certs.d
[plugins."io.containerd.grpc.v1.cri".registry]
   config_path = "/etc/containerd/certs.d"
-----------------------------------------------------------------------------------------

#配置加速器
cd /etc/containerd/
curl -L -o certs.d.tgz "https://gitee.com/xdxghy/k8s_public0119/raw/master/01_containerd/certs.d.tgz"
tar -xf certs.d.tgz
systemctl restart containerd && systemctl enable containerd

```

### 2.2 安装k8s初始化包
```shell
cat >  /etc/yum.repos.d/kubernetes.repo <<EOF
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes-new/core/stable/v1.32/rpm/
enabled=1
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes-new/core/stable/v1.32/rpm/repodata/repomd.xml.key
EOF

yum clean all && yum makecache
yum install -y kubelet-1.32.3 kubeadm-1.32.3 kubectl-1.32.3
systemctl enable kubelet --now
```

### 2.3 初始化集群（master1上）
```shell
kubeadm config print init-defaults > kubeadm.yaml

#1、修改advertiseAddress未主节点的实际IP
#2、修改imageRepository未registry.cn-hangzhou.aliyuncs.com/google_containers
#3、追加信息
---
apiVersion: kubeproxy.config.k8s.io/v1alpha1
kind: KubeProxyConfiguration
mode: ipvs
---
apiVersion: kubelet.config.k8s.io/v1beta1
kind: KubeletConfiguration
cgroupDriver: systemd



kubeadm init --config=kubeadm.yaml

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### 2.4 部署网络插件
```bash
kubectl apply -f calico.yaml
```

### 2.5 添加node
```shell
kubeadm token create --print-join-command
```

### 2.6 证书续期（默认1年，延长到10年）
```bash
#查看证书有效期
openssl x509 -in /etc/kubernetes/pki/ca.crt -noout -text  |grep Not
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -noout -text  |grep Not
#证书续期
chmod +x update-kubeadm-cert.sh
./update-kubeadm-cert.sh all
```

## 3、报错
### 3.1 报错1
```shell
        [WARNING SystemVerification]: cgroups v1 support is in maintenance mode, please migrate to cgroups v2
error execution phase preflight: [preflight] Some fatal errors occurred:
        [ERROR SystemVerification]: kernel release 3.10.0-693.el7.x86_64 is unsupported. Recommended LTS version from the 4.x series is 4.19. Any 5.x or 6.x versions are also supported. For cgroups v2 support, the minimal version is 4.15 and the recommended version is 5.8+
        [ERROR SystemVerification]: unexpected kernel config: CONFIG_CGROUP_BPF
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
To see the stack trace of this error execute with --v=5 or higher


```

升级内核后在测试

### 3.2 报错2
```shell
failed to create new CRI runtime service: validate service connection: validate CRI v1 runtime API for endpoint "unix:///var/run/containerd/containerd.sock": rpc error: code = Unimplemented desc = unknown service runtime.v1.RuntimeService[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
To see the stack trace of this error execute with --v=5 or higher
```

containerd配置文件存在问题导致的

