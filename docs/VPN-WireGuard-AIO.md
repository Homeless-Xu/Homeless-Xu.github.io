---
title: VPN WireGuard+FRP AIO ✔️
layout: home
---



🔵 Summary ✔️

	Wireguard ➜ Powerful  + difficult learn
	frp       ➜ enough    + easy      learn

	So Wireguard for main use. Frp for backup.



🔵 map ✔️

	VPS_Linux:    WWW  ➜ Wireguard Server   + Frp Server

	CHR_Mikrotik: LAN  ➜ Wireguard Client 
	Windows:      LAN  ➜                      Frp Client_01
	HAOS:         LAN  ➜                      Frp Client_02




🔵 CHR + ESX

Download (ESX Choose OVA)
	https://mikrotik.com/download#chr




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵
🔵 WireGuard Config 



VPN. - Wireguard 💯💯💯💯💯💯



1.  wireguard-main   2.frp-backup


WWW: VPS: wireguard_Srv + FRP.Srv
LAN: RB4: Wireguard_Cli + 
LAN: RP4: Frp.Cli 


🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵

🟢 install wg  ✅
🔻 vps: sudo apt-get install wireguard

🔻 ros: build in

🔻 mac: donwload app 



🟢 Gener Key Pair ✅

🔶 vps
用命令生成私钥:   wg genkey 
用私钥生成公钥:   echo "<私钥>" | wg pubkey


root@racknerd-3725a7:~# wg genkey
OMr1eMK9KBTEBV/yNQC3XKeoFUMJX/3tB1yyFEZIuF8=
root@racknerd-3725a7:~# echo "OMr1eMK9KBTEBV/yNQC3XKeoFUMJX/3tB1yyFEZIuF8=" | wg pubkey
naYclosZQgeZmCVNblebhradLz1GTWwqz3ld7STtCXo=


🔶 ros (add new wireguard .not peer)

🔻 create key 

web >> wireguard >> wireguard (not peers)
create new: auto gener public key.

WjSheBEikMFO4oP04/9oY4mQy1Gwssz4rMjIKvcDXXQ=


🔻 check key

[admin@MikroTik] > /interface/wireguard/print 
Flags: X - disabled; R - running 
 0  R name="wireguard1" mtu=1420 listen-port=4455 private-key="sFWQyTEcsAN7zAFIrxOCeChcUsyFWAdLNH1B0pEJPUI=" public-key="YG4jDuVVJXIJaCUeIorz7yho6ldasbuBBl96Lc4OHEI="


🔻 Mac
install wireguard.
create new: auto gen key pair


🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Server config - vps 

🟢 config wg server (vps)

🔶 Create wg config file
vi /etc/wireguard/wg0.conf


# 🔻 Server config 💯
[Interface]
# set wg server ip and mask
Address = 10.0.0.1/24
PrivateKey = OMr1eMK9KBTEBV/yNQC3XKeoFUMJX/3tB1yyFEZIuF8=
ListenPort = 4455



# 🔻 Client Config-iMac 💯
[Peer]
# imac public key
PublicKey = MEZwFsDUjpgcYMGZfw/VToVrgWDsN62M5lRr34CwuVM=
# imac ip
AllowedIPs = 10.0.0.99/24


# # 🔻 Client Config-Router 
[Peer]
PublicKey = YG4jDuVVJXIJaCUeIorz7yho6ldasbuBBl96Lc4OHEI=
AllowedIPs = 10.0.0.2/24






🔶 run wg 
sudo wg-quick up wg0



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 client - imac 💯

[Interface]
PrivateKey = +CHYiqlOl9QWtui+FQ+TPYyn5hYjeq5Q7hEkv3aVD2M=
Address = 10.0.0.99/24

[Peer]
PublicKey = naYclosZQgeZmCVNblebhradLz1GTWwqz3ld7STtCXo=
AllowedIPs = 10.0.0.1/24
Endpoint = 148.135.67.4:4455






🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 client - ros  💯

❗️ ros 必须要给vpn网卡设置ip的! ip 不再wg里面设置. 是ros其他地方设置的❗️
❗️ ros 必须要给vpn网卡设置ip的! ip 不再wg里面设置. 是ros其他地方设置的❗️
❗️ ros 必须要给vpn网卡设置ip的! ip 不再wg里面设置. 是ros其他地方设置的❗️


ros 有两个wg 设置: 一个是 wireguard  一个是peer.
总共三步.

1. ros>wireguard  生成一对密钥 以及一个wg的网卡
2. 去ros > interface > 给新建的wg网卡设置ip
3. ros>peer   配置wg 服务端的信息.

wireguard:  用来生成密钥对而已的!!! 以及一个vpn网卡(但是没有IP 必须另外设置)!!!



🔶 ros peer 

public key:  use vps server public key 
endpoint:    use vps server public ip. 148.135.67.4
endpoint port: 4455
allowed address: 0.0.0.0/0    所有流浪走vpn.
allowed address: 10.0.0.1/24  这样只允许 10.0.0.1/24 网段的设备用vpn(也就是内网穿透). 本地出去不走vpn 的.






🟢 防火墙设置.

🔶 vps - allow 
allow 4455 端口

sudo ufw allow 4455/udp
sudo ufw reload

🔶 vps - check 
sudo ufw status verbose

4455/udp                   ALLOW IN    Anywhere


🔶 本地?
只要设置有公网ip那边的 端口就行.
本地是不用设置的.  
本地发起连接服务器. 只用到服务器的固定端口.
连接成功后. 本地端口是不固定的. 不用设置.
简单说 客户端和服务器. 只要设置服务器的防火墙就行. 


🟢 vps 启动wg 


sudo wg-quick up wg0

🔶 vps wg 状态

VPS wireguard sudo wg
interface: wg0
  public key: oG5dpI7+/mWvXrWJ60SrGnOG+yIfv35w50O6Mww0GSE=
  private key: (hidden)
  listening port: 4455

peer: WjSheBEikMFO4oP04/9oY4mQy1Gwssz4rMjIKvcDXXQ=
  allowed ips: 10.0.0.2/32


🔶


🔶 端口测试 ( 服务器必须先启动服务的. 不然肯定不通啊)

nmap -sU -p 4455 148.135.67.4


🔵 Wireguard. Config .



🔶 vps. install wg 




🔶 vps. create key pair 




🔶 vps. config wg 
/etc/wireguard/wg0.conf


[Interface]
Address = 10.0.0.1/24
PrivateKey = <服务器的私钥>
ListenPort = 51820

[Peer]
PublicKey = <客户端的公钥>
AllowedIPs = 10.0.0.2/32



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 debug 

🟢 vps - debugt 

🔻日志
sudo journalctl | grep wireguard


🔻 卸载重装
sudo apt-get purge wireguard 


🔻 禁止重启. 用docker
sudo systemctl disable wg-quick@wg-vps.service



m8XF1tR2xBwCp35Nn0


🟢 ros 

🔶 WG info

[admin@MikroTik] > /interface/wireguard/print 
Flags: X - disabled; R - running 
 0  R name="wireguard1" mtu=1420 listen-port=4455 private-key="sFWQyTEcsAN7zAFIrxOCeChcUsyFWAdLNH1B0pEJPUI=" public-key="YG4jDuVVJXIJaCUeIorz7yho6ldasbuBBl96Lc4OHEI="



🔶 peer info
[admin@MikroTik] /interface/wireguard/peers> print 
Columns: INTERFACE, PUBLIC-KEY, ENDPOINT-ADDRESS, ENDPOINT-PORT, ALLOWED-ADDRESS
# INTERFACE  PUBLIC-KEY                                    ENDPOINT-ADDRESS  ENDPOINT-PORT  ALLOWED-ADDRESS
0 VPS        oG5dpI7+/mWvXrWJ60SrGnOG+yIfv35w50O6Mww0GSE=  148.135.67.4               4455  10.0.0.1/32






🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 rb4..


# ros-rb4
[Peer]
PublicKey = lYRLcSQCPLGM7V9J2TW64PlU4aG4lv4XIsOvIsyYf2o=
AllowedIPs = 10.0.0.3/24

🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 debug.- 

🔶 cat ping 


tcpdump 



sudo tcpdump -i <interface> icmp
sudo tcpdump -i wg0 icmp




ssh -p 21922 root@148.135.67.4





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 防火墙. 端口转发 💯 


 

