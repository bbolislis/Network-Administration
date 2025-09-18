# Static Route Troubleshooting

## Introduction
This guide provides step-by-step instructions for troubleshooting the static routes in the given packet tracer file.

## Topology
![Topology](https://github.com/bbolislis/Network-Administration/blob/main/static-routes/img/topolgy.png)

## Step 1: Verify Connectivity
On PC1 try to ping PC2
`ping 192.168.3.1`
then try to ping PC1 default-gateway
`ping 192.168.1.254`
![Ping Results](https://github.com/bbolislis/Network-Administration/blob/main/static-routes/img/ping_test.png)
Ping results show that PC1 can reach the default gateway but not PC2.

## Step 2: Check Routing Table on R1
Issue the following command on R1 to display the routing table:
`show ip route`
![Routing Table](https://github.com/bbolislis/Network-Administration/blob/main/static-routes/img/R1_ip_route.png)
Our router is configured with a static route to reach the network 192.168.3.0/24 via the next hop 192.168.12.3 which does not exist.
Let's resolve this issue by configuring a new static route on R1.
Remove the existing static route.
`no ip route 192.168.3.0 255.255.255.0 192.168.12.3` or `no ip route 192.168.3.0 255.255.255.0`
Configure a new static route.
`ip route 192.168.3.0 255.255.255.0 192.168.12.2`

## Step 3: Check Routing Table on R2
Issue the following command on R2 to display the routing table:
`show ip route`
![Routing Table](https://github.com/bbolislis/Network-Administration/blob/main/static-routes/img/R2_ip_route.png)
There are two static routes in the routing table. One is the route to the network 192.168.1.0/24 via the next hop 192.168.12.1. The other is the route to the network 192.168.3.0/24 via the exit interface gigabitEthernet0/0, this exit interface is incorrect.
Remove the incorrect static route.
`no ip route 192.168.3.0 255.255.255.0 gigabitEthernet0/0` or `no ip route 192.168.3.0 255.255.255.0`
Configure a new static route using next hop or exit interface.
`ip route 192.168.3.0 255.255.255.0 192.168.13.3` or `ip route 192.168.3.0 255.255.255.0 gigabitEthernet0/1`

## Step 4: Check Routing Table on R3
Issue the following command on R3 to display the routing table:
`show ip route`
![Routing Table](https://github.com/bbolislis/Network-Administration/blob/main/static-routes/img/R3_ip_route.png)
The routing table on R3 shows no static routes and interface gigabitEthernet0/0 is misconfigured with an incorrect IP address. The correct IP address should be 192.168.13.3.
On R3, configure the correct IP address on interface gigabitEthernet0/0.
`interface gigabitEthernet0/0`
`ip address 192.168.13.3 255.255.255.0`
`no shutdown`
`exit`
Issue the following command on R3 to display the routing table:
`show ip route`
![Routing Table](https://github.com/bbolislis/Network-Administration/blob/main/static-routes/img/R3_ip_route2.png)
This time, the routing table on R3 shows a static route to the network 192.168.1.0/24 via the next hop 192.168.13.2.

## Step 5: Verify Connectivity
Issue the following command on PC2 to verify connectivity to PC1:
`ping 192.168.1.1`
![Ping](https://github.com/bbolislis/Network-Administration/blob/main/static-routes/img/ping_test_final.png)
The ping is successful, indicating that the static routes are working correctly.
