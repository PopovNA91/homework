
ntpdate -s ntp.ubuntu.com
sudo service ntp restart
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
https://51.250.94.165
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
Name:         gitlab-token-c7xzt
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: gitlab
              kubernetes.io/service-account.uid: 448ab5c9-856c-40fd-91d4-e2625187e6c9

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1066 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6ImFVTTh2clk3eDV2c2tuVWhteEpudzFiR2o0NUlMSTBjZWs3anJlcFZFRDAifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJnaXRsYWItdG9rZW4tYzd4enQiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiZ2l0bGFiIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiNDQ4YWI1YzktODU2Yy00MGZkLTkxZDQtZTI2MjUxODdlNmM5Iiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Omt1YmUtc3lzdGVtOmdpdGxhYiJ9.Soj798eLmg0RWKdPCADzg5imqk0m_JMDmkB5b8rJC0U1qhnqS5VEMy9Cur8NBgm8HIUntonsVe8_Z7z1jJ7bRMBKmGqHa9UuBeYAQNjA1Un1-hgt1ZQGwFbiMpGFx237Z_oXEWXNQ8I6N4BVuNf72FBY96eGAh0EEw1sudxnDLNtIXWS39H_1BGSL0xoLnRlfhWT1MDEMQ0SkpgHIM5jP429xNsSBDGTqid3hK5sYScyGHsVC9NW0Bq5IdXI3qWy0U5QJbjnIU6fSQeM5r6Aun2U2OqqcoSkhxy4AIbLfPhkobsWuhuqWcxn5zPVrrRxw8hNO-A-xsizX0a9hmkBlg

```
Выполните команду:

kubectl get secrets

Найдите название секрета, который начинается на default-token-xxxxx.
```
nikolay@nikolay-VirtualBox:~/yandex-diplom/terraform$ kubectl get secrets
NAME                  TYPE                                  DATA   AGE
default-token-dnxxp   kubernetes.io/service-account-token   3      12m

```
kubectl get secret "default-token-z9mx4" -o jsonpath="{['data']['ca\.crt']}" | base64 --decode

```
nikolay@nikolay-VirtualBox:~/yandex-diplom/terraform$ kubectl get secret "default-token-dnxxp" -o jsonpath="{['data']['ca\.crt']}" | base64 --decode
-----BEGIN CERTIFICATE-----
MIIC5zCCAc+gAwIBAgIBADANBgkqhkiG9w0BAQsFADAVMRMwEQYDVQQDEwprdWJl
cm5ldGVzMB4XDTIyMDYxNzA2MjczOFoXDTMyMDYxNDA2MjczOFowFTETMBEGA1UE
AxMKa3ViZXJuZXRlczCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAMZg
C/GZn5o0TVXFKs3j7Kp2/QJOp5ZPcM0/6DNhnwiGYtBmdngBZop6H++7TAA7qS36
ENtQs2RH/Zcon3dT9eA5hE9JwAA88m8BNnmLIYID6XfLRTWUPA0jxyV2Nrp2aUY4
MrmdcGpUi6EssoJ56q3G155UgwAprnxO5EOj+3dDZb1oJDR4Xd3P1mjYdnGx89Nx
hsbk0YMipVOhBn9gurJ1D9Q513IjGVVIpU7ZMmjHRBPrl/TIE59UkEdRNtWEbKe/
odj1dx5NPt8RUYkPQP5gPdxwVwkcTaBlEayDiPYNWlOuVAI94uiCchUqkJ4AcEym
R3k0soDwtalThs17x0UCAwEAAaNCMEAwDgYDVR0PAQH/BAQDAgKkMA8GA1UdEwEB
/wQFMAMBAf8wHQYDVR0OBBYEFLAodTRSJoDzAXgtGIORHvGySZguMA0GCSqGSIb3
DQEBCwUAA4IBAQCLQAIQr23zX0ifNF+JVVOMurkk6y8VQC5vYNEe5znS/zRjG5Wb
4zeVBDzG66Un1W27r9UUTWgTlYuct28cgyn7WCT8ZM5Bo9OM8kXLALJfoYS95klP
gVt8Avnk0V36qq9Kn3EQqbGihzUTJqNrm6UUlH4z/k4jee9GchLq0xiGQVj2maqi
NG0zgQ5hOiGBHcUlE4sLli3dFdf7UoIcA5WebyBcaJbqAYHQEaJ5Rn6Mmc8gThET
xSj9NnBqK1pdt/YVoZm4gx5N5PeZgycsHEAHzDi2kp4s5+F2mX5vFnPfA6bZnwUs
zL2XNJhYCPnGxbZJtKXcNzCtGaOFWekHJYLl
-----END CERTIFICATE-----

```
$KUBE_DEPLOY_SECRET_NAME=kubectl get sa gitlab -n kube-system -o jsonpath='{.secrets[0].name}'
KUBE_API_TOKEN=kubectl get secret gitlab-token-mrqbg -n kube-system -o jsonpath='{.data.token}'|base64 --decode
KUBE_API_CA=kubectl get secret gitlab-token-mrqbg -n kube-system -o jsonpath='{.data.ca\.crt}'|base64 --decode

qwFZR6bDpeutcW1AsKz_XTzPfurVQsqFQetLXw6toonzpqHaWw

Install using Helm (recommended)

From a terminal, connect to your cluster and run this command. The token is included in the command.
```
helm repo add gitlab https://charts.gitlab.io
helm repo update
helm upgrade --install agent-netology gitlab/gitlab-agent \
    --namespace gitlab-agent \
    --create-namespace \
    --set image.tag=v15.1.0 \
    --set config.token=qwFZR6bDpeutcW1AsKz_XTzPfurVQsqFQetLXw6toonzpqHaWw \
    --set config.kasAddress=wss://kas.gitlab.com
```
```
nikolay@nikolay-VirtualBox:~/yandex-diplom/terraform$ kubectl get pod,deployment -n my-nginx
NAME                            READY   STATUS    RESTARTS   AGE
pod/my-nginx-575c74d9c6-bfqwg   1/1     Running   0          56m

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-nginx   1/1     1            1           56m
```
