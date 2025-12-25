```bash
cat /proc/sys/net/ipv4/icmp_echo_ignore_all
cat /etc/sysctl.conf |grep icmp_echo_ignore_all
```

参数：

0为允许icmp

1为禁止

临时开启

```bash
echo 0 >/proc/sys/net/ipv4/icmp_echo_ignore_all   
```



## 永久禁止ping
```bash
cat >> /etc/sysctl.conf <<EOF
net.ipv4.icmp_echo_ignore_all=1
EOF
sysctl -p
```

如果已经有net.ipv4.icmp_echo_ignore_all这一行了，直接修改=号后面的值即可的（0表示允许，1表示禁止）。



```bash
iptables -I INPUT -p icmp -j ACCEPT
```

