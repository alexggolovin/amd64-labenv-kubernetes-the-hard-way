# Configuring kubectl for Remote Access

In this lab you will generate a kubeconfig file for the `kubectl` command line utility based on the `admin` user credentials.

```bash
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# date
Sat Apr 12 05:50:46 UTC 2025
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
}root@jumpbox:~/kubernetes-the-hard-way#{{
  kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.crt \
    --embed-certs=true \
    --server=https://server.kubernetes.local:6443

  kubectl config set-credentials admin \
    --client-certificate=admin.crt \
    --client-key=admin.key

  kubectl config set-context kubernetes-the-hard-way \
    --cluster=kubernetes-the-hard-way \
    --user=admin

  kubectl config use-context kubernetes-the-hard-way
}
Cluster "kubernetes-the-hard-way" set.
User "admin" set.
Context "kubernetes-the-hard-way" created.
Switched to context "kubernetes-the-hard-way".
root@jumpbox:~/kubernetes-the-hard-way# ls -la ~/.kube/config 
-rw------- 1 root root 3007 Apr 12 05:51 /root/.kube/config
root@jumpbox:~/kubernetes-the-hard-way# cat ~/.kube/config
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUZURENDQXpTZ0F3SUJBZ0lVSzVrWnFBd2l0THMydWtsSk01M09VaEI3RTg0d0RRWUpLb1pJaHZjTkFRRU4KQlFBd1FURUxNQWtHQTFVRUJoTUNWVk14RXpBUkJnTlZCQWdNQ2xkaGMyaHBibWQwYjI0eEVEQU9CZ05WQkFjTQpCMU5sWVhSMGJHVXhDekFKQmdOVkJBTU1Ba05CTUI0WERUSTFNRFF4TWpBeE1EVXlNRm9YRFRNMU1EUXhNekF4Ck1EVXlNRm93UVRFTE1Ba0dBMVVFQmhNQ1ZWTXhFekFSQmdOVkJBZ01DbGRoYzJocGJtZDBiMjR4RURBT0JnTlYKQkFjTUIxTmxZWFIwYkdVeEN6QUpCZ05WQkFNTUFrTkJNSUlDSWpBTkJna3Foa2lHOXcwQkFRRUZBQU9DQWc4QQpNSUlDQ2dLQ0FnRUF0U3hET0dGMWx3WWF4RFY3VkU0dFZkQlNiRFlyaHJxeTQ5d1NWdnlqZlZ5cGVvbjVpVnlICmV6bWpzdWRsRGVlY0s0YmJiZWd3MFlLQVdJQlNnWDFQVmdvdi9FT2E0eHBQRUlVdnNZZ0RDdmFIbkJibHI2eVQKNVp2dmR3eHl5UnovMDBDTHl1MHppaUFGSlFBZ3FzV0VZeEtVTmlKdGUxRys3V1hGa3VJR1BCNTZ1Tm1ZaWtzcApKU09NN2ovWU1yVHRlZkx2SlRoUzhhTFhzbXlvdTQ4TDV1UlU5L2dMR3BadEMxWUUzanhWMktYTXhPYlpoSmQ3CmUxeGc4NlVnWTFUVjdwdy84WGR3T1o1UDNESk8xVWU2VHdWNXNCcFNWZUQrTTBtNFBBZjZqRjQwc3R2VzNJNGQKVzQ5ejB3WTVVQWpkcmNDNmtIMFFKUFVtTXV5cUpqNEY5L2lncWRvS2dKUHN3RTFWZzc2VXhncHBiZUIvNVM0NgpDRTJmNGh2ZkJEVDNrd0Z3ZCtUV3RIQ2hCN2piV3NvM2dUMHlIaU54aS9VQjc4SXVSMFdKTFlWWVpCeHU1aWM5CnlDUVlQTXVKZnhDQVE2UGdycHZUck43b3l2OGZic00zRHN0SmZiQ2U0a09Bd1NFMlRYVkpPaEFwejBaYzZLWFUKUXp1c1BBdlFNYmNBdmhXejE3V1oxL3UrM0swMzBXVUdDNEkxMFpCeS8zclRvRHRkeTQzamozNHJMUXdMaWt2Ywp3Wi9lWDlQbzBXQ1JiWEVRTXEreTd6aW5mUjlwdTU0b0NJcWFrR3VETGV2Mi9GNGJ2aWFBbmtHQ0x3bFlCQ0c3Ckt1Uno4NTFId2Z5WGhUaDVrL1M5bENVZlRjRUpzdjBzV1BZWjFIdjlTdmVDVkxKQWhTSWYrMWtDQXdFQUFhTTgKTURvd0RBWURWUjBUQkFVd0F3RUIvekFMQmdOVkhROEVCQU1DQVFZd0hRWURWUjBPQkJZRUZQWUtJNFFMYkNaagoweHlvK1FhN2hZTVQxdVFBTUEwR0NTcUdTSWIzRFFFQkRRVUFBNElDQVFCaURFQktUREFUc0Viei9kbEVmakx3ClcrV2FRM1BvMlBSTC9EQ1dteGJSSjJOS2NCa2x0T0tnWXQ0NEtMYXNLcFpXRURmVlNYNGdWUVVJdHlrMHU3QUoKSTNpL3pwcEg3ZWRmZmVhdVVBV3VJTmx5YTNyV1lMTXV1TXBBVzQ3Zm9KZGdWNUZBR1pGM0twdUJ1RHhtQldOOQpmOExMdTZIUVY4Y1IvQ3owanlPbTFQT01QOWhJQ1lGN1BEUktFZ1BhT3k1ZXpkbjBlT0RXUlVkbnloYnBWbXV2Ck00eWZFWDFRQ0FIQ1VWREdqVHNhb0JyblA5Y2NTa0FOQTgwalJZaFQ5MzNDRFQ1a1hUSXl0OXdFTzV4ZGNTMmwKSlJhdjhMWGVXNWxHbnRlUDdvM2tDVytoZWpiUUR1TFEvREZQQTlwb3pwVnhVWHQwUVAwTmJwQy9kV1lQSzZXbgpiZnFhQ2VqQ0ZQTkVjT0VidkxaK1hSUDlxVVV0Q29jWHZMUCswbnZzTW5CY0lPa0pCejBLaFBFVWJHR1JpQnJ1Clo2OWk3YVA2ZTFiUXFuUzNaNVdoUWo0cERhY0JVU0JrS2JPTEZNZUtsZ3JKVjhxVFhjVGZxajV1YTkxRy8xYlQKbzhFNVhMaTlvbXkwTElqcDhBU2p2Wjd2TXRpV3RKenU4WGFXWmsxSWVJdStlb29DciszSzBGdGV3UXFxa1ZwMQpTR3k5eW91MU5lK21BalBRYWtIU0QvN0JCTTU5cjVIN3NkVStVY1BsbHpSQkdsUU5VM3JpWENsZ3l5OGNNRkhICitrQWNEWXR1SlFHN3RJTURlR2NsOEZ1SU1jbXdqeFkvZkFLMU9TS0xCZ0ZFSU9rQmh3dW15K2xoc3g2U29TK3cKWWZ6eVR2Mnk4TUlzMDlmV0dkZCtFdz09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    server: https://server.kubernetes.local:6443
  name: kubernetes-the-hard-way
contexts:
- context:
    cluster: kubernetes-the-hard-way
    user: admin
  name: kubernetes-the-hard-way
current-context: kubernetes-the-hard-way
kind: Config
preferences: {}
users:
- name: admin
  user:
    client-certificate: /root/kubernetes-the-hard-way/admin.crt
    client-key: /root/kubernetes-the-hard-way/admin.key
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# kubectl version
Client Version: v1.32.3
Kustomize Version: v5.5.0
Server Version: v1.32.3
root@jumpbox:~/kubernetes-the-hard-way# kubectl get nodes
NAME     STATUS   ROLES    AGE   VERSION
node-0   Ready    <none>   89m   v1.32.3
node-1   Ready    <none>   86m   v1.32.3
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 
root@jumpbox:~/kubernetes-the-hard-way# 

```

Next: [Lab_11_Pod_Network_Routes.md](Lab_11_Pod_Network_Routes.md)
