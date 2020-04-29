# CCNP DataLink
OSI Layer 2 (802.3)


      WAN        OSI        IEEE802.3
                    
     x.25      Data Link     LLC (Logic Link Control)
     PPP         Layer       MAC (Media Access Control), Ethernet, Token Bus
     
* End-Devices:

![end device](https://scontent.ftpe8-3.fna.fbcdn.net/v/t1.0-9/94640851_4236881559658953_7588762712981635072_o.png?_nc_cat=107&_nc_sid=2d5d41&_nc_ohc=vr82mC0l2DYAX81Hnq2&_nc_ht=scontent.ftpe8-3.fna&oh=cd26a453f7d716e947591fba57465b41&oe=5ECE7915)

WAN 的封裝使用的是 Layer 2 資料連結（又稱資料鏈結）層的協定：
此協定確保實體層連結的資料的正確性，包含資料傳輸的錯誤偵測與錯誤更正功能，幫助 Layer 3 能夠正確地存取實體層資料，可以說是介於實體與邏輯之間的封裝功能。

此協定包含的基本功能：

1. 信號初始化：（要求、指示、回覆、確認）， 藉此判斷資料連結無誤。

2. 資料分段：傳輸時，將資料分割成數小段，稱為 Block 區段抑或是 Frame 訊框，方才發送。 

3. 偵測與更正：由於可能受到外界干擾，使資料於傳輸中，無法百分之百正確，因此連結層需要也錯誤偵測與更正的能力，以保證網路傳輸的品質。

4. 同步位元：資料連結層在資料傳輸中加入 Parity bit 同位位元 (錯誤檢測碼)，藉由同步技術，使資料能正確的在兩端間傳輸。

   備註：
   
   SCSI 匯流排使用同位位元檢測傳輸錯誤，許多微處理器的指令高速緩衝記憶體中也包括同位位元保護。因為指令快取資料是主記憶體資料的副本，所以在發現錯誤的時候能夠拋棄錯誤資料並且重新取回資料。
   
   在序列通訊中，同位位元通常是由 UART 這樣的介面硬體生成、核對的，在接收方，通過介面硬體中的暫存器的狀態位傳給CPU以及作業系統。錯誤資料的恢復通常是通過重新傳送資料，這個過程通常由如作業系統輸入輸出程式這樣的軟體處理的。 

5. 流量控制：為了避免傳輸時，接收端因為傳送端速度太快而發生超載，迫使資料流失，因此必須控制資料的輸入速度即流量，其不可快於接收端接收與處理的速度。

# Attack in Layer 2

* MAC Flood --- p529 --- MACsec

https://github.com/QueenieCplusplus/CCNP_DataLink/blob/master/README.md#mac-flood-packets-flood-媒體存取或稱訪問控制泛洪

* ARP attack

https://github.com/QueenieCplusplus/CCNP_DataLink/blob/master/README.md#arp-cache-poisoning-欺騙或稱毒化乙太網路位址解析協議

* ISL, Inter-SW Level Attack & 802.1Q Vlan Atack

https://github.com/QueenieCplusplus/CCNP_DataLink/blob/master/README.md#8021q--isl-tagging-attck-虛擬區域網路攻擊

* Spanning-Tree Attack

* Multicast Brute Force Attack

https://github.com/QueenieCplusplus/CCNP_DataLink/blob/master/README.md#multicast-brute-force-attack-暴力攻擊或稱窮舉攻擊

* Random Frame Stress Attack

# Mac Flood (Packets Flood), 媒體存取(或稱訪問)控制泛洪

this is not a properly network attack, but a limitation of the way all SW and Bridges work.

they possess a finite hardware which is learning (listening) table (kind of Data base) to store the src addr (source addresses) of all rcv (received) packets.

however once the table is full, then the hardware can not learn any more src addresses for its desired traffic, then the MAC Flood occurs.

Packet Flood is constrained within the Vlan of origin, so vlan hop is not permitted.

Malicious users can connect to this sw, and turn it into a dumb psudo-hub (Hub), then sniffing all the flooding traffic.

# ARP Cache Poisoning, 欺騙或稱毒化乙太網路位址解析協議

Mac Flood 漏洞的實際攻擊。

      DB
      LAN Hosts<------->   SW   <------->    LAN GW   ---------  Internet
      End Stations                           Router
                           |
                               
                    LAN Malicious Hacker 
                      
 * Prevention
 
 with Port Security, it prevent from limitstion of MAC addr assigning to a single port.
 
 other secure way to check the forwarded packets for identify correctness by usign ARP inspection.
 
 # 802.1Q & ISL Tagging Attck, 虛擬區域網路攻擊
 
 ![ISL](https://www.jannet.hk/content/public/upload/vlan-attack/01.png)
 
https://www.jannet.hk/zh-Hant/post/virtual-lan-vlan-attack/

it is a schema allows malicious user to get Unauthorized access to another (attacked) Vlan.

once if a SW were config as DTP, Synamic Trunking Protocol auto on, and were to rcv a fake DTP packet, it might be a trunk port and start to acceot the traffic destined for any Vlan thru the compromised port.

 * Prevention
 
 (1) hardware well-config to off
 
 (2) software upgrade

# Multicast Brute-Force Attack, 暴力攻擊或稱窮舉攻擊

this attack tries to expoit SWs' potentail vulnerabilities, or bugs, against storm of L2 multicast frames. This causes leak frame (訊框或是區段區塊) to other SW.

 * Prevention
 
 the correct behavior shall be to constrain the traffic to its Vlan of origin.





