# CCNP DataLink
OSI Layer 2 (802.3)


      WAN        OSI        IEEE802.3
                    
     x.25      Data Link     LLC (Logic Link Control)
     PPP         Layer       MAC (Media Access Control), Ethernet, Token Bus
     

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

* ARP attack

* ISL, Inter-SW Level Attack & 802.1Q Vlan Atack

* Spanning-Tree Attack

* Multicast Brute Force Attack

* Random Frame Stress Attack

# Mac Flood (Packets Flood), 媒體存取(或稱訪問)控制泛洪水

this is not a properly network attack, but a limitation of the way all SW and Bridges work.

they possess a finite hardware which is learning (listening) table (kind of Data base) to store the src addr (source addresses) of all rcv (received) packets.

however once the table is full, then the hardware can not learn any more src addresses for its desired traffic, then the MAC Flood occurs.

Packet Flood is constrained within the Vlan of origin, so vlan hop is not permitted.

Malicious users can connect to this sw, and turn it into a dumb psudo-hub (Hub), then sniffing all the flooding traffic.

# ARP Cache Poisoning, 欺騙或稱毒化乙太網路位址解析協議

Mac Flood 漏洞的實際攻擊。


      LAN Hosts   <------->   SW   <------->    LAN GW   ---------  Internet
      
                               |
                               
                      LAN Malicious Hacker 
                      
 * Prevention
 
 with Port Security, it prevent from limitstion of MAC addr assigning to a single port.

