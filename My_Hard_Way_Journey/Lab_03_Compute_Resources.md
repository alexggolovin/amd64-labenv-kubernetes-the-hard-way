### Provisioning Compute Resources

Kubernetes requires a set of machines to host the Kubernetes control plane and the worker nodes where containers are ultimately run. In this lab you will provision the machines required for setting up a Kubernetes cluster.


```bash
root@jumpbox:~/kubernetes-the-hard-way# ls
CONTRIBUTING.md  COPYRIGHT.md  LICENSE	README.md  ca.conf  configs  docs  downloads  downloads-amd64.txt  downloads-arm64.txt	units
root@jumpbox:~/kubernetes-the-hard-way# cd
root@jumpbox:~# ls
kubernetes-the-hard-way  machines.txt
root@jumpbox:~# cat machines.txt 
192.168.56.10 server.kubernetes.local server
192.168.56.11 node-0.kubernetes.local node-0 10.200.0.0/24
192.168.56.12 node-1.kubernetes.local node-1 10.200.1.0/24
root@jumpbox:~# ls
kubernetes-the-hard-way  machines.txt
root@jumpbox:~# while read IP FQDN HOST SUBNET; do
  ssh -n root@${IP} hostname
done < machines.txt
The authenticity of host '192.168.56.10 (192.168.56.10)' can't be established.
ED25519 key fingerprint is SHA256:HwaxguqZzlVZvVHVXdpAufggLeVpcpl5fUsB9gHwdFU.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:1: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '192.168.56.10' (ED25519) to the list of known hosts.
server
The authenticity of host '192.168.56.11 (192.168.56.11)' can't be established.
ED25519 key fingerprint is SHA256:4AmzblZOQeLblr9RA5AR97n3si+9cHnfRSFCdvkze38.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
node-0
The authenticity of host '192.168.56.12 (192.168.56.12)' can't be established.
ED25519 key fingerprint is SHA256:D+NXSdBfF0wQ2kxZUGoZY93r6PdiEyH14mMcRHw+hRQ.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '192.168.56.12' (ED25519) to the list of known hosts.
node-1
root@jumpbox:~# while read IP FQDN HOST SUBNET; do   ssh -n root@${IP} hostname; done < machines.txt
server
node-0
node-1
root@jumpbox:~# while read IP FQDN HOST SUBNET; do
  ssh -n root@${IP} hostname --fqdn
done < machines.txt
server.kubernetes.local
node-0.kubernetes.local
node-1.kubernetes.local
root@jumpbox:~# cat /etc/hosts
192.168.56.13	jumpbox.kubernetes.local	jumpbox
127.0.0.1	localhost
127.0.0.2	bookworm
::1		localhost ip6-localhost ip6-loopback
ff02::1		ip6-allnodes
ff02::2		ip6-allrouters

192.168.56.10 server.kubernetes.local server
192.168.56.11 node-0.kubernetes.local node-0
192.168.56.12 node-1.kubernetes.local node-1
192.168.56.13 jumpbox.kubernetes.local jumpbox
root@jumpbox:~# for host in server node-0 node-1
   do ssh root@${host} hostname
done
The authenticity of host 'server (192.168.56.10)' can't be established.
ED25519 key fingerprint is SHA256:HwaxguqZzlVZvVHVXdpAufggLeVpcpl5fUsB9gHwdFU.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:1: [hashed name]
    ~/.ssh/known_hosts:4: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'server' (ED25519) to the list of known hosts.
server
The authenticity of host 'node-0 (192.168.56.11)' can't be established.
ED25519 key fingerprint is SHA256:4AmzblZOQeLblr9RA5AR97n3si+9cHnfRSFCdvkze38.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:5: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'node-0' (ED25519) to the list of known hosts.
node-0
The authenticity of host 'node-1 (192.168.56.12)' can't be established.
ED25519 key fingerprint is SHA256:D+NXSdBfF0wQ2kxZUGoZY93r6PdiEyH14mMcRHw+hRQ.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:8: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'node-1' (ED25519) to the list of known hosts.
node-1
root@jumpbox:~# for host in server node-0 node-1;    do ssh root@${host} hostname; done
server
node-0
node-1
root@jumpbox:~# 

```

Next: [Lab_04_CertAuthority.md](Lab_04_CertAuthority.md)
