# Bootstrapping the Kubernetes Worker Nodes

In this lab you will bootstrap two Kubernetes worker nodes. The following components will be installed: [runc](https://github.com/opencontainers/runc), [container networking plugins](https://github.com/containernetworking/cni), [containerd](https://github.com/containerd/containerd), [kubelet](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet), and [kube-proxy](https://kubernetes.io/docs/concepts/cluster-administration/proxies).

```bash

root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# date
Sat Apr 12 04:11:40 UTC 2025
root@jumpbox:~/kubernetes-the-hard-way# for HOST in node-0 node-1; do
  SUBNET=$(grep ${HOST} machines.txt | cut -d " " -f 4)
  sed "s|SUBNET|$SUBNET|g" \
    configs/10-bridge.conf > 10-bridge.conf

  sed "s|SUBNET|$SUBNET|g" \
    configs/kubelet-config.yaml > kubelet-config.yaml

  scp 10-bridge.conf kubelet-config.yaml \
  root@${HOST}:~/
done
grep: machines.txt: No such file or directory
10-bridge.conf                                                                                                                                                                                                     100%  252   136.2KB/s   00:00    
kubelet-config.yaml                                                                                                                                                                                                100%  610   271.4KB/s   00:00    
grep: machines.txt: No such file or directory
10-bridge.conf                                                                                                                                                                                                     100%  252   153.4KB/s   00:00    
kubelet-config.yaml                                                                                                                                                                                                100%  610   394.6KB/s   00:00    
root@jumpbox:~/kubernetes-the-hard-way# cp ../machines.txt .
root@jumpbox:~/kubernetes-the-hard-way# cat machines.txt 
192.168.56.10 server.kubernetes.local server
192.168.56.11 node-0.kubernetes.local node-0 10.200.0.0/24
192.168.56.12 node-1.kubernetes.local node-1 10.200.1.0/24
root@jumpbox:~/kubernetes-the-hard-way# for HOST in node-0 node-1; do
  SUBNET=$(grep ${HOST} machines.txt | cut -d " " -f 4)
  sed "s|SUBNET|$SUBNET|g" \
    configs/10-bridge.conf > 10-bridge.conf

  sed "s|SUBNET|$SUBNET|g" \
    configs/kubelet-config.yaml > kubelet-config.yaml

  scp 10-bridge.conf kubelet-config.yaml \
  root@${HOST}:~/
done
10-bridge.conf                                                                                                                                                                                                     100%  265   252.6KB/s   00:00    
kubelet-config.yaml                                                                                                                                                                                                100%  610   646.6KB/s   00:00    
10-bridge.conf                                                                                                                                                                                                     100%  265   227.3KB/s   00:00    
kubelet-config.yaml                                                                                                                                                                                                100%  610   514.3KB/s   00:00    
root@jumpbox:~/kubernetes-the-hard-way# for HOST in node-0 node-1; do
  scp \
    downloads/worker/* \
    downloads/client/kubectl \
    configs/99-loopback.conf \
    configs/containerd-config.toml \
    configs/kube-proxy-config.yaml \
    units/containerd.service \
    units/kubelet.service \
    units/kube-proxy.service \
    root@${HOST}:~/
done
containerd                                                                                                                                                                                                         100%   56MB  65.0MB/s   00:00    
containerd-shim-runc-v2                                                                                                                                                                                            100% 8060KB  79.4MB/s   00:00    
containerd-stress                                                                                                                                                                                                  100%   22MB  81.9MB/s   00:00    
crictl                                                                                                                                                                                                             100%   38MB  78.1MB/s   00:00    
ctr                                                                                                                                                                                                                100%   23MB  83.7MB/s   00:00    
kube-proxy                                                                                                                                                                                                         100%   64MB  80.4MB/s   00:00    
kubelet                                                                                                                                                                                                            100%   74MB  85.8MB/s   00:00    
runc                                                                                                                                                                                                               100%   11MB  98.1MB/s   00:00    
kubectl                                                                                                                                                                                                            100%   55MB  83.4MB/s   00:00    
99-loopback.conf                                                                                                                                                                                                   100%   65    51.9KB/s   00:00    
containerd-config.toml                                                                                                                                                                                             100%  470   335.3KB/s   00:00    
kube-proxy-config.yaml                                                                                                                                                                                             100%  184   161.5KB/s   00:00    
containerd.service                                                                                                                                                                                                 100%  352   279.0KB/s   00:00    
kubelet.service                                                                                                                                                                                                    100%  365   338.1KB/s   00:00    
kube-proxy.service                                                                                                                                                                                                 100%  268   223.3KB/s   00:00    
containerd                                                                                                                                                                                                         100%   56MB  74.7MB/s   00:00    
containerd-shim-runc-v2                                                                                                                                                                                            100% 8060KB  86.1MB/s   00:00    
containerd-stress                                                                                                                                                                                                  100%   22MB  93.1MB/s   00:00    
crictl                                                                                                                                                                                                             100%   38MB  79.8MB/s   00:00    
ctr                                                                                                                                                                                                                100%   23MB  85.8MB/s   00:00    
kube-proxy                                                                                                                                                                                                         100%   64MB  81.7MB/s   00:00    
kubelet                                                                                                                                                                                                            100%   74MB  84.9MB/s   00:00    
runc                                                                                                                                                                                                               100%   11MB  85.8MB/s   00:00    
kubectl                                                                                                                                                                                                            100%   55MB  87.5MB/s   00:00    
99-loopback.conf                                                                                                                                                                                                   100%   65    45.8KB/s   00:00    
containerd-config.toml                                                                                                                                                                                             100%  470   362.6KB/s   00:00    
kube-proxy-config.yaml                                                                                                                                                                                             100%  184   118.1KB/s   00:00    
containerd.service                                                                                                                                                                                                 100%  352   231.4KB/s   00:00    
kubelet.service                                                                                                                                                                                                    100%  365   273.2KB/s   00:00    
kube-proxy.service                                                                                                                                                                                                 100%  268   172.6KB/s   00:00    
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# for HOST in node-0 node-1; do
  scp \
    downloads/cni-plugins/* \
    root@${HOST}:~/cni-plugins/
done
LICENSE                                                                                                                                                                                                            100%   11KB   5.9MB/s   00:00    
README.md                                                                                                                                                                                                          100% 2343     1.8MB/s   00:00    
bandwidth                                                                                                                                                                                                          100% 4546KB  20.5MB/s   00:00    
bridge                                                                                                                                                                                                             100% 5163KB  72.7MB/s   00:00    
dhcp                                                                                                                                                                                                               100%   12MB  85.6MB/s   00:00    
dummy                                                                                                                                                                                                              100% 4734KB  83.5MB/s   00:00    
firewall                                                                                                                                                                                                           100% 5191KB  82.8MB/s   00:00    
host-device                                                                                                                                                                                                        100% 4680KB  85.5MB/s   00:00    
host-local                                                                                                                                                                                                         100% 3965KB  49.3MB/s   00:00    
ipvlan                                                                                                                                                                                                             100% 4757KB  84.6MB/s   00:00    
loopback                                                                                                                                                                                                           100% 4018KB  80.5MB/s   00:00    
macvlan                                                                                                                                                                                                            100% 4788KB  83.3MB/s   00:00    
portmap                                                                                                                                                                                                            100% 4603KB  86.4MB/s   00:00    
ptp                                                                                                                                                                                                                100% 4958KB  84.2MB/s   00:00    
sbr                                                                                                                                                                                                                100% 4232KB  88.3MB/s   00:00    
static                                                                                                                                                                                                             100% 3566KB  79.7MB/s   00:00    
tap                                                                                                                                                                                                                100% 4813KB  70.5MB/s   00:00    
tuning                                                                                                                                                                                                             100% 4110KB  61.1MB/s   00:00    
vlan                                                                                                                                                                                                               100% 4754KB  82.2MB/s   00:00    
vrf                                                                                                                                                                                                                100% 4383KB 112.0MB/s   00:00    
LICENSE                                                                                                                                                                                                            100%   11KB   3.4MB/s   00:00    
README.md                                                                                                                                                                                                          100% 2343     1.1MB/s   00:00    
bandwidth                                                                                                                                                                                                          100% 4546KB  25.0MB/s   00:00    
bridge                                                                                                                                                                                                             100% 5163KB  67.5MB/s   00:00    
dhcp                                                                                                                                                                                                               100%   12MB  71.6MB/s   00:00    
dummy                                                                                                                                                                                                              100% 4734KB  82.3MB/s   00:00    
firewall                                                                                                                                                                                                           100% 5191KB 119.2MB/s   00:00    
host-device                                                                                                                                                                                                        100% 4680KB 126.5MB/s   00:00    
host-local                                                                                                                                                                                                         100% 3965KB 125.3MB/s   00:00    
ipvlan                                                                                                                                                                                                             100% 4757KB 120.4MB/s   00:00    
loopback                                                                                                                                                                                                           100% 4018KB 127.9MB/s   00:00    
macvlan                                                                                                                                                                                                            100% 4788KB 125.0MB/s   00:00    
portmap                                                                                                                                                                                                            100% 4603KB  94.4MB/s   00:00    
ptp                                                                                                                                                                                                                100% 4958KB  60.0MB/s   00:00    
sbr                                                                                                                                                                                                                100% 4232KB  93.5MB/s   00:00    
static                                                                                                                                                                                                             100% 3566KB 125.9MB/s   00:00    
tap                                                                                                                                                                                                                100% 4813KB 122.9MB/s   00:00    
tuning                                                                                                                                                                                                             100% 4110KB 104.9MB/s   00:00    
vlan                                                                                                                                                                                                               100% 4754KB 122.6MB/s   00:00    
vrf                                                                                                                                                                                                                100% 4383KB 101.9MB/s   00:00    
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# ssh root@node-0
Linux node-0 6.1.0-29-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.123-1 (2025-01-02) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
root@node-0:~# 
root@node-0:~# 
root@node-0:~# 
root@node-0:~# 
root@node-0:~# {
  apt-get update
  apt-get -y install socat conntrack ipset kmod
}
Hit:1 https://security.debian.org/debian-security bookworm-security InRelease
Hit:2 https://deb.debian.org/debian bookworm InRelease
Get:3 https://deb.debian.org/debian bookworm-updates InRelease [55.4 kB]
Get:4 https://deb.debian.org/debian bookworm-backports InRelease [59.4 kB]
Reading package lists... Done       
E: Release file for https://deb.debian.org/debian/dists/bookworm-updates/InRelease is not valid yet (invalid for another 3h 55min 34s). Updates for this repository will not be applied.
E: Release file for https://deb.debian.org/debian/dists/bookworm-backports/InRelease is not valid yet (invalid for another 3h 55min 33s). Updates for this repository will not be applied.
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
kmod is already the newest version (30+20221128-1).
The following NEW packages will be installed:
  conntrack ipset libipset13 socat
0 upgraded, 4 newly installed, 0 to remove and 31 not upgraded.
Need to get 524 kB of archives.
After this operation, 2084 kB of additional disk space will be used.
Get:1 https://deb.debian.org/debian bookworm/main amd64 conntrack amd64 1:1.4.7-1+b2 [35.2 kB]
Get:2 https://deb.debian.org/debian bookworm/main amd64 libipset13 amd64 7.17-1 [67.5 kB]
Get:3 https://deb.debian.org/debian bookworm/main amd64 ipset amd64 7.17-1 [45.7 kB]
Get:4 https://deb.debian.org/debian bookworm/main amd64 socat amd64 1.7.4.4-2 [375 kB]
Fetched 524 kB in 0s (2311 kB/s)
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
	LANGUAGE = (unset),
	LC_ALL = (unset),
	LC_CTYPE = "UTF-8",
	LANG = "C.UTF-8"
    are supported and installed on your system.
perl: warning: Falling back to a fallback locale ("C.UTF-8").
locale: Cannot set LC_CTYPE to default locale: No such file or directory
locale: Cannot set LC_ALL to default locale: No such file or directory
Selecting previously unselected package conntrack.
(Reading database ... 25536 files and directories currently installed.)
Preparing to unpack .../conntrack_1%3a1.4.7-1+b2_amd64.deb ...
Unpacking conntrack (1:1.4.7-1+b2) ...
Selecting previously unselected package libipset13:amd64.
Preparing to unpack .../libipset13_7.17-1_amd64.deb ...
Unpacking libipset13:amd64 (7.17-1) ...
Selecting previously unselected package ipset.
Preparing to unpack .../ipset_7.17-1_amd64.deb ...
Unpacking ipset (7.17-1) ...
Selecting previously unselected package socat.
Preparing to unpack .../socat_1.7.4.4-2_amd64.deb ...
Unpacking socat (1.7.4.4-2) ...
Setting up conntrack (1:1.4.7-1+b2) ...
Setting up socat (1.7.4.4-2) ...
Setting up libipset13:amd64 (7.17-1) ...
Setting up ipset (7.17-1) ...
Processing triggers for man-db (2.11.2-2) ...
Processing triggers for libc-bin (2.36-9+deb12u9) ...
root@node-0:~# 
root@node-0:~# 
root@node-0:~# 
root@node-0:~# swapon --show
root@node-0:~# 
root@node-0:~# swapoff -a
root@node-0:~# mkdir -p \
  /etc/cni/net.d \
  /opt/cni/bin \
  /var/lib/kubelet \
  /var/lib/kube-proxy \
  /var/lib/kubernetes \
  /var/run/kubernetes
root@node-0:~# {
  mv crictl kube-proxy kubelet runc \
    /usr/local/bin/
  mv containerd containerd-shim-runc-v2 containerd-stress /bin/
  mv cni-plugins/* /opt/cni/bin/
}
root@node-0:~# mv 10-bridge.conf 99-loopback.conf /etc/cni/net.d/
root@node-0:~# {
  modprobe br-netfilter
  echo "br-netfilter" >> /etc/modules-load.d/modules.conf
}
root@node-0:~# {
  echo "net.bridge.bridge-nf-call-iptables = 1" \
    >> /etc/sysctl.d/kubernetes.conf
  echo "net.bridge.bridge-nf-call-ip6tables = 1" \
    >> /etc/sysctl.d/kubernetes.conf
  sysctl -p /etc/sysctl.d/kubernetes.conf
}
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
root@node-0:~# {
  mkdir -p /etc/containerd/
  mv containerd-config.toml /etc/containerd/config.toml
  mv containerd.service /etc/systemd/system/
}
root@node-0:~# {
  mv kubelet-config.yaml /var/lib/kubelet/
  mv kubelet.service /etc/systemd/system/
}
root@node-0:~# {
  mv kube-proxy-config.yaml /var/lib/kube-proxy/
  mv kube-proxy.service /etc/systemd/system/
}
root@node-0:~# {
  systemctl daemon-reload
  systemctl enable containerd kubelet kube-proxy
  systemctl start containerd kubelet kube-proxy
}
Created symlink /etc/systemd/system/multi-user.target.wants/containerd.service → /etc/systemd/system/containerd.service.
Created symlink /etc/systemd/system/multi-user.target.wants/kubelet.service → /etc/systemd/system/kubelet.service.
Created symlink /etc/systemd/system/multi-user.target.wants/kube-proxy.service → /etc/systemd/system/kube-proxy.service.
root@node-0:~# systemctl is-active kubelet
active
root@node-0:~# 
root@node-0:~# 
root@node-0:~# 
root@node-0:~# 
root@node-0:~# ssh root@server \
  "kubectl get nodes \
  --kubeconfig admin.kubeconfig"
The authenticity of host 'server (192.168.56.10)' can't be established.
ED25519 key fingerprint is SHA256:HwaxguqZzlVZvVHVXdpAufggLeVpcpl5fUsB9gHwdFU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'server' (ED25519) to the list of known hosts.
NAME     STATUS   ROLES    AGE   VERSION
node-0   Ready    <none>   25s   v1.32.3
root@node-0:~# ssh root@server   "kubectl get nodes \
  --kubeconfig admin.kubeconfig"
NAME     STATUS   ROLES    AGE   VERSION
node-0   Ready    <none>   29s   v1.32.3
root@node-0:~# 
logout
Connection to node-0 closed.
root@jumpbox:~/kubernetes-the-hard-way# ssh node-1
Linux node-1 6.1.0-29-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.123-1 (2025-01-02) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
root@node-1:~# {
  apt-get update
  apt-get -y install socat conntrack ipset kmod
}
Hit:1 https://security.debian.org/debian-security bookworm-security InRelease
Hit:2 https://deb.debian.org/debian bookworm InRelease
Get:3 https://deb.debian.org/debian bookworm-updates InRelease [55.4 kB]
Get:4 https://deb.debian.org/debian bookworm-backports InRelease [59.4 kB]
Reading package lists... Done       
E: Release file for https://deb.debian.org/debian/dists/bookworm-updates/InRelease is not valid yet (invalid for another 3h 48min 6s). Updates for this repository will not be applied.
E: Release file for https://deb.debian.org/debian/dists/bookworm-backports/InRelease is not valid yet (invalid for another 3h 48min 4s). Updates for this repository will not be applied.
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
kmod is already the newest version (30+20221128-1).
The following NEW packages will be installed:
  conntrack ipset libipset13 socat
0 upgraded, 4 newly installed, 0 to remove and 31 not upgraded.
Need to get 524 kB of archives.
After this operation, 2084 kB of additional disk space will be used.
Get:1 https://deb.debian.org/debian bookworm/main amd64 conntrack amd64 1:1.4.7-1+b2 [35.2 kB]
Get:2 https://deb.debian.org/debian bookworm/main amd64 libipset13 amd64 7.17-1 [67.5 kB]
Get:3 https://deb.debian.org/debian bookworm/main amd64 ipset amd64 7.17-1 [45.7 kB]
Get:4 https://deb.debian.org/debian bookworm/main amd64 socat amd64 1.7.4.4-2 [375 kB]
Fetched 524 kB in 0s (2684 kB/s)
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
	LANGUAGE = (unset),
	LC_ALL = (unset),
	LC_CTYPE = "UTF-8",
	LANG = "C.UTF-8"
    are supported and installed on your system.
perl: warning: Falling back to a fallback locale ("C.UTF-8").
locale: Cannot set LC_CTYPE to default locale: No such file or directory
locale: Cannot set LC_ALL to default locale: No such file or directory
Selecting previously unselected package conntrack.
(Reading database ... 25536 files and directories currently installed.)
Preparing to unpack .../conntrack_1%3a1.4.7-1+b2_amd64.deb ...
Unpacking conntrack (1:1.4.7-1+b2) ...
Selecting previously unselected package libipset13:amd64.
Preparing to unpack .../libipset13_7.17-1_amd64.deb ...
Unpacking libipset13:amd64 (7.17-1) ...
Selecting previously unselected package ipset.
Preparing to unpack .../ipset_7.17-1_amd64.deb ...
Unpacking ipset (7.17-1) ...
Selecting previously unselected package socat.
Preparing to unpack .../socat_1.7.4.4-2_amd64.deb ...
Unpacking socat (1.7.4.4-2) ...
Setting up conntrack (1:1.4.7-1+b2) ...
Setting up socat (1.7.4.4-2) ...
Setting up libipset13:amd64 (7.17-1) ...
Setting up ipset (7.17-1) ...
Processing triggers for man-db (2.11.2-2) ...
Processing triggers for libc-bin (2.36-9+deb12u9) ...
root@node-1:~# swapon --show
root@node-1:~# swapoff -a
root@node-1:~# mkdir -p \
  /etc/cni/net.d \
  /opt/cni/bin \
  /var/lib/kubelet \
  /var/lib/kube-proxy \
  /var/lib/kubernetes \
  /var/run/kubernetes
root@node-1:~# {
  mv crictl kube-proxy kubelet runc \
    /usr/local/bin/
  mv containerd containerd-shim-runc-v2 containerd-stress /bin/
  mv cni-plugins/* /opt/cni/bin/
}
root@node-1:~# mv 10-bridge.conf 99-loopback.conf /etc/cni/net.d/
root@node-1:~# {
  modprobe br-netfilter
  echo "br-netfilter" >> /etc/modules-load.d/modules.conf
}
root@node-1:~# {
  echo "net.bridge.bridge-nf-call-iptables = 1" \
    >> /etc/sysctl.d/kubernetes.conf
  echo "net.bridge.bridge-nf-call-ip6tables = 1" \
    >> /etc/sysctl.d/kubernetes.conf
  sysctl -p /etc/sysctl.d/kubernetes.conf
}
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
root@node-1:~# {
  mkdir -p /etc/containerd/
  mv containerd-config.toml /etc/containerd/config.toml
  mv containerd.service /etc/systemd/system/
}
root@node-1:~# {
  mv kubelet-config.yaml /var/lib/kubelet/
  mv kubelet.service /etc/systemd/system/
}
root@node-1:~# {
  mv kube-proxy-config.yaml /var/lib/kube-proxy/
  mv kube-proxy.service /etc/systemd/system/
}
root@node-1:~# {
  systemctl daemon-reload
  systemctl enable containerd kubelet kube-proxy
  systemctl start containerd kubelet kube-proxy
}
Created symlink /etc/systemd/system/multi-user.target.wants/containerd.service → /etc/systemd/system/containerd.service.
Created symlink /etc/systemd/system/multi-user.target.wants/kubelet.service → /etc/systemd/system/kubelet.service.
Created symlink /etc/systemd/system/multi-user.target.wants/kube-proxy.service → /etc/systemd/system/kube-proxy.service.
root@node-1:~# systemctl is-active kubelet
active
root@node-1:~# ssh root@server \
  "kubectl get nodes \
  --kubeconfig admin.kubeconfig"
The authenticity of host 'server (192.168.56.10)' can't be established.
ED25519 key fingerprint is SHA256:HwaxguqZzlVZvVHVXdpAufggLeVpcpl5fUsB9gHwdFU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'server' (ED25519) to the list of known hosts.
NAME     STATUS   ROLES    AGE     VERSION
node-0   Ready    <none>   2m44s   v1.32.3
node-1   Ready    <none>   14s     v1.32.3
root@node-1:~# 
root@node-1:~# 
root@node-1:~# 
root@node-1:~# 
root@node-1:~# ssh root@server   "kubectl get nodes \
  --kubeconfig admin.kubeconfig"
NAME     STATUS   ROLES    AGE     VERSION
node-0   Ready    <none>   2m49s   v1.32.3
node-1   Ready    <none>   19s     v1.32.3
root@node-1:~# 

```

Next: [Lab_10_Config_Kubectl_Remote.md](Lab_10_Config_Kubectl_Remote.md)
