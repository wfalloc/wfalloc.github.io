
#### CentOS 7 防火墙设置

```bash
// 查看防火墙状态
firewall-cmd --state

// 检查防火墙规则
firewall-cmd --list-all

// 查看端口是否开放
firewall-cmd --query-port=80/tcp

// 开启端口
sudo firewall-cmd --zone=public --add-port=3000/tcp --permanent

// 开启端口后需重新加载防火墙
firewall-cmd --reload

// Disable firewall
systemctl disable firewalld
systemctl stop firewalld
systemctl status firewalld
// Enable firewall
systemctl enable firewalld // 开机自启
systemctl start firewalld
systemctl status firewalld
```

#### Linux 查看服务器端口开放状态

```bash
// 本机查看端口是否占用
lsof -i:3306

// 查看远程服务器端口是否启用
telnet 192.168.1.102 80

// 连接超时，端口没开
Trying 192.168.1.102...
telnet: connect to address 192.168.1.102: Connection timed out

// 端口没开 服务器端口即使处于监听状态，但是防火墙iptables屏蔽了该端口，是无法通过该方法检测端口是否开放的
Trying 192.168.1.102...
telnet: connect to address 192.168.1.102: No route to host

// 连接到此端口，此端口目前开放且有应用占用；
Trying 192.168.1.102...
Connected to 192.168.1.102.
Escape character is '^]'.
Connection closed by foreign host.

// 连接被拒绝；此端口开放，暂无应用占用此端口；
Trying 192.168.1.102...
telnet: connect to address 192.168.1.102: Connection refused
```

---

date: 2020-06-04 13:00:00