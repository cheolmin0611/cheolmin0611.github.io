---
layout: views
title: kubernetes
nav_order: 5
has_children: true
permalink: /kubernetes
---

# ![kubernetes icon](/assets/images/icon/Kubernetes_Logo_Hrz_lockup_REV1.png){:class="icon-title"} Kubernetes 설치

* [Installing kubeadm, kubelet and kubectl](#ㅑnstalling-kubeadm-kubelet-and-kubectl)
    * [Docker 설치](#-docker-설치)
* [kubeadm init 실행](#kubeadm-init-실행)
* [kubernetes dashboard 설치](#kubernetes-dashboard-설치)
    * [dashboard 기동 방법](#dashboard-기동-방법)
* [kubernetes dashboard - (metrics server) 설치](#kubernetes-dashboard---metrics-server-설치)
    * [Certificate](#-certificate)


Ubuntu 20.04 Docker 적용
{:class="desc-text"}

# Installing kubeadm, kubelet and kubectl

```bash
sudo apt-get update && sudo apt-get install -y apt-transport-https curl

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

sudo apt-get update

sudo apt-get install -y kubelet kubeadm kubectl

sudo apt-mark hold kubelet kubeadm kubectl
```

#### ![docker icon](/assets/images/icon/docker.svg){:class="icon-title"} [Docker 설치](/docker)



# kubeadm init 실행

apiserver-advertise-address 에는 ifconfig 나오는 ens5 마스터 노드의 ip를 적는다
{:class="desc-text"}

```bash
# images pull 후에 실행
sudo kubeadm config images pull

sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=10.10.0.151
```

-- woker node 에 적용될 결과
{:class="desc-text"}

sudo kubeadm join --node-name node-worker-01 10.10.0.154:6443 --token k7z4hy.5f47ac9h5bmpwikn --discovery-token-ca-cert-hash sha256:d99f25181a5581a74b8fa319f2caa16635923b247493ae87b9dc12f801c9f204
{:class="desc-text"}


```bash
# token 만료이후 다시 조회 생성 하는 command
# 1) 토큰을 모를 경우
sudo kubeadm token list
# 2) 토큰이 없는 경우 (기본 24시간 후 삭제)
sudo kubeadm token create
# 3) --discovery-token-ca-cert-hash를 모를 경우
openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | openssl dgst -sha256 -hex | sed 's/^.* //'
```

node에서 master 에 join후에 node에 taint 를 주려고 하는 경우

```bash
# taint 추가
kubectl taint node node-worker-1 key1=value1:NoSchedule

# taint 제거
kubectl taint nodes node-worker-1 key1-
```

셋업한 클러스터 사용 명령어

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

해당 명령어는 Root 계정이 아닌 다른 사용자 계정에서 kubectl 커맨드 명령어를 사용하여 클러스터를 제어하기 위해 사용하는 명령어이다.

<br/>
만약 다른 컴퓨터에서 kubectl을 사용하여 클러스터와 통신하고 싶다면 아래와 같이 admin.conf 파일을 마스터 노드에서 복사해와서 kubectl --kubeconfig 명령어를 통해 가능하다.
{:class="desc-text"}

```bash
scp ubuntu@<마스터 노드 IP>:/etc/kubernetes/admin.conf .
kubectl --kubeconfig ./admin.conf get nodes
```

또한 클러스터 외부에서 마스터 노드의 API 서버에 연결하고 싶다면 아래와 같이 `kubectl proxy` 명령어를 사용할 수 있다.
{:class="desc-text"}

```bash
scp ubuntu@<마스터 노드 IP>:/etc/kubernetes/admin.conf .
kubectl --kubeconfig ./admin.conf proxy
```

그 다음은 우리가 사용할 Pod 네트워크 애드온(Flannel)을 사용하기 위해 아래의 명령어를 통해 Pod 네트워크를 클러스터에 배포한다.

```bash
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

10.0.0.0/8 대역의 방화벽을 열어놔야함 - kubeadm init 시에 10.244.0.0/16 대역을 사용하기 때문에 각 node 간 방화벽이 동작하지 않도록 주의
{:class="desc-text"}

flannel 설정시 하단의 메시지가 나오는건 위의 셋업한 클러스터 사용 명령어 를 적용하지 않아서이다.
{:class="desc-text"}

The connection to the server localhost:8080 was refused - did you specify the right host or port?
{:class="desc-text"}

# kubernetes dashboard 설치

출처 [https://waspro.tistory.com/516](https://waspro.tistory.com/516)

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.2.0/aio/deploy/recommended.yaml
```

kubernetes 확인

![kubernetes 확인](/assets/images/views/kubernetes/Untitled.png)
<br/>
<br/>

### dashboard 기동 방법

- **Proxy를 이용하는 방법**
- **NodePort를 이용하는 방법**
- **API Server를 이용하는방법**

<br/>
 **API Server 이용 방법** - dashboard 기동 방법 중 API Server 를 이용하는 방법으로 진행

```bash
grep 'client-certificate-data' ~/.kube/config | head -n 1 | awk '{print $2}' | base64 -d >> kubecfg.crt

grep 'client-key-data' ~/.kube/config | head -n 1 | awk '{print $2}' | base64 -d >> kubecfg.key
```

<br/>
다음으로 생성한 키를 기반으로 p12 인증서 파일을 생성합니다. -  브라우저에 개별로 등록

```bash
openssl pkcs12 -export -clcerts -inkey kubecfg.key -in kubecfg.crt -out kubecfg.p12 -name "kubernetes-admin"

Enter Export Password:
Verifying - Enter Export Password:

ls -la kubecfg.p12
-rw-r--r-- 1 root root 3262 Aug  3 22:49 kubecfg.p12

```

**kubecfg.p12, /etc/kubernetes/pki/ca.crt** 인증서 client pc 로 copy
{:class="desc-text"}
ca.crt 는 동일한 이름으로 기관이 등록되어 있어 여러개 띄울때 에러 발생
{:class="desc-text"}

<br/>
<br/>
ca.crt 인증서 root에 전역으로 설치 - ca.crt 는 등록하지 않아도 사용 가능

```bash
sudo mkdir /usr/share/ca-certificates/extra

sudo cp ca.crt /usr/share/ca-certificates/extra/

sudo dpkg-reconfigure ca-certificates

```

<br/>
master 쪽에서 해당 명령어로 token get  - 하단의 kubernetes login 인증키 생성을 실행후 해야한다

```bash
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}')
```

<br/>
<br/>
- **Kubernetes Login 인증키 생성**

- Kubernetes는 접속방법 
    - kubeconfig
    - Token

<br/>
**Token 방식 적용**

- serviceaccount, clusterrolebinding 생성

```bash
vi eks-admin-service-account.yaml

apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kube-system
```

```bash
// 적용
kubectl apply -f eks-admin-service-account.yaml

// 삭제
kubectl delete -f eks-admin-service-account.yaml
```

<br/>
<br/>
- ~~serviceaccount를 생성~~
- 하단 코드 사용하지 않음

```bash
cat <<EOF | kubectl create -f -
 apiVersion: v1
 kind: ServiceAccount
 metadata:
   name: admin-user
   namespace: kube-system
EOF
```

<br/>
<br/>
- ~~ClusterRoleBinding을 생성~~
- 하단 코드 사용하지 않음

```bash
cat <<EOF | kubectl create --validate=false -f -
 apiVersion: rbac.authorization.k8s.io/v1
 kind: ClusterRoleBinding
 metadata:
   name: admin-user
 roleRef:
   apiGroup: rbac.authorization.k8s.io
   kind: ClusterRole
   name: cluster-admin
 subjects:
 - kind: ServiceAccount
   name: admin-user
   namespace: kube-system
EOF
```

<br/>
- 사용자 계정의 토큰을 가져와서 대시보드에 입력합니다.

```bash
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}')

Name:         admin-user-token-p9ldd
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=admin-user
              kubernetes.io/service-account.uid=041cb7ec-946a-49b6-8900-6dc90fc08464

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3Vu
```

<br/>
브라우저에 인증서 개인용, 신뢰할수 있는 기관 인증서 등록 후 실행하면 열림

https://[master_ip]:6443/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login

#### ![elasticsearch icon](/assets/images/icon/elastic-elasticsearch.png){:class="icon-title"} [Elasticsearch](/docker/elasticsearch)

# kubernetes dashboard - (metrics server) 설치

cpu, memory usage 를 확인하려면 metrics server 가 설치되어야한다

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

참고: [https://kangwoo.github.io/devops/kubernetes/kubernetes-metrics-server-install/](https://kangwoo.github.io/devops/kubernetes/kubernetes-metrics-server-install/)

<br/>
deployments 에 수정 tolerations 에 선언할게 있다면 추가해야한다


```text
schedulerName: default-scheduler
tolerations:
- key: [node-role.kubernetes.io/master](http://node-role.kubernetes.io/master)
effect: NoSchedule
- key: key
operator: Equal
value: api
effect: NoSchedule
```

deployments 에 수정 - '--kubelet-insecure-tls'

```
args:
- '--cert-dir=/tmp'
- '--secure-port=4443'
- '--kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname'
- '--kubelet-use-node-status-port'
- '--kubelet-insecure-tls'
```

#### ![certificate icon](/assets/images/icon/certificate-icon-png-10319.png){:class="icon-title"} [kubernetes - kubeadm 인증서](/kuebernetes/kubeadm-certificate)
