### Set Up The Jumpbox

In this lab you will set up one of the four machines to be a jumpbox. 

This machine will be used to run commands throughout this tutorial. 



```bash
┌──(mrkali㉿kalilinux)-[~/k8s]
└─$ ls
Vagrantfile  hosts  id_rsa  id_rsa.pub  machines.txt
                                                                                                                                                                                                                                                     
┌──(mrkali㉿kalilinux)-[~/k8s]
└─$ vagrant status
Current machine states:

k8s_server                running (virtualbox)
k8s_node_0                running (virtualbox)
k8s_node_1                running (virtualbox)
k8s_jumpbox               running (virtualbox)

This environment represents multiple VMs. The VMs are all listed
above with their current state. For more information about a specific
VM, run `vagrant status NAME`.
                                                                                                                                                                                                                                                     
┌──(mrkali㉿kalilinux)-[~/k8s]
└─$ ssh -i id_rsa root@jumpbox.kubernetes.local
Linux jumpbox 6.1.0-29-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.123-1 (2025-01-02) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sat Apr 12 00:24:59 2025 from 192.168.56.1
root@jumpbox:~# kubectl version --client
-bash: kubectl: command not found
root@jumpbox:~# {
  apt-get update
  apt-get -y install wget curl vim openssl git
}
Hit:1 https://deb.debian.org/debian bookworm InRelease
Hit:2 https://deb.debian.org/debian bookworm-updates InRelease
Hit:3 https://deb.debian.org/debian bookworm-backports InRelease
Hit:4 https://security.debian.org/debian-security bookworm-security InRelease
Reading package lists... Done                          
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
openssl is already the newest version (3.0.15-1~deb12u1).
The following additional packages will be installed:
  git-man libcurl3-gnutls libcurl4 liberror-perl libgpm2 libsodium23 patch vim-common vim-runtime vim-tiny
Suggested packages:
  git-daemon-run | git-daemon-sysvinit git-doc git-email git-gui gitk gitweb git-cvs git-mediawiki git-svn gpm ed diffutils-doc ctags vim-doc vim-scripts indent
Recommended packages:
  xxd
The following NEW packages will be installed:
  curl git git-man libcurl4 liberror-perl libgpm2 libsodium23 patch vim vim-runtime
The following packages will be upgraded:
  libcurl3-gnutls vim-common vim-tiny wget
4 upgraded, 10 newly installed, 0 to remove and 27 not upgraded.
Need to get 21.1 MB of archives.
After this operation, 91.2 MB of additional disk space will be used.
Get:1 https://deb.debian.org/debian bookworm/main amd64 vim-tiny amd64 2:9.0.1378-2+deb12u2 [720 kB]
Get:2 https://deb.debian.org/debian bookworm/main amd64 vim-common all 2:9.0.1378-2+deb12u2 [125 kB]
Get:3 https://deb.debian.org/debian bookworm/main amd64 wget amd64 1.21.3-1+deb12u1 [937 kB]
Get:4 https://deb.debian.org/debian bookworm/main amd64 libcurl4 amd64 7.88.1-10+deb12u12 [391 kB]
Get:5 https://deb.debian.org/debian bookworm/main amd64 curl amd64 7.88.1-10+deb12u12 [315 kB]
Get:6 https://deb.debian.org/debian bookworm/main amd64 libcurl3-gnutls amd64 7.88.1-10+deb12u12 [386 kB]
Get:7 https://deb.debian.org/debian bookworm/main amd64 liberror-perl all 0.17029-2 [29.0 kB]
Get:8 https://deb.debian.org/debian bookworm/main amd64 git-man all 1:2.39.5-0+deb12u2 [2053 kB]
Get:9 https://deb.debian.org/debian bookworm/main amd64 git amd64 1:2.39.5-0+deb12u2 [7260 kB]
Get:10 https://deb.debian.org/debian bookworm/main amd64 libgpm2 amd64 1.20.7-10+b1 [14.2 kB]
Get:11 https://deb.debian.org/debian bookworm/main amd64 libsodium23 amd64 1.0.18-1 [161 kB]
Get:12 https://deb.debian.org/debian bookworm/main amd64 patch amd64 2.7.6-7 [128 kB]
Get:13 https://deb.debian.org/debian bookworm/main amd64 vim-runtime all 2:9.0.1378-2+deb12u2 [7027 kB]
Get:14 https://deb.debian.org/debian bookworm/main amd64 vim amd64 2:9.0.1378-2+deb12u2 [1568 kB]
Fetched 21.1 MB in 1s (32.0 MB/s)
apt-listchanges: Reading changelogs...
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
(Reading database ... 25536 files and directories currently installed.)
Preparing to unpack .../00-vim-tiny_2%3a9.0.1378-2+deb12u2_amd64.deb ...
Unpacking vim-tiny (2:9.0.1378-2+deb12u2) over (2:9.0.1378-2) ...
Preparing to unpack .../01-vim-common_2%3a9.0.1378-2+deb12u2_all.deb ...
Unpacking vim-common (2:9.0.1378-2+deb12u2) over (2:9.0.1378-2) ...
Preparing to unpack .../02-wget_1.21.3-1+deb12u1_amd64.deb ...
Unpacking wget (1.21.3-1+deb12u1) over (1.21.3-1+b2) ...
Selecting previously unselected package libcurl4:amd64.
Preparing to unpack .../03-libcurl4_7.88.1-10+deb12u12_amd64.deb ...
Unpacking libcurl4:amd64 (7.88.1-10+deb12u12) ...
Selecting previously unselected package curl.
Preparing to unpack .../04-curl_7.88.1-10+deb12u12_amd64.deb ...
Unpacking curl (7.88.1-10+deb12u12) ...
Preparing to unpack .../05-libcurl3-gnutls_7.88.1-10+deb12u12_amd64.deb ...
Unpacking libcurl3-gnutls:amd64 (7.88.1-10+deb12u12) over (7.88.1-10+deb12u8) ...
Selecting previously unselected package liberror-perl.
Preparing to unpack .../06-liberror-perl_0.17029-2_all.deb ...
Unpacking liberror-perl (0.17029-2) ...
Selecting previously unselected package git-man.
Preparing to unpack .../07-git-man_1%3a2.39.5-0+deb12u2_all.deb ...
Unpacking git-man (1:2.39.5-0+deb12u2) ...
Selecting previously unselected package git.
Preparing to unpack .../08-git_1%3a2.39.5-0+deb12u2_amd64.deb ...
Unpacking git (1:2.39.5-0+deb12u2) ...
Selecting previously unselected package libgpm2:amd64.
Preparing to unpack .../09-libgpm2_1.20.7-10+b1_amd64.deb ...
Unpacking libgpm2:amd64 (1.20.7-10+b1) ...
Selecting previously unselected package libsodium23:amd64.
Preparing to unpack .../10-libsodium23_1.0.18-1_amd64.deb ...
Unpacking libsodium23:amd64 (1.0.18-1) ...
Selecting previously unselected package patch.
Preparing to unpack .../11-patch_2.7.6-7_amd64.deb ...
Unpacking patch (2.7.6-7) ...
Selecting previously unselected package vim-runtime.
Preparing to unpack .../12-vim-runtime_2%3a9.0.1378-2+deb12u2_all.deb ...
Adding 'diversion of /usr/share/vim/vim90/doc/help.txt to /usr/share/vim/vim90/doc/help.txt.vim-tiny by vim-runtime'
Adding 'diversion of /usr/share/vim/vim90/doc/tags to /usr/share/vim/vim90/doc/tags.vim-tiny by vim-runtime'
Unpacking vim-runtime (2:9.0.1378-2+deb12u2) ...
Selecting previously unselected package vim.
Preparing to unpack .../13-vim_2%3a9.0.1378-2+deb12u2_amd64.deb ...
Unpacking vim (2:9.0.1378-2+deb12u2) ...
Setting up libsodium23:amd64 (1.0.18-1) ...
Setting up libgpm2:amd64 (1.20.7-10+b1) ...
Setting up wget (1.21.3-1+deb12u1) ...
Setting up libcurl3-gnutls:amd64 (7.88.1-10+deb12u12) ...
Setting up liberror-perl (0.17029-2) ...
Setting up vim-common (2:9.0.1378-2+deb12u2) ...
Setting up patch (2.7.6-7) ...
Setting up libcurl4:amd64 (7.88.1-10+deb12u12) ...
Setting up git-man (1:2.39.5-0+deb12u2) ...
Setting up curl (7.88.1-10+deb12u12) ...
Setting up vim-runtime (2:9.0.1378-2+deb12u2) ...
Setting up vim (2:9.0.1378-2+deb12u2) ...
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/ex (ex) in auto mode
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/rview (rview) in auto mode
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/rvim (rvim) in auto mode
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/vi (vi) in auto mode
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/view (view) in auto mode
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/vim (vim) in auto mode
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/vimdiff (vimdiff) in auto mode
Setting up vim-tiny (2:9.0.1378-2+deb12u2) ...
Setting up git (1:2.39.5-0+deb12u2) ...
Processing triggers for libc-bin (2.36-9+deb12u9) ...
Processing triggers for man-db (2.11.2-2) ...
Processing triggers for mailcap (3.70+nmu1) ...
root@jumpbox:~# ls
machines.txt
root@jumpbox:~# git clone --depth 1 \
  https://github.com/kelseyhightower/kubernetes-the-hard-way.git
Cloning into 'kubernetes-the-hard-way'...
remote: Enumerating objects: 41, done.
remote: Counting objects: 100% (41/41), done.
remote: Compressing objects: 100% (40/40), done.
remote: Total 41 (delta 3), reused 19 (delta 1), pack-reused 0 (from 0)
Receiving objects: 100% (41/41), 29.27 KiB | 665.00 KiB/s, done.
Resolving deltas: 100% (3/3), done.
root@jumpbox:~# cd kubernetes-the-hard-way
root@jumpbox:~/kubernetes-the-hard-way# cat downloads-$(dpkg --print-architecture).txt
https://dl.k8s.io/v1.32.3/bin/linux/amd64/kubectl
https://dl.k8s.io/v1.32.3/bin/linux/amd64/kube-apiserver
https://dl.k8s.io/v1.32.3/bin/linux/amd64/kube-controller-manager
https://dl.k8s.io/v1.32.3/bin/linux/amd64/kube-scheduler
https://dl.k8s.io/v1.32.3/bin/linux/amd64/kube-proxy
https://dl.k8s.io/v1.32.3/bin/linux/amd64/kubelet
https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.32.0/crictl-v1.32.0-linux-amd64.tar.gz
https://github.com/opencontainers/runc/releases/download/v1.3.0-rc.1/runc.amd64
https://github.com/containernetworking/plugins/releases/download/v1.6.2/cni-plugins-linux-amd64-v1.6.2.tgz
https://github.com/containerd/containerd/releases/download/v2.1.0-beta.0/containerd-2.1.0-beta.0-linux-amd64.tar.gz
https://github.com/etcd-io/etcd/releases/download/v3.6.0-rc.3/etcd-v3.6.0-rc.3-linux-amd64.tar.gz
root@jumpbox:~/kubernetes-the-hard-way# wget -q --show-progress \
  --https-only \
  --timestamping \
  -P downloads \
  -i downloads-$(dpkg --print-architecture).txt
kubectl                                                       100%[==============================================================================================================================================>]  54.67M  45.8MB/s    in 1.2s    
kube-apiserver                                                100%[==============================================================================================================================================>]  88.94M  45.5MB/s    in 2.0s    
kube-controller-manager                                       100%[==============================================================================================================================================>]  82.00M  18.6MB/s    in 4.4s    
kube-scheduler                                                100%[==============================================================================================================================================>]  62.79M  40.2MB/s    in 1.6s    
kube-proxy                                                    100%[==============================================================================================================================================>]  63.75M  27.7MB/s    in 2.3s    
kubelet                                                       100%[==============================================================================================================================================>]  73.82M  35.0MB/s    in 2.1s    
crictl-v1.32.0-linux-amd64.tar.gz                             100%[==============================================================================================================================================>]  18.21M  33.7MB/s    in 0.5s    
runc.amd64                                                    100%[==============================================================================================================================================>]  11.30M  28.3MB/s    in 0.4s    
cni-plugins-linux-amd64-v1.6.2.tgz                            100%[==============================================================================================================================================>]  50.35M  41.0MB/s    in 1.2s    
containerd-2.1.0-beta.0-linux-amd64.tar.gz                    100%[==============================================================================================================================================>]  37.01M  36.1MB/s    in 1.0s    
etcd-v3.6.0-rc.3-linux-amd64.tar.gz                           100%[==============================================================================================================================================>]  22.48M  31.5MB/s    in 0.7s    
root@jumpbox:~/kubernetes-the-hard-way# ls -oh downloads
total 566M
-rw-r--r-- 1 root 51M Jan  6 16:13 cni-plugins-linux-amd64-v1.6.2.tgz
-rw-r--r-- 1 root 38M Mar 18 02:33 containerd-2.1.0-beta.0-linux-amd64.tar.gz
-rw-r--r-- 1 root 19M Dec  9 09:16 crictl-v1.32.0-linux-amd64.tar.gz
-rw-r--r-- 1 root 23M Mar 27 23:15 etcd-v3.6.0-rc.3-linux-amd64.tar.gz
-rw-r--r-- 1 root 89M Mar 12 03:31 kube-apiserver
-rw-r--r-- 1 root 83M Mar 12 03:31 kube-controller-manager
-rw-r--r-- 1 root 64M Mar 12 03:31 kube-proxy
-rw-r--r-- 1 root 63M Mar 12 03:31 kube-scheduler
-rw-r--r-- 1 root 55M Mar 12 03:31 kubectl
-rw-r--r-- 1 root 74M Mar 12 03:31 kubelet
-rw-r--r-- 1 root 12M Mar  4 12:14 runc.amd64
root@jumpbox:~/kubernetes-the-hard-way# {
  ARCH=$(dpkg --print-architecture)
  mkdir -p downloads/{client,cni-plugins,controller,worker}
  tar -xvf downloads/crictl-v1.32.0-linux-${ARCH}.tar.gz \
    -C downloads/worker/
  tar -xvf downloads/containerd-2.1.0-beta.0-linux-${ARCH}.tar.gz \
    --strip-components 1 \
    -C downloads/worker/
  tar -xvf downloads/cni-plugins-linux-${ARCH}-v1.6.2.tgz \
    -C downloads/cni-plugins/
  tar -xvf downloads/etcd-v3.6.0-rc.3-linux-${ARCH}.tar.gz \
    -C downloads/ \
    --strip-components 1 \
    etcd-v3.6.0-rc.3-linux-${ARCH}/etcdctl \
    etcd-v3.6.0-rc.3-linux-${ARCH}/etcd
  mv downloads/{etcdctl,kubectl} downloads/client/
  mv downloads/{etcd,kube-apiserver,kube-controller-manager,kube-scheduler} \
    downloads/controller/
  mv downloads/{kubelet,kube-proxy} downloads/worker/
  mv downloads/runc.${ARCH} downloads/worker/runc
}
crictl
bin/containerd-shim-runc-v2
bin/containerd
bin/containerd-stress
bin/ctr
./
./ipvlan
./tap
./loopback
./host-device
./README.md
./portmap
./ptp
./vlan
./bridge
./firewall
./LICENSE
./macvlan
./dummy
./bandwidth
./vrf
./tuning
./static
./dhcp
./host-local
./sbr
etcd-v3.6.0-rc.3-linux-amd64/etcdctl
etcd-v3.6.0-rc.3-linux-amd64/etcd
root@jumpbox:~/kubernetes-the-hard-way# rm -rf downloads/*gz
root@jumpbox:~/kubernetes-the-hard-way# {
  chmod +x downloads/{client,cni-plugins,controller,worker}/*
}
root@jumpbox:~/kubernetes-the-hard-way# {
  cp downloads/client/kubectl /usr/local/bin/
}
root@jumpbox:~/kubernetes-the-hard-way# kubectl version --client
Client Version: v1.32.3
Kustomize Version: v5.5.0
root@jumpbox:~/kubernetes-the-hard-way# 

```