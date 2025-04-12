# Smoke Test

In this lab you will complete a series of tasks to ensure your Kubernetes cluster is functioning correctly.


## Data Encryption

In this section you will verify the ability to [encrypt secret data at rest](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/#verifying-that-data-is-encrypted).

Create a generic secret:

```bash
kubectl create secret generic kubernetes-the-hard-way \
  --from-literal="mykey=mydata"
```

```bash
root@jumpbox:~/kubernetes-the-hard-way# date
Sat Apr 12 06:06:40 UTC 2025
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# kubectl create secret generic kubernetes-the-hard-way \
  --from-literal="mykey=mydata"
secret/kubernetes-the-hard-way created
root@jumpbox:~/kubernetes-the-hard-way# ssh root@server \
    'etcdctl get /registry/secrets/default/kubernetes-the-hard-way | hexdump -C'
00000000  2f 72 65 67 69 73 74 72  79 2f 73 65 63 72 65 74  |/registry/secret|
00000010  73 2f 64 65 66 61 75 6c  74 2f 6b 75 62 65 72 6e  |s/default/kubern|
00000020  65 74 65 73 2d 74 68 65  2d 68 61 72 64 2d 77 61  |etes-the-hard-wa|
00000030  79 0a 6b 38 73 3a 65 6e  63 3a 61 65 73 63 62 63  |y.k8s:enc:aescbc|
00000040  3a 76 31 3a 6b 65 79 31  3a 24 96 ff d8 a6 fa 93  |:v1:key1:$......|
00000050  d8 f0 61 22 b1 f3 f6 e5  07 db 29 51 1c 01 a6 81  |..a"......)Q....|
00000060  11 65 39 7d 2e 9c 0d da  bf ee 6f 57 c5 9d d8 94  |.e9}......oW....|
00000070  bd e7 f3 5c d0 ef d4 dc  8f 1d 79 d2 f1 30 ff fa  |...\......y..0..|
00000080  3b 1c 7f 0e c8 f6 d1 93  84 28 79 f1 d8 f9 36 fd  |;........(y...6.|
00000090  a7 03 90 ed 7c c5 11 01  5e 9a 26 cf 98 78 ac 1b  |....|...^.&..x..|
000000a0  d4 89 0b f3 9c 81 3a f7  38 5c e3 43 f0 c1 f2 8a  |......:.8\.C....|
000000b0  84 80 90 43 08 5b 6a 68  8f f0 49 77 b6 02 ed 06  |...C.[jh..Iw....|
000000c0  28 d9 10 c7 a0 df d8 4b  3f c3 01 f3 4b f2 cf fb  |(......K?...K...|
000000d0  a8 b0 d7 27 d6 c8 8e 44  bb c0 0a b1 21 58 4c 53  |...'...D....!XLS|
000000e0  00 a8 d8 d6 5d c6 55 0d  37 7b ee 94 cf bd 56 eb  |....].U.7{....V.|
000000f0  c9 34 55 0c ff 69 f3 46  f9 a2 8a 73 19 03 37 c4  |.4U..i.F...s..7.|
00000100  8b a0 69 60 a5 dc 6f d9  60 d8 c0 b7 d3 94 aa da  |..i`..o.`.......|
00000110  36 7c 19 f9 36 7b d6 2b  b3 53 4b 6f e2 1a ce 77  |6|..6{.+.SKo...w|
00000120  c0 ec ec 16 d9 95 4c 9e  fa 0f a7 bc ff 95 3b 4e  |......L.......;N|
00000130  14 29 db 1d 49 f4 04 6e  2c 14 15 0b 0e a0 b4 f0  |.)..I..n,.......|
00000140  ea 9a 68 c5 82 a3 b6 b1  f0 84 df 29 5f fa 45 53  |..h........)_.ES|
00000150  c4 c0 46 ae 32 01 89 ed  cb 0a                    |..F.2.....|
0000015a
root@jumpbox:~/kubernetes-the-hard-way# 
```


The etcd key should be prefixed with `k8s:enc:aescbc:v1:key1`, which indicates the `aescbc` provider was used to encrypt the data with the `key1` encryption key.

## Deployments

In this section you will verify the ability to create and manage [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/).

Create a deployment for the [nginx](https://nginx.org/en/) web server:

```bash
kubectl create deployment nginx \
  --image=nginx:latest
```

List the pod created by the `nginx` deployment:

```bash
kubectl get pods -l app=nginx
```

```bash
NAME                     READY   STATUS    RESTARTS   AGE
nginx-56fcf95486-c8dnx   1/1     Running   0          8s
```

### Port Forwarding

In this section you will verify the ability to access applications remotely using [port forwarding](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/).

Retrieve the full name of the `nginx` pod:

```bash
POD_NAME=$(kubectl get pods -l app=nginx \
  -o jsonpath="{.items[0].metadata.name}")
```

Forward port `8080` on your local machine to port `80` of the `nginx` pod:

```bash
kubectl port-forward $POD_NAME 8080:80
```

```text
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80
```

In a new terminal make an HTTP request using the forwarding address:

```bash
curl --head http://127.0.0.1:8080
```

```text
HTTP/1.1 200 OK
Server: nginx/1.27.4
Date: Sun, 06 Apr 2025 17:17:12 GMT
Content-Type: text/html
Content-Length: 615
Last-Modified: Wed, 05 Feb 2025 11:06:32 GMT
Connection: keep-alive
ETag: "67a34638-267"
Accept-Ranges: bytes
```

Switch back to the previous terminal and stop the port forwarding to the `nginx` pod:

```text
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80
Handling connection for 8080
^C
```

### Logs

In this section you will verify the ability to [retrieve container logs](https://kubernetes.io/docs/concepts/cluster-administration/logging/).

Print the `nginx` pod logs:

```bash
kubectl logs $POD_NAME
```

```text
...
127.0.0.1 - - [06/Apr/2025:17:17:12 +0000] "HEAD / HTTP/1.1" 200 0 "-" "curl/7.88.1" "-"
```

### Exec

In this section you will verify the ability to [execute commands in a container](https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/#running-individual-commands-in-a-container).

Print the nginx version by executing the `nginx -v` command in the `nginx` container:

```bash
kubectl exec -ti $POD_NAME -- nginx -v
```

```text
nginx version: nginx/1.27.4
```

## Services

In this section you will verify the ability to expose applications using a [Service](https://kubernetes.io/docs/concepts/services-networking/service/).

Expose the `nginx` deployment using a [NodePort](https://kubernetes.io/docs/concepts/services-networking/service/#type-nodeport) service:

```bash
kubectl expose deployment nginx \
  --port 80 --type NodePort
```

> The LoadBalancer service type can not be used because your cluster is not configured with [cloud provider integration](https://kubernetes.io/docs/getting-started-guides/scratch/#cloud-provider). Setting up cloud provider integration is out of scope for this tutorial.

Retrieve the node port assigned to the `nginx` service:

```bash
NODE_PORT=$(kubectl get svc nginx \
  --output=jsonpath='{range .spec.ports[0]}{.nodePort}')
```

Retrieve the hostname of the node running the `nginx` pod:

```bash
NODE_NAME=$(kubectl get pods \
  -l app=nginx \
  -o jsonpath="{.items[0].spec.nodeName}")
```

Make an HTTP request using the IP address and the `nginx` node port:

```bash
curl -I http://${NODE_NAME}:${NODE_PORT}
```

```text
Server: nginx/1.27.4
Date: Sun, 06 Apr 2025 17:18:36 GMT
Content-Type: text/html
Content-Length: 615
Last-Modified: Wed, 05 Feb 2025 11:06:32 GMT
Connection: keep-alive
ETag: "67a34638-267"
Accept-Ranges: bytes
```

Next: Lab_13_Repeat [Lab_02_JumpBox.md](Lab_02_JumpBox.md)


