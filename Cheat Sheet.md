[![hackmd-github-sync-badge](https://hackmd.io/8wTmHDA2THi5UU6w-ZlwKA/badge)](https://hackmd.io/8wTmHDA2THi5UU6w-ZlwKA)
## 2.0 Virtualization
* 2.3 Describe network virtualization concepts
    * 2.3.a LISP


## 3.0 Infrastructure
* 3.1 Layer 2
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

