---
layout: views
title: Kubeadm-certificate
parent: kubernetes
nav_order: 
permalink: /kubernetes/kubeadm-certificate
---

# ![certificate icon](/assets/images/icon/certificate-icon-png-10319.png){:class="icon-title"} kubernetes - kubeadm 인증서

* [인증서 확인](#인증서-확인)
* [인증서 갱신](#인증서-갱신)

# 인증서 확인

```bash
sudo kubeadm certs check-expiration
```
/assets/images/views/kubernetes/kubeadm-certificate/
![certificate check](/assets/images/views/kubernetes/kubeadm-certificate/Untitled.png)

# 인증서 갱신

CERTIFICATE AUTHORITY 파일들은 갱신이 되지 않음 - 기간이 10년임...

```bash
sudo kubeadm certs renew all
```

![certificate renew all](/assets/images/views/kubernetes/kubeadm-certificate/Untitled1.png)