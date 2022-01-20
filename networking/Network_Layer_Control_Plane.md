# Network_Layer_Control_Plane

## Intra-AS Routing in the internet : OSPF(Open Shortest Path First)

### 1. Routing algorithm problems

1. scale
2. Administrative atonomy

### 2. What is AS - Atonomous System

### 3. what is Intra-atonomous system router protocol

Routers within the same AS all run the same routing algorithm and have information about each other. **The routing algorithm running within an autonomous system is called an intra-autonomous system routing protocol**.

### 4. Open Shortest Path First (OSPF)

## Routing Among ISPs: inter-autonomous system routing protocol - BGP(Border Gateway Protocol)

### 1. Border Gateway Protocol

1. **Obtain prefix reachability information from neighboring ASs.**
   In particular, BGP allows each subnet to advertise its existence to the rest of the Internet. A subnet screams, “I exist and I am here,” and BGP makes sure that all the routers in the Internet know about this subnet. If it weren’t for BGP, each subnet would be an isolated island—alone, unknown and unreachable by the rest of the Internet.
2. **Determine the “best” routes to the prefixes.**
   A router may learn about two or more different routes to a specific prefix. To determine the best route, the router will locally run a BGP route-selection procedure (using the prefix reachability information it obtained via neighboring routers. The best route will be determined based on policy as well as the reachability information.
