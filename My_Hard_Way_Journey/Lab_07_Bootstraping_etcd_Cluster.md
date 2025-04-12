# Bootstrapping the etcd Cluster

Kubernetes components are stateless and store cluster state in [etcd](https://github.com/etcd-io/etcd). In this lab you will bootstrap a single node etcd cluster.

```bash
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# date
Sat Apr 12 02:16:12 UTC 2025
root@jumpbox:~/kubernetes-the-hard-way# scp \
  downloads/controller/etcd \
  downloads/client/etcdctl \
  units/etcd.service \
  root@server:~/
etcd                                                                                                                                                                                                               100%   24MB  54.8MB/s   00:00    
etcdctl                                                                                                                                                                                                            100%   16MB  81.3MB/s   00:00    
etcd.service                                                                                                                                                                                                       100%  572   457.4KB/s   00:00    
root@jumpbox:~/kubernetes-the-hard-way# ssh root@server
Linux server 6.1.0-29-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.123-1 (2025-01-02) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sat Apr 12 00:25:19 2025 from 192.168.56.13
root@server:~# {
  mv etcd etcdctl /usr/local/bin/
}
root@server:~# {
  mkdir -p /etc/etcd /var/lib/etcd
  chmod 700 /var/lib/etcd
  cp ca.crt kube-api-server.key kube-api-server.crt \
    /etc/etcd/
}
root@server:~# mv etcd.service /etc/systemd/system/
root@server:~# {
  systemctl daemon-reload
  systemctl enable etcd
  systemctl start etcd
}
Created symlink /etc/systemd/system/multi-user.target.wants/etcd.service → /etc/systemd/system/etcd.service.
root@server:~# etcdctl member list
6702b0a34e2cfd39, started, controller, http://127.0.0.1:2380, http://127.0.0.1:2379, false
root@server:~# 

```