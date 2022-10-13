---
title: 閘道與路由的基礎介紹
date: 2022-10-13 16:50:21
tags:
categories:
---

閘道（gateway）與路由（router）在網絡早期的世界裡並無區別，且與 TCP/IP 通訊層有密不可分的關係。
在現今發達的網絡世界中逐漸分化了兩者的功能：

# 閘道（gateway）
A Gateway, on the other hand, joins dissimilar systems.
Gateway it is defined as a network entity that allows a network to interface with another network with different protocols.
閘道（gateway）能合併不同的系統，旨在允許不同協定（protocols）的網絡之間互相介接的網路裝置。

Device that converts one protocol or format to another. 
A network gateway converts packets from one protocol to another. 
The gateway functions as an entry/exit point to the network.
將一種網路協定規格或封包轉換成另一種的裝置。
閘道功能當作是進入網絡的節點與進入點。

### 透過運算轉化規格
The gateway interprets the system of networks as endpoints from packet to packet.
閘道（gateway）的主要任務就是透過運算與轉換將一種協定（protocols）的網絡轉換成另一種，使資料在不同協定之間傳遞不受阻。

A gateway can only operate on five layers.
閘道僅能在 OSI 模型的應用層第五層運作。

##### 附加的特色
network access control, protocol conversion, etc


# 路由（router）
A router is a device that is capable of sending and receiving data packets between computer networks, also creating an overlay network.
路由（router）是一個能在電腦網絡之間傳遞與接收資料封包的裝置，亦可創造覆蓋網路（overlay network）。

Based on internal routing tables, routers read each incoming packet and decide how to forward it.
根據內部的路由表格，讀取每個進入的封包並決定如何繼續傳遞。

Routers work at the network layer (layer 3) of the protocol
路由主要在 OSI 模型的應用層第三層或第四層運作。

##### 附加的特色
wireless networking, static routing, NAT, DHCP server, etc

### 可選擇傳遞的路徑（ip）
路由可以在網路（network）之間互相分享與傳遞資料封包，也可以選擇傳遞的路徑。

{% img /images/difference-between-gateway-and-router/1.png 800 200 difference-between-gateway-and-router %}

# 兩者如何共同工作
### 閘道（gateway）關卡
Before arriving at the router, packets go to the gateway channel first, and the gateway checks the header information at once. 
在資料抵達路由以前會間在閘道頻道上檢查資訊頭（the header information）。

After checking for any kind of error in the destination IP address and packet.
According to the needs of the destination network,it carries out data conversion and protocol conversion on the packet,
which is also the most critical step.
檢查完畢目的地的 IP 位址與數據包無任何異常後，閘道會攜出轉換協定（protocols）的封包，這中間經歷的步驟最為嚴謹。


### 路由（router）關卡
Finally, the processed packet is forwarded to the router to establish intelligent communication between the two different networks.
最後當封包傳遞到路由上後，會建立兩個不同網路（networks）的智能通訊。

The router extracts the destination address from the received packet, determines the network number in the address,
and then looks up the routing table to find the entry matching the destination network.
路由從封包中提取目的地的位址並且決定位址中的網絡號碼，查看路由表格對應的目的地網路進入點。

The routing table determines the next stop, destination, output interface, and other routing-related parameters that match the current packet.
路由表格會查看符合當前封包的路由相關參數，以決定好封包的下一站、目的地與傳輸出去的介面。

Finally, the packet is sent to the computer with the best route.
最後封包被傳遞到電腦中最適宜的路由道路上。

# 閘道就是大門，路由就是樓層間的電梯
In layman's terms, gateways and routers are like the gates and elevators of a castle.
口語化來說，閘道就是大門，路由就是樓層間的電梯。

they must first pass the inspection and arrangement through the gate.
必須經過檢查與安排才能透過大門。

Then the elevator headquarters will choose the fastest elevator for them according to the characteristics and purpose of these strangers and finally,
arrive at the destination.
電梯總部人員根據性質與訴求選擇最適合最快的電梯協助抵達目的地。

# 選擇閘道還是路由
If your devices are on the same network, then routers and gateways can be peer to peer.
若你的多個裝置是同個網路，則路於與閘道可以進行對等式網路 peer to peer（P2P）

But if you want to connect two different networks with different protocols, the gateway can suit your demands.
若你想連接兩個不同協定的不同網絡，閘道可以符合需求。

Some routers and gateways in the market can already replace each other.
有些市面上的閘道與路由已經可以互相取代了。

The technology and capabilities of gateways and routers will continue to evolve and become more sophisticated over the next few years.
往後的幾年兩者的技術與可能性會互相演變得更細分精緻。