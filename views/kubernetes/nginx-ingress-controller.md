---
layout: views
title: Nginx-ingress-controller
parent: kubernetes
nav_order: 2
permalink: /kubernetes/nginx-ingress-controller
---

# ![nginx ingress controller icon](/assets/images/icon/nginx-ingress-controller.png){:class="icon-title"} nginx - ingress -controller

* [Ingress controller 실행](#ingress-controller-실행)

# Ingress controller 실행

출처 [https://kubernetes.github.io/ingress-nginx/deploy/](https://kubernetes.github.io/ingress-nginx/deploy/){:target="_blank"}

// bare metal ingress controller 설치 - aws, azure 등  따로 있음 설정 확인 필요
{:class="desc-text"}

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.44.0/deploy/static/provider/baremetal/deploy.yaml

```

<br/>
pod 을 master node 에 올리고 싶다면 pod 설정의  tolerations 에 추가 - ingress-nginx-admission-patch, ingress-nginx-admission-create, ingress-nginx-controller

```bash
  tolerations:
...
    - key: node-role.kubernetes.io/master
      effect: NoSchedule
...
```

<br/>
pod 을 master node 에 올리고 싶다면 service - service 설정의 ingress-nginx-controller에 externalIPs 추가 - master node 의 내부아이피 사용

```bash
spec:
...
  type: NodePort
  externalIPs:
    - 10.10.0.154
...
```