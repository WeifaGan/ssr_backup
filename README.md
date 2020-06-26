**1.安装SSR客户端**

yum -y install git 

git clone https://github.com/WeifaGan/ssr_backup.git

```python
cd ssr_backup/ mv ssr /usr/local/bin chmod +x /usr/local/bin/ssr ssr install 
```

**2.配置SSR客户端**

参数配置 

```python
 ssr config 
```

```python
#server、server_port、password、method、protocol、obfs  和obfs_param需要从服务商那边获取 
{ 

 "server": "103.116.123.123", # ssr服务器IPV4地址 

 "server_ipv6": "::",       # ssr服务器IPV6地址  

"server_port": 12345,      # ssr服务器端口  

"local_address": "192.168.8.168",  # 本地监听地址，默认127.0.0.1  

"local_port": 1080,    # 本地监听端口，默认1080  

"password": "123456",  # 密码  

"method": "aes-256-cfb", # 加密方式  

"protocol": "auth_sha1_v4", # 协议参数

"protocol_param": "",   # 协议  

"obfs": "tls1.2_ticket_auth",# 混淆  

"obfs_param": "",  # 混淆参数  

"speed_limit_per_con": 0,  

"speed_limit_per_user": 0,  

"additional_ports" : {}, // only works under multi-user mode 

"additional_ports_only" : false, // only works under multi-user mode  

"timeout": 120,  

"udp_timeout": 60,  

"dns_ipv6": false,  

"connect_verbose_info": 0,  

"redirect": "",  "fast_open": false 

 } 
```

**3.转换HTTP代理** 

Shadowsocks默认是用Socks5协议的，对于Terminal的get,wget等走http协议的地方是无能为力的，所以需要转换成http代理，加强通用性，这里使用的转换方法是基于Polipo的。 

**3.1安装Polipo**

```python
sudo apt-get install polipo      
```

**3.2 修改配置文件**

```python
sudo gedit /etc/polipo/config    
```

将下面的内容整个替换到文件中并保存： 

```python
# This file only needs to list configuration variables that deviate
# from the default values. See /usr/share/doc/polipo/examples/config.sample
# and "polipo -v" for variables you can tweak and further information.
logSyslog = false
logFile = "/var/log/polipo/polipo.log"

socksParentProxy = "127.0.0.1:1080"
socksProxyType = socks5

chunkHighMark = 50331648
objectHighMark = 16384

serverMaxSlots = 64
serverSlots = 16
serverSlots1 = 32

proxyAddress = "0.0.0.0"
proxyPort = 8123
```

 **3.2 重启Polipo:**

```python
/etc/init.d/polipo restart 
```

验证代理是否正常工作：如果正常，就会返回抓取到的Google网页内容。此时，终端里面可以访问外网了。 

export http_proxy="http://127.0.0.1:8123/" 

curl www.google.com 

**4.配置浏览器 修改全局网络** 

打开设置->网络->网络代理： 把网络设置为手动 所有代理和主机设置为：127.0.0.1，端口设置为：8123 

**5.SSR客户端状态管理**

**5.1启动**

```python
ssr start 
```

**5.2 停止**

```python
ssr stop 
```

**5.3 重启**

```python
ssr restart 
```

**5.4 卸载**

```python
ssr uninstall
```

