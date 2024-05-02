# <font color=ntgreen>1.0 Architecture</font>
### 1.4 Explain the working principles of the Cisco SD-WAN solution
* 1.3.a SD-WAN control and data planes elements
    * redistributed into OMP
        * OSPF/EIGRP
* 1.3.b Benefits and limitations of SD-WAN solutions
### 1.4 Explain the working principles of the Cisco SD-Access solution
* 1.4.a SD-Access control and data planes elements
    * SD-Access
        * Lisp  --> control plane
        * VXLAN --> data plane
		* Management Layer
			* Cisco DNA Center GUI
		* Controller Layer
			* Cisco Network Control Platform (NCP): Control SW, Router
			* Cisco Network Data Platform (NDP):NetFlow,SPAN
			* Cisco Identity Services Engine (ISE)
		* Network Layer
			* Underlay: physical
			* Overlay: virtual(tunneled)
				* Control plane= LISP
				* Data plane= VXLAN
				* Policy plane= Cisco TrustSec				
			* WLC: outside of the fabric
			* Wireless client: Underlay
			* Access points : Overlay
		* Physical Layer
			* Cisco switches, routers, wireless			
		* Node classification
			* Control plane node
				* LISP map server/resolver (MS/MR)
				* SGT mapping
			* Fabric border node
				* LISP proxy tunnel routers (PxTRs)
			* Fabric edge node
				* conect endpoint
				* LISP tunnel router (xTR)
				* assign SGT
				* register endpoint EID
			* Fabric WLAN controller (WLC)
				* control plane: CAPWAP
				* data plane: VXLAN
			* Intermediate nodes
    * Other
		* SD-Access Roaming: inter-xTR
		* SD-Access MTU bytes: 1500 + 50(vxlan) => minimum, 9100 =>recommand
		* edge node <-> extended node: 802.1Q Trunk port
* 1.4.b Traditional campus interoperating with SD-Access
---
# <font color=ntgreen>2.0 Virtualization</font>
### 2.2 Configure and verify data path virtualization technologies
* 2.2.b GRE and IPsec tunneling
    * GRE (Generic Routing Encapsulation)
        * encapsulation and de-encapsulation packet between router
        * GRE tunnels support IPv4 or IPv6
        * bandwidth [1-10000000]: QoS
        * keepalive [seconds[retries]]: default 10 seconds, 3 retry
        * IP Header=(20 Bytes), TCP/UDP Header(20 bytes), GRE Header(24bytes)
        * TCP: Maximum Segment Size (MSS): 1500-20-20-24=1436 bytes
        * IP: Maximum Transmission Unit (MTU) : 1500 bytes
### 2.3 Describe network virtualization concepts
* 2.3.a LISP
    * Endpoint Identifier (EID)
        * Host IP Address
    * Routing Locator (RLOC)
        * ETR IP Address contain host
    * Ingress Tunnel Router (ITR)
    * Egress Tunnel Router (ETR)
    * Map Server (MS)
        * ETR -> Map-Register -> MS
        * ETR <- Map-Notify <- MS
    * Map Resolver (MR)
        * ITR -> Map-Request -> MR
        * ITR <- Map-Reply <- MR
    * Proxy Ingress/Egress Tunnel Router (PxTR)
        * non-LISP -> PITR -> LISP
        * LISP -> PETR -> non-LISP
    ---
* 2.3.b VXLAN (UDP4789)
    * Extend a Layer 2 segment across a Layer 3 network
        * VXLAN Tunnel Endpoint (VTEP)
            * <font color=ntgreen>Encapsulate and De-encapsulate</font> VXLAN Ethernet frames 
        * VNI (VXLAN Network Indentifier) / VNID
            * Valid VNI values are 16 million

![image](https://github.com/0xPK/CCIE-EI-DC/blob/main/ENCOR-v1.1/pics/vxlan%20frame.png)

---
# <font color=ntgreen>3.0 Infrastructure</font>
### 3.1 Layer 2
* 3.1.b Troubleshoot static and dynamic EtherChannels
	* STP operates on a logical link and not on a physical link.
	* Static EtherChannel
		* no health check
		```channel group etherchannel id mode on```
	* PAgP(Cisco)
		* Auto
		* Desirable
		* X: Auto <-> Auto 
		```#non silent keyword requires a port to receive PAgP packets before
		channel group etherchannel id mode auto | desirable } [non-silent]```
		
	* LACP(Open industry standard)
		* Passive
		* Active
		* X: Passive <-> Passive 
		```channel group etherchannel id mode active | passive```
		* lacp rate fast (health check 30's *3 => 1's * 3)
		* lacp max bundle [max links]: max links that are forwarding traffic, remaining links remain in hot-standby mode.
	* show status
		* show etherchannel summary
		* show etherchannel port
		* show pagp neighbor
		* show lacp neighbor
* 3.1.c Configure and verify common Spanning Tree Protocols (RSTP, MST) and Spanning Tree enhancements such as root guard and BPDU guard
     * PVST+
     * MST
     * root guard
     * BPDU guard
### 3.2 Layer 3
* 3.2.a <font color=ntgreen>Compare</font> routing concepts of EIGRP and OSPF (advanced distance vector vs. link state, load balancing, path selection, path operations, metrics, and area types)<a name="vs."></a> 
```go
  | OSPF(Shotest Path First)  | EIGRP(Ehance Interior Gateway Route)                |
  |:----------------------    | ---------------------------------                   |
  | Supports any Vendor       | Cisco only (Autonomous system(AS) number)           |
  | ----------------          | -----------------------                             |
  | Dijkstra(SPF) Algo        | DUAL(Diffusing Update) Algo                         |
  |   Network Map             |   Only Best and backup route                        |
  |                           |     (Fast Convergence than SPF Algo)                |
  | ----------------          | ---------------------                               |
  | *Supports virtual links   | x                                                   |
  | ----------------          | ----------------------                              |
  | Cost-based Metric         | 256 * { K1*BW + [(K2*BW)/(256-load)] + (K3*delay) } |
  | (BandWidth only)          |     * { K5/(reliability+K4) } in Metric             |
  | ----------------          | ---------------------------------                   |
  | DR/DBR Election           | x                                                   |
  | ----------------          | ---------------------------------                   |
  | Link-state                | Distance vectory                                    |
  | Default Distance 110      |                                                     |
  | ----------------          | ---------------------------------                   |
  | Default Hello 10 sec      | Default Hello 5 sec                                 |
  | Dead 40 sec               | Hold 15 sec                                         |
  | ----------------          | ---------------------------------                   |
  | *Equal-cost Load Balance  | Variance(Unequal-costLoad Balance)                  |
```
* 3.2.b Configure simple OSPFv2/v3 environments, including multiple normal areas, summarization, and filtering (neighbor adjacency, point-to-point, and broadcast network types, and passive-interface)
    * Config
        * Enable OSPF on network interfaces matching a specified network range for a specific OSPF area
            * network ip-address wildcard-mask area area-id
        * Enable OSPF on an explicit specific network interface for a specific OSPF area 
            * ip ospf process-id area area-id
    * Point-to-point
        * Only support 2 device to communication use ospf
        * Statically configure an interface as a point to-point OSPF network type ip ospf network point-to-point
    * passive-interface
        * Configure a specific interface as passive passive interface-id 
        * Configure all interfaces as passive passive interface default
* 3.2.c Configure and verify eBGP between directly connected neighbors (best path selection algorithm and neighbor relationships)
### 3.3 Wireless
* 3.3.c Describe access point discovery and join process (discovery algorithms, WLC selection process)
    * CAPWAP
        * Discover WLC: 1.broadcasts or 2.send UDP-5246 to WLC IP
        * Get WLC Address: DHCP option 43
        * Domain: CISCO-CAPWAP-CONTROLLER.localdomain
### 3.4 IP Services
* 3.4.a Interpret network time protocol configurations such as NTP and PTP
* 3.4.b Configure NAT/PAT 
    * Static NAT
        * (config)# ip nat inside source static [Inside local add] [Indise Golobal add]
        * extendable
        * no-alias
            *    
        * https://networklessons.com/uncategorized/nat-extendable-on-cisco-ios
    * Dynamic NAT
        * (config)# ip nat inside source list [number] pool [pool-name]
        * (config)# access-list [number] permit [Inside local add] [wildmask]
        * (config)# ip nat pool [pool-name] [add_start] [add_end] [netmask]
    * PAT
        * =<font color=ntgreen>NAT Overload</font>
        * (config)# ip nat inside source list [number] pool [pool-name] overload
        * (config)# access-list [number] permit [Inside local add] [wildmask]
        * (config)# ip nat pool [pool-name] [add_start] [add_end] [netmask]
* 3.4.c Configure first hop redundancy protocols, such as HSRP, VRRP
* 3.4.d Describe multicast protocols, such as RPF check, PIM and IGMP v2/v3
---
# <font color=ntgreen>4.0 Network Assurance</font>
### 4.5 Describe Cisco DNA Center workflows to apply network configuration, monitoring, and management
### 4.3 Configure SPAN/RSPAN/ERSPAN
* Switched Port Analyzer (SPAN) => Local
   * Catalyst(config)#monitor session 1 source interface f0/1 - 2
   * Catalyst(config)#monitor session 1 destination interface f0/3
* Remote SPAN (RSPAN) => send SPAN traffic to a VLAN
   * SW1(config)# vlan 999
   * SW1(config-vlan)# remote-span
   * SW1(config)# monitor session 1 source interface FastEthernet 0/10
   * SW1(config)# monitor session 1 destination remote vlan 999
* Encapsulated Remote SPAN (ERSPAN) => send SPAN traffic to a IP
   * erspan-id must be equal.
   * sender's dst IP equal to reciver's src IP.
--- 
   * R1(config-mon-erspan-src)#destination
   * R1(config-mon-erspan-src-dst)#erspan-id 100
   * R1(config-mon-erspan-src-dst)#ip address 172.16.2.200
   * R1(config-mon-erspan-src-dst)#origin ip address 172.16.12.1
---
   * R2(config-mon-erspan-dst)#source
   * R2(config-mon-erspan-dst-src)#erspan-id 100
   * R2(config-mon-erspan-dst-src)#ip address 172.16.2.200

---
# <font color=ntgreen>6.0 Automation</font>
### 6.4 Describe APIs for Cisco DNA Center and vManage
* Cisco DNA Center API (network automation)
    * Northbound
        * Intent APIs(Cisco's API name) to simplify API usage
        * based on RESTful API
        * controller -> DNA management software
    * Southbound
        * Multivendor Support APIs/SDK
        * controller(push config) -> devices(routers, switches)
---
## Mapping Table
* 1.4.a =#82 #88 #92 #120 #131 #188 #206 #222 #228 #237 #241 #267 #278 #283 #321 #336 #344 #362 #388 #399 #407 #411 #425 #439 #445 #458 #543 #608 #648 #1 #51
* 2.2.b
   * GRE= #79 #86 #179 #238 #264 #273 #286 #287 #309 #317
* 2.3.b= #75 #92 #145 #200 #237 #278 #324 #365 #418 #458 #495 #499
* 3.1.c
   * PVST+= #408 #413
   * portfast= #463
   * BPDU guard= #270 #147 #175 #279 #386 #487
* 3.2.a= #89 #202 #233 #375 #480 #481 #512 #515 #604 #647
* 3.2.b= <font color=ntgreen>#294</font> #494 <font color=red>#548</font> <font color=red>#591</font> #597
    * Point-to-point #621
    * Passive-int #314 #631 #642
* 3.2.c= #490=634 #536 #548 #580 #582 #588 #613 #617 
* 3.3.c= #6 #71 #78 #129 #216 #268 #302 #464
* 3.4.b= #66 #132 #186 #223 #370 #384 #484 #489 #605
* 4.3 = #80 #97 #130 #160 #168 #211 #334 #417 #530 #540
* 6.4 = #77 #91 #127 #144 #183 #225 #307 #379 #406 #436 #471 #468 #513 #518 #569
