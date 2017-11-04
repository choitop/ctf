# WIFI Hacking
### Hacking 1:分析附件提供的pcap中有多少可用AP
打开wireless traffic 统计beacons==1的SSID：

![1](https://github.com/choitop/ctf/blob/master/WIFI%20hacking/image/1.PNG)

将其中一个无法显示的SSID用hex转GB2312得到桃花岛训练专用WIFI,经统计有以下不重复SSID：
```shell
 CUC  A101E  CMCC  lczhap  ChinaNet
 CMCC-WEB   CMCC-EDU   and-Business
 CUC-AC-001  a101e-guest  桃花岛训练专用WIFI
```
### Hacking 2:找到WIFI Hacking 1题目附件中所有尝试连接过ssid为a101e-guest的无线客户端MAC地址
在wireless traffic 统计中选择ssid=='a101e-guest'
![2](https://github.com/choitop/ctf/blob/master/WIFI%20hacking/image/2.PNG)
统计不重复的目标MAC地址：
```shell
 e2:08:bd:58:1e:3b  70:14:a6:39:f8:2f  00:1a:a9:c1:fc:35
 56:75:f1:3c:4c:08  38:bc:1a:64:2c:08  6c:25:b9:1f:cb:94
 78:9f:70:03:44:3a  64:9a:be:82:9f:c5  60:f8:1d:87:0e:19
 08:70:45:dd:20:a0  34:23:ba:8b:97:b7
```
### Hacking 3:找到WIFI Hacking 1题目附件中无线客户端尝试连接过但抓包期间信号覆盖范围内不存在的AP的SSID集合
查找有Probe Request的数据包，过滤条件如图

![3](https://github.com/choitop/ctf/blob/master/WIFI%20hacking/image/3-1.PNG)
去掉有过Beacons包的三个AP：CUC,CMCC-WEB和lczhap
剩下的包即为：
```shell
GGBWG-G   sharkAP   Chu-Sushi    Apple Setup
```
### Hacking 4:找到附件中唯一的一个“乱入”的手机，这部手机没有连接上任何一个热点，但暴漏了它曾经连过很多热点。统计这些热点的SSID集合
在wireless traffic中可以看出发出过Request但未收到Response的STA的MAC为：7c:1d:d9:df:a1:0b

![](https://github.com/choitop/ctf/blob/master/WIFI%20hacking/image/4-1.PNG)

过滤有request无response，查找SSID

![](https://github.com/choitop/ctf/blob/master/WIFI%20hacking/image/4-2.PNG)

其中一个SSID经hex2bin转码得到神州专车

![](https://github.com/choitop/ctf/blob/master/WIFI%20hacking/image/4-3.PNG)

统计如下：
```shell
神州专车  KL4   SQBG8701   LieBaoWiFi744
XUE  Feixun_AFE4B3  gg  linan  sxblz  sft4-1
ganluyuan5   sxbl
```
### Hacking 5:找到WIFI Hacking 4题目附件中哪些AP仅支持使用WPA2 CCMP/AES认证加密模式，统计这些热点的MAC地址集合
在RSN字段中选择加密类型AES(CCM）为过滤条件
![](https://github.com/choitop/ctf/blob/master/WIFI%20hacking/image/5.PNG)

统计SSID：
```shell
CMCC   CUC-AC-001  dj   48A102
```
再找到对应的BSSID
```shell
52:1a:a9:c1:fc:37  ac:f1:df:78:c7:67
6c:70:9f:e8:57:90  ec:88:8f:95:ff:12
```
### Hacking 6:找到WIFI Hacking 4题目附件中哪些AP支持WPA/WPA2企业级认证方式？统计这些热点的MAC地址集合
选择RSN版本类型和AKM认证密钥类型,筛选为过滤条件如图

![](https://github.com/choitop/ctf/blob/master/WIFI%20hacking/image/6.PNG)

统计SSID：
```shell
CMCC  a101e-lab
```
再找到对应的BSSID
```shell
52:1a:a9:c1:fc:37  d8:fe:e3:e5:31:18
```
### Hacking 7:找到WIFI Hacking 4题目附件中哪些AP可能是设置了禁止SSID广播？试列举所有的BSSID？并统计这些热点的MAC地址集合

统计Beacons为0但又有Probe Response的SSID，如图

![](https://github.com/choitop/ctf/blob/master/WIFI%20hacking/image/7-1.PNG)
选择dj之后选择过滤条件为response不为0
统计SSID有以下：
```shell
 dj   48-A102
```
再找到对应的BSSID
```shell
6c:70:9f:e8:57:90  ec:88:8f:95:ff:12
```
