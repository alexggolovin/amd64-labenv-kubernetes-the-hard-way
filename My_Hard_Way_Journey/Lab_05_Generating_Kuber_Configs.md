### Generating Kubernetes Configuration Files for Authentication

In this lab you will generate [Kubernetes client configuration files](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/), typically called kubeconfigs, which configure Kubernetes clients to connect and authenticate to Kubernetes API Servers.


```bash
root@jumpbox:~/kubernetes-the-hard-way# ls
CONTRIBUTING.md  README.md  admin.key  ca.key	docs		     downloads-arm64.txt  kube-api-server.key	       kube-controller-manager.key  kube-proxy.key	kube-scheduler.key  node-0.key	node-1.key	      service-accounts.key
COPYRIGHT.md	 admin.crt  ca.conf    ca.srl	downloads	     kube-api-server.crt  kube-controller-manager.crt  kube-proxy.crt		    kube-scheduler.crt	node-0.crt	    node-1.crt	service-accounts.crt  units
LICENSE		 admin.csr  ca.crt     configs	downloads-amd64.txt  kube-api-server.csr  kube-controller-manager.csr  kube-proxy.csr		    kube-scheduler.csr	node-0.csr	    node-1.csr	service-accounts.csr
root@jumpbox:~/kubernetes-the-hard-way# for host in node-0 node-1; do
  kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.crt \
    --embed-certs=true \
    --server=https://server.kubernetes.local:6443 \
    --kubeconfig=${host}.kubeconfig

  kubectl config set-credentials system:node:${host} \
    --client-certificate=${host}.crt \
    --client-key=${host}.key \
    --embed-certs=true \
    --kubeconfig=${host}.kubeconfig

  kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=system:node:${host} \
    --kubeconfig=${host}.kubeconfig

  kubectl config use-context default \
    --kubeconfig=${host}.kubeconfig
done
Cluster "kubernetes-the-hard-way" set.
User "system:node:node-0" set.
Context "default" created.
Switched to context "default".
Cluster "kubernetes-the-hard-way" set.
User "system:node:node-1" set.
Context "default" created.
Switched to context "default".
root@jumpbox:~/kubernetes-the-hard-way# ls -1 | grep node
node-0.crt
node-0.csr
node-0.key
node-0.kubeconfig
node-1.crt
node-1.csr
node-1.key
node-1.kubeconfig
root@jumpbox:~/kubernetes-the-hard-way# {
  kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.crt \
    --embed-certs=true \
    --server=https://server.kubernetes.local:6443 \
    --kubeconfig=kube-proxy.kubeconfig

  kubectl config set-credentials system:kube-proxy \
    --client-certificate=kube-proxy.crt \
    --client-key=kube-proxy.key \
    --embed-certs=true \
    --kubeconfig=kube-proxy.kubeconfig

  kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=system:kube-proxy \
    --kubeconfig=kube-proxy.kubeconfig

  kubectl config use-context default \
    --kubeconfig=kube-proxy.kubeconfig
}
Cluster "kubernetes-the-hard-way" set.
User "system:kube-proxy" set.
Context "default" created.
Switched to context "default".
root@jumpbox:~/kubernetes-the-hard-way# ls -1 | grep proxy
kube-proxy.crt
kube-proxy.csr
kube-proxy.key
kube-proxy.kubeconfig
root@jumpbox:~/kubernetes-the-hard-way# {
  kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.crt \
    --embed-certs=true \
    --server=https://server.kubernetes.local:6443 \
    --kubeconfig=kube-controller-manager.kubeconfig

  kubectl config set-credentials system:kube-controller-manager \
    --client-certificate=kube-controller-manager.crt \
    --client-key=kube-controller-manager.key \
    --embed-certs=true \
    --kubeconfig=kube-controller-manager.kubeconfig

  kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=system:kube-controller-manager \
    --kubeconfig=kube-controller-manager.kubeconfig

  kubectl config use-context default \
    --kubeconfig=kube-controller-manager.kubeconfig
}
Cluster "kubernetes-the-hard-way" set.
User "system:kube-controller-manager" set.
Context "default" created.
Switched to context "default".
root@jumpbox:~/kubernetes-the-hard-way# ls -1 | grep controller
kube-controller-manager.crt
kube-controller-manager.csr
kube-controller-manager.key
kube-controller-manager.kubeconfig
root@jumpbox:~/kubernetes-the-hard-way# {
  kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.crt \
    --embed-certs=true \
    --server=https://server.kubernetes.local:6443 \
    --kubeconfig=kube-scheduler.kubeconfig

  kubectl config set-credentials system:kube-scheduler \
    --client-certificate=kube-scheduler.crt \
    --client-key=kube-scheduler.key \
    --embed-certs=true \
    --kubeconfig=kube-scheduler.kubeconfig

  kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=system:kube-scheduler \
    --kubeconfig=kube-scheduler.kubeconfig

  kubectl config use-context default \
    --kubeconfig=kube-scheduler.kubeconfig
}
Cluster "kubernetes-the-hard-way" set.
User "system:kube-scheduler" set.
Context "default" created.
Switched to context "default".
root@jumpbox:~/kubernetes-the-hard-way# ls -1 | grep scheduler
kube-scheduler.crt
kube-scheduler.csr
kube-scheduler.key
kube-scheduler.kubeconfig
root@jumpbox:~/kubernetes-the-hard-way# {
  kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.crt \
    --embed-certs=true \
    --server=https://127.0.0.1:6443 \
    --kubeconfig=admin.kubeconfig

  kubectl config set-credentials admin \
    --client-certificate=admin.crt \
    --client-key=admin.key \
    --embed-certs=true \
    --kubeconfig=admin.kubeconfig

  kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=admin \
    --kubeconfig=admin.kubeconfig

  kubectl config use-context default \
    --kubeconfig=admin.kubeconfig
}
Cluster "kubernetes-the-hard-way" set.
User "admin" set.
Context "default" created.
Switched to context "default".
root@jumpbox:~/kubernetes-the-hard-way# ls -1 | grep admin
admin.crt
admin.csr
admin.key
admin.kubeconfig
root@jumpbox:~/kubernetes-the-hard-way# for host in node-0 node-1; do
  ssh root@${host} "mkdir -p /var/lib/{kube-proxy,kubelet}"

  scp kube-proxy.kubeconfig \
    root@${host}:/var/lib/kube-proxy/kubeconfig \

  scp ${host}.kubeconfig \
    root@${host}:/var/lib/kubelet/kubeconfig
done
kube-proxy.kubeconfig                                                                                                                                                                                              100%   10KB   1.4MB/s   00:00    
node-0.kubeconfig                                                                                                                                                                                                  100%   10KB   3.3MB/s   00:00    
kube-proxy.kubeconfig                                                                                                                                                                                              100%   10KB   2.4MB/s   00:00    
node-1.kubeconfig                                                                                                                                                                                                  100%   10KB   2.0MB/s   00:00    
root@jumpbox:~/kubernetes-the-hard-way# scp admin.kubeconfig \
  kube-controller-manager.kubeconfig \
  kube-scheduler.kubeconfig \
  root@server:~/
admin.kubeconfig                                                                                                                                                                                                   100% 9953     4.6MB/s   00:00    
kube-controller-manager.kubeconfig                                                                                                                                                                                 100%   10KB   4.5MB/s   00:00    
kube-scheduler.kubeconfig                                                                                                                                                                                          100%   10KB   5.0MB/s   00:00    
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# date
Sat Apr 12 02:06:43 UTC 2025
root@jumpbox:~/kubernetes-the-hard-way# 
```

Next: [Lab_06_Gen_Data_Encrypt_Config_Key.md](Lab_06_Gen_Data_Encrypt_Config_Key.md)
