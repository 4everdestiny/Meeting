匿名通信与暗网的研究
=================
`凌振 东南大学计算机科学与工程学院`
# 研究背景
表层网  
深网  
暗网  
# 暗网
只能用特殊软件、特殊授权或对电脑特殊设置才能连上的网络
## 关键技术
明文通信 加密通信(TLS/SSL) 匿名通信(Tor) 隐蔽通信(通信本身是否存在保密)
# 匿名通信
多代理匿名通信系统  
circuit 建立 任何一跳无法知道通信的细节  
单代理匿名通信系统 Anonymizer  
Alice <-> SSH tunnel <-> Reverse Proxy/NAT <-> SSH Server <-> Web Proxy <-> Bob  
## 暗网内容
枪支买卖 军火制作 毒品销售 伪造证件 假钞买卖 系统漏洞买卖  
### 匿名通信流量分析与取证
`基于侧信道信息实现匿名加密流量的识别、分析和追踪`  
分析流量的用途  
分析流量的目标  
分析流量的上层的应用
## Tor匿名通信流量的识别
`基于TLS指纹的识别方法`  
### TLS链路建立流程  
建立TLS链接、握手过程为明文
### 识别特征
TLS指纹：密码套件+数字证书
### 基于报文长度分布的识别方法
`基于长度`  
Tor报文长度分布  
512 bytes 的 cell 与 TLS 报文结构  
报文长度分布公式  
典型报文长度  
典型报文长度的出现频率作为识别特征  
## 匿名通信应用类型分析
针对HTTP、FTP、P2P和IM四种类型Tor流量识别
## 匿名通信内容分析
识别用户通过匿名通信系统访问的目标web站点  
Web 站点指纹攻击
### 基本流程  
提取不同的特征形成上下行指纹  
### 基于流水印的匿名通信关系确认
---
# components of Tor
client : the user of the Tor network
## how Tor works -- Circuits
These circuits are dedicated for _Alice_  
Alice can choose the Citcuit to use  
## How Tor works -- Onion Routing
a circuit is built incrementally one hop by one hop  
Onion like encryption  
Alice negotiates an AES key with each other  
Messages are divided into equal sized cells  
Each router knows only its predecessor and successor  
Only _the Exit router_ can know the message  
## Tor Cell Format

Circ_id | Command | Data  
2 | 1 | 509  
Data :  
1 | 2 | 2 | 4 | 2  
Relay command | Recognized | Stream id | Intergrity | Length | Data  
1 Circuit <==> n Stream id  
## Detailed Circuit Setup Steps: One-Hop Circuts
## Two-Hop Circuts
Create C1,E(g^x1)  <==> C1,g^y1,H(K1)
Relay C1,Extend OR2,E(g^x2) <==> C2,E(g^x2)
## Connection Setup Example
Relay C1 \<IP,port\> <==> TCP Handshake  
## Proble Definition of Attacks against of Tor
confirm the communication bettween Alice and Bob  
### Attack Methodology
If the attacker can determine circuit segments C1 and C3 belong to the same circuit, can determine the communication.  
#### AES Counter model - Normal Case
An AES counter is synchronized though the Alice
#### AES Counter - Relay Attack Case
Replayed message causes a decryption error at the end of circuit C3 at Eve 2  
`the duplocated message discrupts the counter`  
#### AES Counter - Deletion Attack Case
The cell after the deleted cell causes decryption error
#### AES Counter - Insert Attack Case
The inserted cell causes decryption error
#### AES Counter - Modify Attack Case
The modified cell causes decryption error
## Issues in Attacks Above
`which cells and when to manipulate`  
`the circuit is torn down`
### time
can't attack in the _circuit_(TCP connection) build process  
Target data cells after the circuit is built  
## How to make Attack Stealthy
init & time default
## Experiment Setup
One computer was setup as an exit router  
the C1 & Clast is solid and test the algroithm
## Impact
Metrics : probability that a circuit chooses malicious Tor routers  
S1:Inject  
S2:Buy the big flow router  
## Protocol-Level Attack vs Brute Force Attack
better
## hard to defend  
I2P : packet head length range
# Cell Counter based Attack
The attacker can manioulate the cell transmission  
3 cells for bit "1"  
1 cell for bit "0"  
## Processing Cells at Onion Router
Empty -> TLS buffer  
Output buffer
## Watermarks
signal to transmit  
watermarks at the exit OR  
Detected watermarks at the entry OR  
^t : Intentional delay interval between the two signal bits
## How to Guarantee the Cell Group
By manipulation the timing of cell  
background : 11 1 -> 1 1->0  
### Detection Rate
Assumption : one way trip time between OR3 and OR1 is approxmated by a lognormal function
### False Positive Rate
Normal Traffic  
False positive rate  
## Detection Schemes
Type 1 : 1 1 1 -> 1  
Type 2 : 111  
Type 3 : 1111
### Scheme 1
### Scheme 2:Hopping Based Encoding
### Summary - Cell Counter based Attack
Needs only tens of cells

# Hidden Service
Directory Servers(DNS)  
Client <==> Hidden Server  
`反向代理?`  
## Protocol-level Hidden Server Discovery
Control several entry onion routers
### Phase 1:Presumably identify the hidden server
### Phase 2:Verify the Hidden server
### Phase 3:Conlude by time correlation
Our entry router is chosen  
the time bettween two packet is short  
### Analysis
### Experiment setup
### Experiment results
True posibilities : 100%
# Summary - Protocol-level Hidden Server Discovery
The hidden service over Tor is a double-edged sword  
Provide the anonymity of Web service  
Host illegal centent  
# TorWard Components
`FireWall`  
`IDS`  
`Transparent proxy`  
`Tor exit router`
# System Setup
Firewall:iptables  
IDS:Suricata  
Transparent proxy:Tor  
Automatic firewall rule management tool  
# Effectiveness of TorWard
100 times -> 1 time choose our network
## Alert Classification
Raised alerts (Dataset 2)  
## Trojan Activity
Malware Android
## Malicious Traffic Statistics
Malicious Traffic Volume  
### Malware Activities
622 C&C server IP address  
DDos attacks  
spam traffic  
Bitcoin Protocol
`Skynet` 
# Tor Issue
## Tor Bridges