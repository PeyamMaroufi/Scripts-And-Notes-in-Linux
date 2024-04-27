# Network Connection
### Creating virtual network
Load the dummy module
```bash
$ sudo modprobe dummy
```
Create a dummy device
```bash
$ sudo ip link add eth0 type dummy
```
#### Creating VLAN on a device
Let first see what devices we have:
```bash
$ sudo ip link show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: enp0s31f6: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether f8:b4:6a:ba:4c:37 brd ff:ff:ff:ff:ff:ff
3: enxf8b46a9f883c: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether f8:b4:6a:9f:88:3c brd ff:ff:ff:ff:ff:ff
4: wlp112s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DORMANT group default qlen 1000
    link/ether 3c:f0:11:20:a7:c4 brd ff:ff:ff:ff:ff:ff
5: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default 
    link/ether 02:42:e6:12:39:4f brd ff:ff:ff:ff:ff:ff
```
Let's create a vlan on the `wlp112s0`.
```bash
$ sudo ip link add link wlp112s0 name wlp112s0.700 type vlan id 700
```
Now let's see if that is added.
```bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: enp0s31f6: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether f8:b4:6a:ba:4c:37 brd ff:ff:ff:ff:ff:ff
3: enxf8b46a9f883c: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether f8:b4:6a:9f:88:3c brd ff:ff:ff:ff:ff:ff
4: wlp112s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DORMANT group default qlen 1000
    link/ether 3c:f0:11:20:a7:c4 brd ff:ff:ff:ff:ff:ff
5: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default
    link/ether 02:42:e6:12:39:4f brd ff:ff:ff:ff:ff:ff
6: wlp112s0.700@wlp112s0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether 3c:f0:11:20:a7:c4 brd ff:ff:ff:ff:ff:ff
```
Number 6 is the newly created virtual lan. Let's adda a IPv4 addres.
```bash
$ sudo ip addr add 10.12.200.2/9 dev wlp112s0.700
```
Now bring VLAN up
```bash
$ sudo ip link set dev wlp112s0.700
```
