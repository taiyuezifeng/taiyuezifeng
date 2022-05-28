# OpenVPN

## 安装`Epel`库

```shell
yum install epel-release
```

## 安装`easyrsa`

```shell
yum install easy-rsa
# 安装路径在/usr/share/easy-rsa/3.0.8/easyrsa
# 生成证书参考下面
```
> **需要注意路径**


## 手动下载安装`EasyRSA`

> 下载地址：Github OpenVPN/EasyRSA

- 解压到`/usr/local/easy-rsa`
- 生成证书
 ```shell
 cd ～/vpn
 /usr/local/easy-rsa/easyrsa init-pki
 /usr/local/easy-rsa/easyrsa build-ca
 # 证书密码：123456
 # 证书Common Name：jadq.com

 ```
 
- 生成交互密钥
  ```shell
  /usr/local/easy-rsa/easyrsa gen-dh
  ```
- 生成服务端密钥
  ```shell
  /usr/local/easy-rsa/easyrsa build-server-full vpn-server
  # PEM密码：123456
  # 验证ca证书密码：123456
  ```
- 生成客户端密钥
  ```shell
  /usr/local/easy-rsa/easyrsa build-client-full vpn-client-5v
  # PEM密码：123456
  # 验证ca证书密码：123456
  ```
- 生成证书交互列表
  ```shell
  /usr/local/easy-rsa/easyrsa gen-crl
  # 验证ca证书密码：123456
  ```
- 如果开启`tls-auth`，则需要生成共享密钥
  ```shell
  openvpn --genkey --secret pki/ta.key
  ```
- 将上述证书放到`OpenVPN`的配置文件夹中或在`conf`中使用绝对路径
  ```
  cp pki/ca.crt /etc/openvpn/ca.crt
  cp pki/dh.pem /etc/openvpn/dh.pem
  cp pki/issued/vpn-server.crt /etc/openvpn/server.crt
  cp pki/private/vpn-server.key /etc/openvpn/server.key
  cp pki/ta.key /etc/openvpn/ta.key
  cp pki/crl.pem /etc/openvpn/crl.pem
  ```

## 配置服务端

```shell
vim /etc/openvpn/server.conf
# 以下为配置文件内容
# Secure OpenVPN Server Config
# Basic Connection Config
dev tun
proto udp
port 1194
keepalive 10 120
max-clients 5
# Certs
ca /etc/openvpn/ca.crt
cert /etc/openvpn/server.crt
key /etc/openvpn/server.key
dh /etc/openvpn/dh.pem
tls-auth /etc/openvpn/ta.key 0
# Ciphers and Hardening
# reneg-sec 0
# remote-cert-tls client
# crl-verify /etc/openvpn/crl.pem
# tls-version-min 1.2
# cipher AES-256-CBC
# auth SHA512
# tls-cipher TLS-DHE-RSA-WITH-AES-256-GCM-SHA384:TLS-DHE-RSA-WITH-AES-256-CBC-SHA256:TLS-DHE-RSA-WITH-AES-128-GCM-SHA256:TLS-DHE-RSA-WITH-AES-128-CBC-SHA256
# Drop Privs
# user nobody
# group nobody
# IP pool
# server 172.31.100.0 255.255.255.0
server 192.168.0.0 255.255.255.0
server 192.168.1.0 255.255.255.0
# topology subnet
ifconfig-pool-persist ipp.txt
# client-config-dir client
# Misc
persist-key
persist-tun
comp-lzo
# DHCP Push options force all traffic through VPN and sets DNS servers
push "redirect-gateway def1 bypass-dhcp"
push "dhcp-option DNS 8.8.8.8"
push "dhcp-option DNS 8.8.4.4"
# Logging
log-append /var/log/openvpn.log
verb 3
```

## 启动服务

```shell
sudo openvpn --config /etc/openvpn/server.conf # 输入证书密码就可以启动了
# sudo openvpn --daemon --config /etc/openvpn/server.conf # 证书不设置密码时可以后台启动
```

## 客户端配置待续