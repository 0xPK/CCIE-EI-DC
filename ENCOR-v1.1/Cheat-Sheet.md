## 1.0 Architecture
* 1.4 Explain the working principles of the Cisco SD-Access solution
    * 1.4.a SD-Access control and data planes elements
        * SD-Access
            * Lisp  --> control plane
            * VXLAN --> data plane
    * 1.4.b Traditional campus interoperating with SD-Access
---
## 2.0 Virtualization
* 2.3 Describe network virtualization concepts
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
    * 2.3.b VXLAN
        * spine-leaf topology
        * IaaS service providers use to extend a Layer 2 segment across a Layer 3 network
            * VXLAN Tunnel Endpoint (VTEP)
                * 
            * VNI (VXLAN Network Indentifier)
            
###### #75 #92 #145 #200 #237 #278 #324 #365 #418 #458 #482 #495 #499 
---
## 3.0 Infrastructure
* 3.1 Layer 2
    * 3.1.b Troubleshoot static and dynamic EtherChannels
* 3.2 Layer 3
    * 3.2.a <font color=ntgreen>Compare</font> routing concepts of *EIGRP* and *OSPF* (advanced distance vector vs. link state, load balancing, path selection, path operations, metrics, and area types)<a name="vs."></a>
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

###### #89 #202 #233 #375 #480 #481 #512 #515 #604 #647

