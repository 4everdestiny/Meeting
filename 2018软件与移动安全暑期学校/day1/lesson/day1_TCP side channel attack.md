#"软件与移动智能系统安全"暑期学校
`钱志云`  
防火墙 APP hijacking TCP side channels
##哲学
应对有敌意的攻击者  
微博  
Attacks: creative and focused  
Nature of security problems:games  
Man-made roles,just different types of rules
##how to get into security area
`1.`Operating system  
`2.`do a project  
`3.`read books  
##TCP的厄运，网络协议侧信道分析
Vuluerability discovery and exploitation techniques  
Side channels analysis  
代替了堆栈溢出  
Measurement/characterization(cdn 内容分发网络)  
1 day <=> 15days  
One-click root app  
###Real world side channel attacks
狼人杀
###TCP
widely used  
###background
`1.`Man-in-the-middle `or` Off-path attacks  
normal attackers can't  
流量加密  
`2.`Off-path attacks  
packet time ? who first who use  
and a X random bits
###side channel attack
Three-way handshake  
`1.`1985 predictable initial seq num,every hundreds ms add 1  
`2.`2007,Windows attack(hours to finish)
`3.`2018,_unfixable_ side channels:
###DEMO:attack agaginst android apps
###TCP sequence number inference attack:2012 work
target four tuples  
Sequence number
####req1
On-site unprivileged malware  
netstat
####req2-feedback through side channels
judge the sequence number  
firewall-enabled side channels  
the firewall drop the wrong out-of-window sequence number  
how to judge the packet :
`1.`cpu use
`2.`TCP global counters(?)
`3.`33% has the firewall
###without the firewall?
seq too small:Packet counter ++  
seq too large:Packet counter remains  
二分查找:2^32 sequence number  
judge the sequence number in 1 second  
this can Linux,FreeBSD,Apple  
`the more you do , the more information you leak`  
###2016 works:without softwares--Geekpwn 2016
####Yet another side channel
Discovered a subtle TCP side channel vnlnerability in Linux 3.6+  
####global rate limit
sysctl_tcp_challenge_ack_limit  
Default value:100  
_Sproofed SYN_ packets with client's IP and a guessed src port  
judge correct:100 RST 99 challenges  
judge error:100 RST 100 challenges  
####Evaluation
existence of connection:<10 seconds
####Defense
add a random noise  
single challenge not global  
cause:TCP specification RFC5961
###What now?
ALL side channels are _softwares Vuluerability_
###2017 works:
####TCP packets receiving basics
the check process  
conn match:Drop  
Seq #check:Reply  
Ack #check:Drop  
how to judge the server has a reply:Timing channel
####But it becomes visible in wireless!
Root cause:Half-duplex  
In all generations of WiFi and 802.11 technology
####Timing difference - trigger reply
there is a delay  
Local experiment:40->2ms  
Remote experiment:40->5ms (RTT 20ms)  
####what to do next
change a website like a backdoor(web cache poisoning)  
works against all major OSes + browsers:  
Success rate:90%
Time-to-succeed:25s - 600s
##Conclusion
TCP side channel problems are real!  
-Huge impact on network security
-Variety of channels  
-Variety of exploitation scenarios and techniques  
-Difficult to fix at times