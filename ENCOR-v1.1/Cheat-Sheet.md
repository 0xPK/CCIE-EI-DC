# <font color=ntgreen>1.0 Architecture</font>
### 1.3 Explain the working principles of the Cisco SD-WAN solution
* 1.3.a SD-WAN control and data planes elements
    * redistributed into OMP
        * OSPF/EIGRP
* 1.3.b Benefits and limitations of SD-WAN solutions
	* vManage: 
		* GUI management
		* software upgrades
	* vSmart Controller: 
		* control plane
		* distribute policies
		* authenticate SD-WAN router
		* DTLS tunnel to SD-WAN router =>  Overlay Management Protocol (OMP) 
	* Cisco SD-WAN Routers (vEdge and cEdge)
		* vEdge: The original Viptela platforms running Viptela software.
		* cEdge: Viptela software integrated with Cisco IOS-XE. 
	* vBond Orchestrator: 
		* authenticates: vSmart Controller, SD-WAN Routers
		* onboarding of SDWAN routers
	
	* control plane connection -> DTLS tunnel ->  vSmart controller
	* NAT traversal: behind NAT devices
	* load balancing: SD-WAN routers across the vSmart controllers when routers come online.
	
	* vAnalytics: assurance,  analytics
	* other:
		* Cloud OnRamp : WAN Cloud(SD-WAN router, sending small HTTP probes, find best path)
		* monitor protocal: Bi-directional Forwarding Detection (BFD) 
		* segmentation: VPN
		* tunnel= DTLS or TLS
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
### 1.5 Interpret wired and wireless QoS configurations
* 1.5.a QoS components
    * Marking in packet
        * Layer3: ToS/DiffServ -> Differentiated Services Code Point(DSCP)
        * Layer2.5: MPLS Experimental(EXP) bits
        * Layer2: CoS

![QoS2](https://hackmd.io/_uploads/SkYb54BGA.png)

* 1.5.b QoS policy


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
* EtherChannel Bundle
	* STP operates on a logical link and not on a physical link.
	* Static EtherChannel
		* no health check
		* ```channel group etherchannel id mode on```
	* PAgP(Cisco)
		* Auto
		* Desirable
		* X: Auto <-> Auto 
		* non silent keyword requires a port to receive PAgP packets before
		* ```channel group etherchannel id mode auto | desirable } [non-silent]```
	* LACP(Open industry standard)
		* Passive
		* Active
		* X: Passive <-> Passive 
		* ```channel group etherchannel id mode active | passive```
		* lacp rate fast (health check 30's *3 => 1's * 3)
		* lacp max bundle [max links]: max links that are forwarding traffic, remaining links remain in hot-standby mode.
	* show status
		* show etherchannel summary
		* show etherchannel port
		* show pagp neighbor
		* show lacp neighbor	
	#147 #175 = 1.關閉其中一個port再設定EtherChannel，再啟用port，讓bpdu封包走在邏輯介面(EtherChannel)上。2.關閉bpdu
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
* 3.2.b Configure simple <span style="font-size:20px; color:red">OSPFv2/v3</span> environments, including multiple normal areas, summarization, and filtering (neighbor adjacency, point-to-point, and broadcast network types, and passive-interface)
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
* 3.2.c Configure and verify <span style="font-size:20px; color:red">eBGP</span> between directly connected neighbors (best path selection algorithm and neighbor relationships)
	* BGP Packet Types
		* 1 OPEN:		Sets up and establishes BGP adjacency
		* 2 UPDATE:		Advertises, updates, or withdraws routes
		* 3 NOTIFICATION:	Indicates an error condition to a BGP neighbor
		* 4 KEEPALIVE:		Ensures that BGP neighbors are still alive
  	* BGP Neighbor States:
		* Idle:		initiate a TCP session
		* Connect:	TCP handshake is complete
		* Active:	TCP handshake again
		* OpenSent:	sent OPEN
		* OpenConfirm:	Confirm OPEN
		* Established
	* best-path algorithm = weight, local preference, AS path, MED
 	* bgp default local-preference: AS出入口
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
### 4.5 Describe Cisco DNA Center workflows to apply network configuration, monitoring, and management
# <font color=ntgreen>5.0 Security</font>
### 5.1 Configure and verify device access control
* 5.1.a Lines and local user authentication
* 5.1.b Authentication and authorization using AAA
    * aaa authentication default group radius 
        * only aaa auth
    * aaa authentication login default group radius [local/none] 
    * <--> aaa authentication login default local group [radius/tacacs+]
        * local: first radius/tacacs+, next local
        * none: when radius auth fail, no auth any command
    * aaa authentication login default group ISE-servers local enable
        * when no usernames are defined in the configuration then the <font color=ntgreen>enable password</font> must be the last resort to log in
    * aaa authorization exec default group radius if-authenticated
        * when the user has successfully authenticated 
### 5.5 Describe the components of network security design
* 5.5.a Threat defense
    * Solution: Secure Network Analytics <-> Stealwatch
        * collects <font color=red>telemetry</font> from every part of the network and applies advanced security analytics to the data
            * Telemetry: real-time flows of network traffic, user activities, device statuses, and other information
* 5.5.b Endpoint security
* 5.5.c Next-generation firewall
    * Secure Firewall <-> Firepowre <-> ASA 
* 5.5.d TrustSec and MACsec
* 5.5.e Network access control with 802.1X, MAB, and WebAuth
    * EAP
        * Out(Tunnel): 
        * In:
    * EAP-FAST
        * Cisco only 
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
* 1.3: #87 #123 #140 #152 #229 #234 #297 #328 #335 #382 #393 401 473 523 527 560 565 566 589
* 1.4.a: #1 #51 #82 #88 #92 #120 #130 #188 #206 #222 #228 #237 #241 #267 #278 #283 #321 #336 #344 #362 #388 #399 #407 #411 #425 #439 #445 #458 #534 #608 #648
* 1.5.a: #596 #643
* 2.2.b
   * GRE: #79 #86 #179 #238 #264 #273 #286 #287 #309 #317
* 2.3.b: #75 #92 #145 #200 #324 #365 #418 #495 #499
* 3.1.b: #17 #37 #83 #110 #147 #175 #479 #626 #224 #318 #358 #447
* 3.1.c
   * PVST+: #408 #413
   * portfast: #463
   * BPDU guard: #270 #147 #175 #279 #386 #487
* 3.2.a: #89 #202 #233 #375 #480 #481 #512 #515 #604 #647
* 3.2.b: <font color=ntgreen>#294</font> #494 <font color=red>#548</font> <font color=red>#591</font> #597
    * Point-to-point: #621
    * Passive-int: #314 #631 #642
* 3.2.c(BGP): #490=634 <font color=red>#502</font> #536 #548 #580 #582 #588 #613 #617 
* 3.3.c: #6 #71 #78 #129 #216 #268 #302 #464
* 3.4.b: #66 #132 #186 #223 #370 #384 #484 #489 #605
* 4.3: #80 #97 #130 #160 #168 #211 #334 #417 #530 #540
* 5.1.b(AAA): #139 #155 #341 #426 #442 <font color=red>#501</font> #594 #612 #629 #641 #700
* 5.5.a(TD): #133 #192 #214 #337 #571 #576
* 5.5.b(NAC): #652
* 5.5.c(FW): #8 #409 #419 #559 #630
* 5.5.e
    * EAP: #76 #504 #747
* 6.4: #77 #91 #127 #144 #183 #225 #307 #379 #406 #436 #471 #468 #513 #518 #569
