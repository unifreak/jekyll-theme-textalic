---
category: K8s
tags: [CKA]
usemath: [latex, ascii] 
---

## Deploy 생성하기

<mark>kubectl create</mark>를 이용하여 Deployment를 생성하고 <mark>--record</mark> 옵션을 이용하여 deployment에 있는 image를 업그레이드 한다.

```bash
$ kubectl create deployment  nginx-deploy --image=nginx:1.16
```



## Deployment 업그레이드 하기

<mark>nginx-deploy</mark>에 있는 이미지를 업데이트한다.

```bash
$ kubectl set image deployment/nginx-deploy nginx=nginx:1.17 --record
```



