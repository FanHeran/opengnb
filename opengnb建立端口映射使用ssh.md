## opengnb 建立端口映射

服务端配置

```bash
# 场景：服务器端22端口开一个ssh，客户端通过gnb隧道访问远程服务器ssh
# 服务器（winsows）：10.1.0.1
# 客户端（winsows）：10.1.0.2
# -p passcode 是一个长度为8个字符的32bit的16进制字符串，尽量复杂，避免与 public index其他用户相同
# gnb lite模式启动
gnb -n 1001 -I "101.32.178.3/9001" --multi-socket=on -p 12345678

# 使用netsh端口映射
# 开启
netsh interface portproxy add v4tov4 listenaddress=10.1.0.1 listenport=22 connectaddress=127.0.0.1 connectport=22

# ！！注意设置防火墙入站规则，程序所有，本地端口22，远程端口所有！！

```

客户端配置

```bash
gnb -n 1002 -I "101.32.178.3/9001" --multi-socket=on -p 12345678
# 等待5分钟左右建立Idex，之后p2p互通，可互相ping通
ping 10.1.0.1
# ping通后ssh可用
ssh uer@10.1.0.1
```

结束端口映射

 ```bash
 # 结束
 netsh interface portproxy delete v4tov4 listenaddress=10.1.0.1 listenport=22
 
 ```

netsh参数说明

```bash
# 查看映射
netsh interface portproxy show all

netsh interface portproxy 表示端口映射列表

add v4tov4 表示添加的是IPV4到IPV4的端口

listenaddress 表示侦听的ip地址，填的是映射方

listenport 侦听的端口，可以与被映射的端口设置成不一样

connectaddress 被映射方（连接方）的ip地址

connectport 被映射方的端口
```







