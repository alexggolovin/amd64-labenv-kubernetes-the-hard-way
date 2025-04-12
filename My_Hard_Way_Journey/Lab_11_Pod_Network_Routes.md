# Provisioning Pod Network Routes

Pods scheduled to a node receive an IP address from the node's Pod CIDR range. At this point pods can not communicate with other pods running on different nodes due to missing network [routes](https://cloud.google.com/compute/docs/vpc/routes).

In this lab you will create a route for each worker node that maps the node's Pod CIDR range to the node's internal IP address.

> There are [other ways](https://kubernetes.io/docs/concepts/cluster-administration/networking/#how-to-achieve-this) to implement the Kubernetes networking model.

```bash
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# date
Sat Apr 12 05:56:40 UTC 2025
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# {
  SERVER_IP=$(grep server machines.txt | cut -d " " -f 1)
  NODE_0_IP=$(grep node-0 machines.txt | cut -d " " -f 1)
  NODE_0_SUBNET=$(grep node-0 machines.txt | cut -d " " -f 4)
  NODE_1_IP=$(grep node-1 machines.txt | cut -d " " -f 1)
  NODE_1_SUBNET=$(grep node-1 machines.txt | cut -d " " -f 4)
}
root@jumpbox:~/kubernetes-the-hard-way# echo $SERVER_IP
192.168.56.10
root@jumpbox:~/kubernetes-the-hard-way# ssh root@server <<EOF
  ip route add ${NODE_0_SUBNET} via ${NODE_0_IP}
  ip route add ${NODE_1_SUBNET} via ${NODE_1_IP}
EOF
Pseudo-terminal will not be allocated because stdin is not a terminal.
Linux server 6.1.0-29-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.123-1 (2025-01-02) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
root@jumpbox:~/kubernetes-the-hard-way# ssh root@node-0 <<EOF
  ip route add ${NODE_1_SUBNET} via ${NODE_1_IP}
EOF
Pseudo-terminal will not be allocated because stdin is not a terminal.
Linux node-0 6.1.0-29-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.123-1 (2025-01-02) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
root@jumpbox:~/kubernetes-the-hard-way# ip r
default via 10.0.2.2 dev eth0 
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 
192.168.56.0/24 dev eth1 proto kernel scope link src 192.168.56.13 
root@jumpbox:~/kubernetes-the-hard-way# ssh root@server <<EOF
> ip r
> EOF
Pseudo-terminal will not be allocated because stdin is not a terminal.
Linux server 6.1.0-29-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.123-1 (2025-01-02) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
default via 10.0.2.2 dev eth0 
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 
10.200.0.0/24 via 192.168.56.11 dev eth1 
10.200.1.0/24 via 192.168.56.12 dev eth1 
192.168.56.0/24 dev eth1 proto kernel scope link src 192.168.56.10 
root@jumpbox:~/kubernetes-the-hard-way# echo $NODE_0_SUBNET
10.200.0.0/24
root@jumpbox:~/kubernetes-the-hard-way# echo $NODE_1_SUBNET
10.200.1.0/24
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# ssh root@node-1 <<EOF
  ip route add ${NODE_0_SUBNET} via ${NODE_0_IP}
EOF
Pseudo-terminal will not be allocated because stdin is not a terminal.
Linux node-1 6.1.0-29-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.123-1 (2025-01-02) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
root@jumpbox:~/kubernetes-the-hard-way# ssh root@server ip route
default via 10.0.2.2 dev eth0 
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 
10.200.0.0/24 via 192.168.56.11 dev eth1 
10.200.1.0/24 via 192.168.56.12 dev eth1 
192.168.56.0/24 dev eth1 proto kernel scope link src 192.168.56.10 
root@jumpbox:~/kubernetes-the-hard-way# ssh root@node-0 ip route
default via 10.0.2.2 dev eth0 
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 
10.200.1.0/24 via 192.168.56.12 dev eth1 
192.168.56.0/24 dev eth1 proto kernel scope link src 192.168.56.11 
root@jumpbox:~/kubernetes-the-hard-way# ssh root@node-1 ip route
default via 10.0.2.2 dev eth0 
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 
10.200.0.0/24 via 192.168.56.11 dev eth1 
192.168.56.0/24 dev eth1 proto kernel scope link src 192.168.56.12 
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 

```

Next: [Lab_12_Smoke_Test.md](Lab_12_Smoke_Test.md)