### Generating the Data Encryption Config and Key

Kubernetes stores a variety of data including cluster state, application configurations, and secrets. Kubernetes supports the ability to [encrypt](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data) cluster data at rest.

In this lab you will generate an encryption key and an [encryption config](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/#understanding-the-encryption-at-rest-configuration) suitable for encrypting Kubernetes Secrets.

```bash
root@jumpbox:~/kubernetes-the-hard-way# date
Sat Apr 12 02:11:01 UTC 2025
root@jumpbox:~/kubernetes-the-hard-way# export ENCRYPTION_KEY=$(head -c 32 /dev/urandom | base64)
root@jumpbox:~/kubernetes-the-hard-way# echo $ENCRYPTION_KEY
noY8j9G68HYP5C8J59L+uO/tIUoUwu3lRoUDY4ujtUo=
root@jumpbox:~/kubernetes-the-hard-way# envsubst < configs/encryption-config.yaml \
  > encryption-config.yaml
root@jumpbox:~/kubernetes-the-hard-way# cat encryption-config.yaml 
kind: EncryptionConfiguration
apiVersion: apiserver.config.k8s.io/v1
resources:
  - resources:
      - secrets
    providers:
      - aescbc:
          keys:
            - name: key1
              secret: noY8j9G68HYP5C8J59L+uO/tIUoUwu3lRoUDY4ujtUo=
      - identity: {}
root@jumpbox:~/kubernetes-the-hard-way# scp encryption-config.yaml root@server:~/
encryption-config.yaml                                                                                                                                                                                             100%  271   115.1KB/s   00:00    
root@jumpbox:~/kubernetes-the-hard-way# 

```

Next: [Lab_07_Bootstraping_etcd_Cluster.md](Lab_07_Bootstraping_etcd_Cluster.md)
