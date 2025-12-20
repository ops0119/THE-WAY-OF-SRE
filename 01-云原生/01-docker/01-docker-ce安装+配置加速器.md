# 01-docker-ce安装+配置加速器

## 1、安装

https://mirrors.huaweicloud.com/mirrorDetail/5ea14d84b58d16ef329c5c13?mirrorName=docker-ce&catalog=docker

## 2、加速器配置

```bash
sudo mkdir -p /etc/docker
mv /etc/docker/daemon.json /etc/docker/daemon.json.backup
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://docker.211678.top","https://docker.1panel.live","https://hub.rat.dev","https://docker.m.daocloud.io","https://do.nark.eu.org","https://dockerpull.com","https://dockerproxy.cn","https://docker.awsl9527.cn"]
}
EOF
sudo systemctl daemon-reload;sudo systemctl restart docker
```

