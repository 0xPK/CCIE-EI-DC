## <font color=ntgreen>1.0 Architecture</font>
* ### 1.4 Explain the working principles of the Cisco SD-Access solution
    * 1.4.a SD-Access control and data planes elements
        * SD-Access
            * Lisp  --> control plane
            * VXLAN --> data plane
    * 1.4.b Traditional campus interoperating with SD-Access
---
## <font color=ntgreen>2.0 Virtualization</font>
* ### 2.3 Describe network virtualization concepts
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
## <font color=ntgreen>3.0 Infrastructure</font>
* ### 3.1 Layer 2
    * 3.1.b Troubleshoot static and dynamic EtherChannels
* ### 3.2 Layer 3
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
* ### 3.4 IP Services
    * 3.4.a Interpret network time protocol configurations such as NTP and PTP
    * 3.4.b Configure NAT/PAT 
        * NAT
            * DNAT/SNAT
        * PAT
            * =<font color=ntgreen>NAT Overload</font>          
    * 3.4.c Configure first hop redundancy protocols, such as HSRP, VRRP
    * 3.4.d Describe multicast protocols, such as RPF check, PIM and IGMP v2/v3
---
## <font color=ntgreen>4.0 Network Assurance</font>
* ### 4.5 Describe Cisco DNA Center workflows to apply network configuration, monitoring, and management
---
## <font color=ntgreen>6.0 Automation</font>
* ### 6.4 Describe APIs for Cisco DNA Center and vManage
   * Cisco DNA Center API (network automation)
      * Northbound
        * RESTful API
        * controller -> DNA management software
    * Southbound
        * controller(push config) -> devices(routers, switches)
#77 #91
---
## Mapping Table
* 2.3.b= #75 #92 #145 #200 #237 #278 #324 #365 #418 #458 #495 #499 
* 3.2.a= #89 #202 #233 #375 #480 #481 #512 #515 #604 #647
* 3.4.b= #66 #132 #186 #223 #370 #384 #484 #489 #605
