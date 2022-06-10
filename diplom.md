```
yc managed-kubernetes cluster get-credentials --id cat68v7ofv588b0s26ni --external
```

```
kubectl get pods
No resources found in default namespace.
```
```
nikolay@nikolay-VirtualBox:~/yandex-diplom/terraform$ kubectl get pods --all-namespaces
NAMESPACE     NAME                                     READY   STATUS    RESTARTS   AGE
kube-system   coredns-5f8dbbff8f-466wv                 1/1     Running   0          11m
kube-system   coredns-5f8dbbff8f-gv2x2                 1/1     Running   0          7m48s
kube-system   ip-masq-agent-hvq45                      1/1     Running   0          7m59s
kube-system   ip-masq-agent-qc6gv                      1/1     Running   0          8m1s
kube-system   ip-masq-agent-zsc9p                      1/1     Running   0          7m46s
kube-system   kube-dns-autoscaler-598db8ff9c-qkcz5     1/1     Running   0          11m
kube-system   kube-proxy-n8thv                         1/1     Running   0          7m46s
kube-system   kube-proxy-v2jpc                         1/1     Running   0          7m59s
kube-system   kube-proxy-xnnzf                         1/1     Running   0          8m1s
kube-system   metrics-server-v0.3.1-6b998b66d6-lx8zz   2/2     Running   0          7m47s
kube-system   npd-v0.8.0-9mh8s                         1/1     Running   0          7m46s
kube-system   npd-v0.8.0-d88lr                         1/1     Running   0          8m1s
kube-system   npd-v0.8.0-rw69h                         1/1     Running   0          8m
kube-system   yc-disk-csi-node-v2-4n5g2                6/6     Running   0          8m1s
kube-system   yc-disk-csi-node-v2-c6gzd                6/6     Running   0          8m
kube-system   yc-disk-csi-node-v2-xtwsq                6/6     Running   0          7m46s
```

API URL
В консоли выполните:
```
nikolay@nikolay-VirtualBox:~/yandex-diplom/terraform$ kubectl cluster-info | grep -E 'Kubernetes master|Kubernetes control plane' | awk '/http/ {print $NF}'
https://51.250.86.83
```
Выполните в той же папке с файлом:

kubectl apply -f gitlab-admin-service-account.yaml

После создания ресурсов выполните:

kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep gitlab | awk '{print $1}')

```
nikolay@nikolay-VirtualBox:~/yandex-diplom/terraform$ kubectl apply -f gitlab-admin-service-account.yaml
serviceaccount/gitlab created
clusterrolebinding.rbac.authorization.k8s.io/gitlab-admin created
nikolay@nikolay-VirtualBox:~/yandex-diplom/terraform$ kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep gitlab | awk '{print $1}')
Name:         gitlab-token-44q8w
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: gitlab
              kubernetes.io/service-account.uid: f51e551e-d8ab-4fc3-9b4a-0d7e426ecfeb

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1066 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6ImVJb1d2TXplUE1WeWE3Tmd0YlFzZXNKU0NBd1RlNGQ3S01lZHFLOEtKWGMifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJnaXRsYWItdG9rZW4tNDRxOHciLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiZ2l0bGFiIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiZjUxZTU1MWUtZDhhYi00ZmMzLTliNGEtMGQ3ZTQyNmVjZmViIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Omt1YmUtc3lzdGVtOmdpdGxhYiJ9.cALh6v3C8ePAOTml0FmiZ_ArSL3SUPNU_xUAbGqEOUQ20795PNYaZjKxvKv311HeAUfOhoGFU1h7-IK1Kr0ggXhmGF4zJ9W0QAxxpaMeaRJ_48HdGnvfqx9QZ4l9rxnVBpJYp882uqzvMivnQ7bmY2wpIyJ7mW0ln1HiAs_pjccRRrYC6as1WNFOG9lXRAWyM5QCz66W8HyZX6eVJxiaxh0edvPCZuULUD8WuIP0nfWnGSCtU_Oh7TBTgh5pSTF0bgZAsTnhuD6VrGN5AoAUzhZ7OVtwK7BAZmxfiLADv7WhiPfZWBd5ZhoU8MktaonaMBnc-VVZHWWPM7Damc6mNg
```
Выполните команду:

kubectl get secrets

Найдите название секрета, который начинается на default-token-xxxxx.
```
nikolay@nikolay-VirtualBox:~/yandex-diplom/terraform$ kubectl get secrets
NAME                  TYPE                                  DATA   AGE
default-token-qwrq5   kubernetes.io/service-account-token   3      14m
```
kubectl get secret "default-token-qwrq5" -o jsonpath="{['data']['ca\.crt']}" | base64 --decode

```
nikolay@nikolay-VirtualBox:~/yandex-diplom/terraform$ kubectl get secret "default-token-qwrq5" -o jsonpath="{['data']['ca\.crt']}" | base64 --decode
-----BEGIN CERTIFICATE-----
MIIC5zCCAc+gAwIBAgIBADANBgkqhkiG9w0BAQsFADAVMRMwEQYDVQQDEwprdWJl
cm5ldGVzMB4XDTIyMDYxMDA5MDgwMloXDTMyMDYwNzA5MDgwMlowFTETMBEGA1UE
AxMKa3ViZXJuZXRlczCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKdJ
y7PEDlfU042XNwCRYSUzzJYCsN3rZIuatExF/48+YxWa4pHd17rAH2r4OvaSGbxl
ovBVf0Qc9REv7+tb30RX0HOeSREHEpDEH7SPCnRNrXej6U407FSS1LZHFJr6/j/i
3DrlkG7AFvGrI48x5NxbQN3jN7Fjr+Y/KeRXez5BJGCktf5erplJgAaAaExgrn1V
AkN+w3WuquHwGoeWj4SXpHe8/jMU0NcsYqzHEP2gz4N5UaFMsawJdcerFua3Tqwt
1gPZX3id0/LVpXDu/vrCJ7itMndKn1qlf1j4oUmyQpFv2U5P+03nIIaCCZ5b1jB6
+W3imDimYW1p8X+JF6ECAwEAAaNCMEAwDgYDVR0PAQH/BAQDAgKkMA8GA1UdEwEB
/wQFMAMBAf8wHQYDVR0OBBYEFK66vQY1ePRPurpUxP3PyL3t/X3bMA0GCSqGSIb3
DQEBCwUAA4IBAQAxbmIgWNuZ7aLRp2gvLs7ncjUpGTKLmk/OnVDo/Jvstc3BKnxU
ODhIgy9aGcsR5A50d17CTDUEocgnoWnq7I9hoio3PZizfXck1KKbClm1Pe9rC5dL
JoqDbFjeMFpn7+uUrx1iER+qyKSC3eGBGZEQhuY640UxmBIGHJdwX0S99Fk3QUpv
3ZxjlimG+OiE4va/5bS2q/LwK70LfjeyO35eXBlWIXNxnM8aTjG7BX/KPwaLg8VQ
hOfIIEcO37srohdCk4CEAOrj5FlTwq10LYukn5Xzm6nxYFG48wa9R/GyIxX1dJE6
ZnUZbFoSIItd1HULwQ/AVfDP/H90vFqCeNBh
-----END CERTIFICATE-----
```
