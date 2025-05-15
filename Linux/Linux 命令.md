1.  统计 /var/log 目录下所有 .log 文件的行数，并输出到文件

```bash
find /var/log -type f -name "*.log" -exec wc -l {} \; > log_line_counts.txt
```



2. 如何查看文件的最后10行内容

```bash
tail -n 10 文件名
```



3. 获取端口为9999的mysqld进程的CPU和内存使用情况

```bash
ss -tlpn sport = :9999
lsof -i: 9999
```



4.  查找/home目录下所有的.py结尾的文件，删除

```bash
find /home -name "*.py" -delete
```



5. 将文本文件中所有包含hello world的字符串替换成good bye

```bash
sed -i 's/hello world/good bye/g' your_text_file.txt
```



6. 查找最近两天修改过的文件

```bash
find /path/to/search -mtime -2
```



7. 限制进程CPU使用

`groups` `systemd-run --cpu-quota`



8. 如何临时配置网卡 eth0 的 IP 为 192.168.1.100/24，并添加默认网关 192.168.1.1

```bash
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip link set eth0 up
sudo ip route del default  # 删除现有的默认路由（如果存在）
sudo ip route add default via 192.168.1.1 dev eth0
```



9. 设置每天凌晨3点自动备份/home 目录到 /backup，并保留最近7天的备份



10. 如何将本地 80 端口的请求转发到 8080 端口？

```bash
# 1. 针对进入本机（从外部访问或本机非环回地址访问）的流量
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-ports 8080

# 2. 针对本机发往本机环回地址（如 127.0.0.1）的流量
sudo iptables -t nat -A OUTPUT -p tcp --dport 80 -j REDIRECT --to-ports 8080
```



11. 若要求仅允许外部访问 80 端口，其他连接一律拒绝

```bash
# 1. 允许已经建立和相关的连接通过
# 这是非常重要的一步，否则你无法接收到对外连接的响应
sudo iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT

# 2. 允许本地环回接口（lo）的所有流量通过
# 允许本机进程间的通信
sudo iptables -A INPUT -i lo -j ACCEPT

# 3. 允许外部对本机的 TCP 80 端口的访问
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT

# 4. 拒绝 INPUT 链中所有其他不符合前面规则的流量
# 注意：这将拒绝所有其他端口的访问，以及非 TCP 协议的流量（如果它们没有被前面的规则允许）
sudo iptables -P INPUT DROP

# 可选：拒绝 OUTPUT 链中所有未明确允许的流量（如果需要限制本机发出的流量，通常不需要这么严格）
# sudo iptables -P OUTPUT DROP

# 可选：拒绝 FORWARD 链中所有流量（如果你的机器不做路由转发）
# sudo iptables -P FORWARD DROP
```



12. 机器4块盘，如何创建Raid0,并挂载到data1盘符



13. 使用 tcpdump 抓取 eth0 接口的 80 端口流量，并将结果保存到文件，写出完整命令

```bash
sudo tcpdump -i eth0 'tcp port 80' -w /path/to/save/eth0_port80_capture.pcap
```



