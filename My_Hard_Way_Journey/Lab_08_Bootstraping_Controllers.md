# Bootstrapping the Kubernetes Control Plane

In this lab you will bootstrap the Kubernetes control plane. The following components will be installed on the `server` machine: Kubernetes API Server, Scheduler, and Controller Manager.

```bash
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# date
Sat Apr 12 02:21:27 UTC 2025
root@jumpbox:~/kubernetes-the-hard-way# scp \
  downloads/controller/kube-apiserver \
  downloads/controller/kube-controller-manager \
  downloads/controller/kube-scheduler \
  downloads/client/kubectl \
  units/kube-apiserver.service \
  units/kube-controller-manager.service \
  units/kube-scheduler.service \
  configs/kube-scheduler.yaml \
  configs/kube-apiserver-to-kubelet.yaml \
  root@server:~/
kube-apiserver                                                                                                                                                                                                     100%   89MB  74.7MB/s   00:01    
kube-controller-manager                                                                                                                                                                                            100%   82MB  84.5MB/s   00:00    
kube-scheduler                                                                                                                                                                                                     100%   63MB  84.7MB/s   00:00    
kubectl                                                                                                                                                                                                            100%   55MB  81.6MB/s   00:00    
kube-apiserver.service                                                                                                                                                                                             100% 1374     1.1MB/s   00:00    
kube-controller-manager.service                                                                                                                                                                                    100%  735   651.9KB/s   00:00    
kube-scheduler.service                                                                                                                                                                                             100%  281   246.1KB/s   00:00    
kube-scheduler.yaml                                                                                                                                                                                                100%  191   134.2KB/s   00:00    
kube-apiserver-to-kubelet.yaml                                                                                                                                                                                     100%  727   603.1KB/s   00:00    
root@jumpbox:~/kubernetes-the-hard-way# ssh root@server
Linux server 6.1.0-29-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.123-1 (2025-01-02) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sat Apr 12 02:17:06 2025 from 192.168.56.13
root@server:~# 
root@server:~# 
root@server:~# mkdir -p /etc/kubernetes/config
root@server:~# {
  mv kube-apiserver \
    kube-controller-manager \
    kube-scheduler kubectl \
    /usr/local/bin/
}
root@server:~# {
  mkdir -p /var/lib/kubernetes/

  mv ca.crt ca.key \
    kube-api-server.key kube-api-server.crt \
    service-accounts.key service-accounts.crt \
    encryption-config.yaml \
    /var/lib/kubernetes/
}
root@server:~# mv kube-apiserver.service \
  /etc/systemd/system/kube-apiserver.service
root@server:~# mv kube-controller-manager.kubeconfig /var/lib/kubernetes/
root@server:~# mv kube-controller-manager.service /etc/systemd/system/
root@server:~# mv kube-scheduler.kubeconfig /var/lib/kubernetes/
root@server:~# mv kube-scheduler.yaml /etc/kubernetes/config/
root@server:~# mv kube-scheduler.service /etc/systemd/system/
root@server:~# {
  systemctl daemon-reload

  systemctl enable kube-apiserver \
    kube-controller-manager kube-scheduler

  systemctl start kube-apiserver \
    kube-controller-manager kube-scheduler
}
Created symlink /etc/systemd/system/multi-user.target.wants/kube-apiserver.service → /etc/systemd/system/kube-apiserver.service.
Created symlink /etc/systemd/system/multi-user.target.wants/kube-controller-manager.service → /etc/systemd/system/kube-controller-manager.service.
Created symlink /etc/systemd/system/multi-user.target.wants/kube-scheduler.service → /etc/systemd/system/kube-scheduler.service.
root@server:~# systemctl is-active kube-apiserver
active
root@server:~# systemctl status kube-apiserver
● kube-apiserver.service - Kubernetes API Server
     Loaded: loaded (/etc/systemd/system/kube-apiserver.service; enabled; preset: enabled)
     Active: active (running) since Sat 2025-04-12 02:23:51 UTC; 28s ago
       Docs: https://github.com/kubernetes/kubernetes
   Main PID: 2401 (kube-apiserver)
      Tasks: 9 (limit: 496)
     Memory: 233.3M
        CPU: 6.858s
     CGroup: /system.slice/kube-apiserver.service
             └─2401 /usr/local/bin/kube-apiserver --allow-privileged=true --audit-log-maxage=30 --audit-log-maxbackup=3 --audit-log-maxsize=100 --audit-log-path=/var/log/audit.log --authorization-mode=Node,RBAC --bind-address=0.0.0.0 --client-c>

Apr 12 02:23:56 server kube-apiserver[2401]: I0412 02:23:56.791809    2401 storage_rbac.go:321] created rolebinding.rbac.authorization.k8s.io/system:controller:bootstrap-signer in kube-public
Apr 12 02:23:56 server kube-apiserver[2401]: I0412 02:23:56.855656    2401 alloc.go:330] "allocated clusterIPs" service="default/kubernetes" clusterIPs={"IPv4":"10.0.0.1"}
Apr 12 02:23:56 server kube-apiserver[2401]: W0412 02:23:56.876510    2401 lease.go:265] Resetting endpoints for master service "kubernetes" to [10.0.2.15]
Apr 12 02:23:56 server kube-apiserver[2401]: I0412 02:23:56.878830    2401 controller.go:615] quota admission added evaluator for: endpoints
Apr 12 02:23:56 server kube-apiserver[2401]: I0412 02:23:56.887884    2401 controller.go:615] quota admission added evaluator for: endpointslices.discovery.k8s.io
Apr 12 02:23:59 server kube-apiserver[2401]: I0412 02:23:59.083834    2401 controller.go:615] quota admission added evaluator for: serviceaccounts
Apr 12 02:24:03 server kube-apiserver[2401]: I0412 02:24:03.571089    2401 cacher.go:1028] cacher (clusterroles.rbac.authorization.k8s.io): 1 objects queued in incoming channel.
Apr 12 02:24:03 server kube-apiserver[2401]: I0412 02:24:03.572171    2401 cacher.go:1028] cacher (clusterroles.rbac.authorization.k8s.io): 2 objects queued in incoming channel.
Apr 12 02:24:05 server kube-apiserver[2401]: I0412 02:24:05.161130    2401 apf_controller.go:493] "Update CurrentCL" plName="exempt" seatDemandHighWatermark=3 seatDemandAvg=0.07741719389965586 seatDemandStdev=0.2816239383370884 seatDemandSmooth>
Apr 12 02:24:15 server kube-apiserver[2401]: I0412 02:24:15.174089    2401 apf_controller.go:493] "Update CurrentCL" plName="exempt" seatDemandHighWatermark=1 seatDemandAvg=0.0014715779970585223 seatDemandStdev=0.0383329160807927 seatDemandSmoo>
root@server:~# journalctl -u kube-apiserver
Apr 12 02:23:51 server systemd[1]: Started kube-apiserver.service - Kubernetes API Server.
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.506839    2401 flags.go:64] FLAG: --admission-control="[]"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508576    2401 flags.go:64] FLAG: --admission-control-config-file=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508603    2401 flags.go:64] FLAG: --advertise-address="<nil>"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508612    2401 flags.go:64] FLAG: --aggregator-reject-forwarding-redirect="true"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508620    2401 flags.go:64] FLAG: --allow-metric-labels="[]"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508635    2401 flags.go:64] FLAG: --allow-metric-labels-manifest=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508640    2401 flags.go:64] FLAG: --allow-privileged="true"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508647    2401 flags.go:64] FLAG: --anonymous-auth="true"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508652    2401 flags.go:64] FLAG: --api-audiences="[]"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508660    2401 flags.go:64] FLAG: --apiserver-count="1"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508667    2401 flags.go:64] FLAG: --audit-log-batch-buffer-size="10000"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508673    2401 flags.go:64] FLAG: --audit-log-batch-max-size="1"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508678    2401 flags.go:64] FLAG: --audit-log-batch-max-wait="0s"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508685    2401 flags.go:64] FLAG: --audit-log-batch-throttle-burst="0"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508747    2401 flags.go:64] FLAG: --audit-log-batch-throttle-enable="false"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508754    2401 flags.go:64] FLAG: --audit-log-batch-throttle-qps="0"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508761    2401 flags.go:64] FLAG: --audit-log-compress="false"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508767    2401 flags.go:64] FLAG: --audit-log-format="json"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508774    2401 flags.go:64] FLAG: --audit-log-maxage="30"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508781    2401 flags.go:64] FLAG: --audit-log-maxbackup="3"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508789    2401 flags.go:64] FLAG: --audit-log-maxsize="100"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508796    2401 flags.go:64] FLAG: --audit-log-mode="blocking"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508802    2401 flags.go:64] FLAG: --audit-log-path="/var/log/audit.log"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508807    2401 flags.go:64] FLAG: --audit-log-truncate-enabled="false"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508814    2401 flags.go:64] FLAG: --audit-log-truncate-max-batch-size="10485760"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508832    2401 flags.go:64] FLAG: --audit-log-truncate-max-event-size="102400"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508838    2401 flags.go:64] FLAG: --audit-log-version="audit.k8s.io/v1"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508844    2401 flags.go:64] FLAG: --audit-policy-file=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508849    2401 flags.go:64] FLAG: --audit-webhook-batch-buffer-size="10000"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508854    2401 flags.go:64] FLAG: --audit-webhook-batch-initial-backoff="10s"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508859    2401 flags.go:64] FLAG: --audit-webhook-batch-max-size="400"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508865    2401 flags.go:64] FLAG: --audit-webhook-batch-max-wait="30s"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508870    2401 flags.go:64] FLAG: --audit-webhook-batch-throttle-burst="15"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508876    2401 flags.go:64] FLAG: --audit-webhook-batch-throttle-enable="true"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508881    2401 flags.go:64] FLAG: --audit-webhook-batch-throttle-qps="10"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508887    2401 flags.go:64] FLAG: --audit-webhook-config-file=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508893    2401 flags.go:64] FLAG: --audit-webhook-initial-backoff="10s"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508898    2401 flags.go:64] FLAG: --audit-webhook-mode="batch"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508903    2401 flags.go:64] FLAG: --audit-webhook-truncate-enabled="false"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508908    2401 flags.go:64] FLAG: --audit-webhook-truncate-max-batch-size="10485760"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508915    2401 flags.go:64] FLAG: --audit-webhook-truncate-max-event-size="102400"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508920    2401 flags.go:64] FLAG: --audit-webhook-version="audit.k8s.io/v1"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508926    2401 flags.go:64] FLAG: --authentication-config=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508931    2401 flags.go:64] FLAG: --authentication-token-webhook-cache-ttl="2m0s"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508937    2401 flags.go:64] FLAG: --authentication-token-webhook-config-file=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508942    2401 flags.go:64] FLAG: --authentication-token-webhook-version="v1beta1"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.508948    2401 flags.go:64] FLAG: --authorization-config=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509001    2401 flags.go:64] FLAG: --authorization-mode="[Node,RBAC]"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509013    2401 flags.go:64] FLAG: --authorization-policy-file=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509020    2401 flags.go:64] FLAG: --authorization-webhook-cache-authorized-ttl="5m0s"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509025    2401 flags.go:64] FLAG: --authorization-webhook-cache-unauthorized-ttl="30s"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509031    2401 flags.go:64] FLAG: --authorization-webhook-config-file=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509036    2401 flags.go:64] FLAG: --authorization-webhook-version="v1beta1"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509041    2401 flags.go:64] FLAG: --bind-address="0.0.0.0"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509048    2401 flags.go:64] FLAG: --cert-dir="/var/run/kubernetes"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509053    2401 flags.go:64] FLAG: --client-ca-file="/var/lib/kubernetes/ca.crt"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509059    2401 flags.go:64] FLAG: --cloud-config=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509065    2401 flags.go:64] FLAG: --cloud-provider=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509070    2401 flags.go:64] FLAG: --contention-profiling="false"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509076    2401 flags.go:64] FLAG: --cors-allowed-origins="[]"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509082    2401 flags.go:64] FLAG: --debug-socket-path=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509088    2401 flags.go:64] FLAG: --default-not-ready-toleration-seconds="300"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509095    2401 flags.go:64] FLAG: --default-unreachable-toleration-seconds="300"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509101    2401 flags.go:64] FLAG: --default-watch-cache-size="100"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509106    2401 flags.go:64] FLAG: --delete-collection-workers="1"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509112    2401 flags.go:64] FLAG: --disable-admission-plugins="[]"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509119    2401 flags.go:64] FLAG: --disable-http2-serving="false"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509124    2401 flags.go:64] FLAG: --disabled-metrics="[]"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509131    2401 flags.go:64] FLAG: --egress-selector-config-file=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509136    2401 flags.go:64] FLAG: --emulated-version="[]"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509142    2401 flags.go:64] FLAG: --enable-admission-plugins="[NamespaceLifecycle,NodeRestriction,LimitRanger,ServiceAccount,DefaultStorageClass,ResourceQuota]"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509158    2401 flags.go:64] FLAG: --enable-aggregator-routing="false"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509164    2401 flags.go:64] FLAG: --enable-bootstrap-token-auth="false"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509169    2401 flags.go:64] FLAG: --enable-garbage-collector="true"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509174    2401 flags.go:64] FLAG: --enable-logs-handler="false"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509179    2401 flags.go:64] FLAG: --enable-priority-and-fairness="true"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509184    2401 flags.go:64] FLAG: --encryption-provider-config="/var/lib/kubernetes/encryption-config.yaml"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509190    2401 flags.go:64] FLAG: --encryption-provider-config-automatic-reload="false"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509196    2401 flags.go:64] FLAG: --endpoint-reconciler-type="lease"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509201    2401 flags.go:64] FLAG: --etcd-cafile=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509206    2401 flags.go:64] FLAG: --etcd-certfile=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509213    2401 flags.go:64] FLAG: --etcd-compaction-interval="5m0s"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509218    2401 flags.go:64] FLAG: --etcd-count-metric-poll-period="1m0s"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509223    2401 flags.go:64] FLAG: --etcd-db-metric-poll-interval="30s"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509228    2401 flags.go:64] FLAG: --etcd-healthcheck-timeout="2s"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509233    2401 flags.go:64] FLAG: --etcd-keyfile=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509238    2401 flags.go:64] FLAG: --etcd-prefix="/registry"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509243    2401 flags.go:64] FLAG: --etcd-readycheck-timeout="2s"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509248    2401 flags.go:64] FLAG: --etcd-servers="[http://127.0.0.1:2379]"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509260    2401 flags.go:64] FLAG: --etcd-servers-overrides="[]"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509269    2401 flags.go:64] FLAG: --event-ttl="1h0m0s"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509274    2401 flags.go:64] FLAG: --external-hostname=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509279    2401 flags.go:64] FLAG: --feature-gates=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509292    2401 flags.go:64] FLAG: --goaway-chance="0"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509299    2401 flags.go:64] FLAG: --help="false"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509308    2401 flags.go:64] FLAG: --http2-max-streams-per-connection="0"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509313    2401 flags.go:64] FLAG: --kubelet-certificate-authority="/var/lib/kubernetes/ca.crt"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509319    2401 flags.go:64] FLAG: --kubelet-client-certificate="/var/lib/kubernetes/kube-api-server.crt"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509324    2401 flags.go:64] FLAG: --kubelet-client-key="/var/lib/kubernetes/kube-api-server.key"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509330    2401 flags.go:64] FLAG: --kubelet-port="10250"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509337    2401 flags.go:64] FLAG: --kubelet-preferred-address-types="[Hostname,InternalDNS,InternalIP,ExternalDNS,ExternalIP]"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509346    2401 flags.go:64] FLAG: --kubelet-read-only-port="10255"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509351    2401 flags.go:64] FLAG: --kubelet-timeout="5s"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509356    2401 flags.go:64] FLAG: --kubernetes-service-node-port="0"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509362    2401 flags.go:64] FLAG: --lease-reuse-duration-seconds="60"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509367    2401 flags.go:64] FLAG: --livez-grace-period="0s"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509372    2401 flags.go:64] FLAG: --log-flush-frequency="5s"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509378    2401 flags.go:64] FLAG: --log-json-info-buffer-size="0"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509388    2401 flags.go:64] FLAG: --log-json-split-stream="false"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509395    2401 flags.go:64] FLAG: --log-text-info-buffer-size="0"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509400    2401 flags.go:64] FLAG: --log-text-split-stream="false"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509408    2401 flags.go:64] FLAG: --logging-format="text"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509413    2401 flags.go:64] FLAG: --max-connection-bytes-per-sec="0"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509418    2401 flags.go:64] FLAG: --max-mutating-requests-inflight="200"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509423    2401 flags.go:64] FLAG: --max-requests-inflight="400"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509428    2401 flags.go:64] FLAG: --min-request-timeout="1800"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509433    2401 flags.go:64] FLAG: --oidc-ca-file=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509439    2401 flags.go:64] FLAG: --oidc-client-id=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509443    2401 flags.go:64] FLAG: --oidc-groups-claim=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509448    2401 flags.go:64] FLAG: --oidc-groups-prefix=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509453    2401 flags.go:64] FLAG: --oidc-issuer-url=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509458    2401 flags.go:64] FLAG: --oidc-required-claim=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509465    2401 flags.go:64] FLAG: --oidc-signing-algs="[RS256]"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509472    2401 flags.go:64] FLAG: --oidc-username-claim="sub"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509478    2401 flags.go:64] FLAG: --oidc-username-prefix=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509485    2401 flags.go:64] FLAG: --peer-advertise-ip=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509490    2401 flags.go:64] FLAG: --peer-advertise-port=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509498    2401 flags.go:64] FLAG: --peer-ca-file=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509503    2401 flags.go:64] FLAG: --permit-address-sharing="false"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509566    2401 flags.go:64] FLAG: --permit-port-sharing="false"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509572    2401 flags.go:64] FLAG: --profiling="true"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509578    2401 flags.go:64] FLAG: --proxy-client-cert-file=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509584    2401 flags.go:64] FLAG: --proxy-client-key-file=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509589    2401 flags.go:64] FLAG: --request-timeout="1m0s"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509594    2401 flags.go:64] FLAG: --requestheader-allowed-names="[]"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509602    2401 flags.go:64] FLAG: --requestheader-client-ca-file=""
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509607    2401 flags.go:64] FLAG: --requestheader-extra-headers-prefix="[]"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509613    2401 flags.go:64] FLAG: --requestheader-group-headers="[]"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509619    2401 flags.go:64] FLAG: --requestheader-uid-headers="[]"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509625    2401 flags.go:64] FLAG: --requestheader-username-headers="[]"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509631    2401 flags.go:64] FLAG: --runtime-config="api/all=true"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509646    2401 flags.go:64] FLAG: --secure-port="6443"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509651    2401 flags.go:64] FLAG: --service-account-extend-token-expiration="true"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509659    2401 flags.go:64] FLAG: --service-account-issuer="[https://server.kubernetes.local:6443]"
Apr 12 02:23:51 server kube-apiserver[2401]: I0412 02:23:51.509668    2401 flags.go:64] FLAG: --service-account-jwks-uri=""
root@server:~# 
root@server:~# 
root@server:~# 
root@server:~# kubectl cluster-info \
  --kubeconfig admin.kubeconfig
Kubernetes control plane is running at https://127.0.0.1:6443

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
root@server:~# 
root@server:~# 
root@server:~# 
root@server:~# 
root@server:~# date
Sat Apr 12 02:25:19 UTC 2025
root@server:~# 
root@server:~# 
root@server:~# kubectl apply -f kube-apiserver-to-kubelet.yaml \
  --kubeconfig admin.kubeconfig
clusterrole.rbac.authorization.k8s.io/system:kube-apiserver-to-kubelet created
clusterrolebinding.rbac.authorization.k8s.io/system:kube-apiserver created
root@server:~# 
root@server:~# 
root@server:~# cat kube-apiserver-to-kubelet.yaml 
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  annotations:
    rbac.authorization.kubernetes.io/autoupdate: "true"
  labels:
    kubernetes.io/bootstrapping: rbac-defaults
  name: system:kube-apiserver-to-kubelet
rules:
  - apiGroups:
      - ""
    resources:
      - nodes/proxy
      - nodes/stats
      - nodes/log
      - nodes/spec
      - nodes/metrics
    verbs:
      - "*"
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: system:kube-apiserver
  namespace: ""
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: system:kube-apiserver-to-kubelet
subjects:
  - apiGroup: rbac.authorization.k8s.io
    kind: User
    name: kubernetesroot@server:~# 
root@server:~# 
root@server:~# 
root@server:~# 
root@server:~# 
root@server:~# curl --cacert ca.crt \
  https://server.kubernetes.local:6443/version
-bash: curl: command not found
root@server:~# 
logout
Connection to server closed.
root@jumpbox:~/kubernetes-the-hard-way# curl --cacert ca.crt \
  https://server.kubernetes.local:6443/version
{
  "major": "1",
  "minor": "32",
  "gitVersion": "v1.32.3",
  "gitCommit": "32cc146f75aad04beaaa245a7157eb35063a9f99",
  "gitTreeState": "clean",
  "buildDate": "2025-03-11T19:52:21Z",
  "goVersion": "go1.23.6",
  "compiler": "gc",
  "platform": "linux/amd64"
}root@jumpbox:~/kubernetes-the-hard-way# curl --cacert ca.crt   https://server.kubernetes.local:6443/version
{
  "major": "1",
  "minor": "32",
  "gitVersion": "v1.32.3",
  "gitCommit": "32cc146f75aad04beaaa245a7157eb35063a9f99",
  "gitTreeState": "clean",
  "buildDate": "2025-03-11T19:52:21Z",
  "goVersion": "go1.23.6",
  "compiler": "gc",
  "platform": "linux/amd64"
}root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 


```