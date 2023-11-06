---
category: K8s
tags: [CKA]
usemath: [latex, ascii] 
---

# Kubeadm 클러스터 업그레이드

## 추상적인 업그레이드 작업 절차

1. 기본 컨트롤 플레인 노드를 업그레이드
2. 추가 컨트롤 플레인 노드를 업그레이드
3. 워커 노드를 업그레이드



### 컨트롤 플레인 노드 업그레이드

컨트롤 플레인 노드의 업그레이드 절차는 한 번에 한 노드씩 실행해야 한다. 먼저 업그레이드 할 컨트롤 플레인 노드를 결정해야 한다. 



시작하기 전에

- Release Note를 주의 깊게 읽기
- 클러스터는 정적 컨트롤 플레인 및 etcd 파드 또는 외부 etcd를 사용해야 한다.
- 데이터베이스에 저장된 앱-레벨 상태와 같은 중요한 컴포넌트를 반드시 백업한다. <span style='background-color:#fff5b1'>kubeadm upgrade</span> 는 워크로드에 영향을 마치지 않고, 쿠버네티스 내부의 컴포넌트만 다루지만, 백업은 항상 모범 사례일 정도로 중요하다.
- 스왑을 비활성화해야 한다.

---

## 알아야 할 개념

### cordon

- 특정 노드를 스케쥴러에서 제외시켜 파드가 할당되지 않도록 함
- 기존에 노드에 배포된 파드는 그대로 남아있음

### drain

- 특정 노드를 스케쥴러에서 제외시켜 파드가 할당되지 않도록 하고, 기존에 배포된 파드를 다른 노드로 이동시킴
- <span style="color:red"> 노드를 업데이트하는 경우 활용 가능 </span>

---

## 컨트롤 플레인 노드

- kubeadm 업그레이드

```bash
$ apt update
$ apt-cache madison kubeadm
# 목록에서 최신 버전을 찾는다
```



- 다운로드하려는 버전이 잘 받아졌는지 확인한다.

```bash
$ kubeadm version
```



- 업그레이드 계획을 확인한다.

```bash
$ kubeadm upgrade plan
```

이 명령은 클러스터를 업그레이드 할 수 있는지를 확인하고, 업그레이드 할 수 있는 버전을 가져온다. 또한 컴포넌트 구성 버전 상태가 있는 표를 보여준다.



- 업그레이드할 버전을 선택하고, 적절한 명령을 실행한다. 예를 들면 다음과 같다.

```bash
# 이 업그레이드를 위해 선택한 패치 버전으로 x를 바꾼다.
$ sudo kubeadm upgrade apply v1.27.x
```



명령이 완료되면 다음을 확인해야 한다.

```
[upgrade/successful] SUCCESS! Your cluster was upgraded to "v1.27.x". Enjoy!

[upgrade/kubelet] Now that your control plane is upgraded, please proceed with upgrading your kubelets if you haven't already done so.
```



- CNI 제공자 플러그인을 수동으로 업데이트 한다. CNI 제공자가 Daemonset으로 실행되는 경우 추가 컨트롤 플레인 노드에는 이 단계가 필요하지 않다.



**다른 컨트롤 플레인 노드의 경우**: CKA에서는 단일 컨트롤 플레인 노드를 사용하기 때문에 생략



### 노드 드레인

- Unschedule로 표시하고 워크로드를 축출하여 유지 보수할 노드를 준비한다.

```bash
# <node-to-drain>을 드레인하는 노드의 이름으로 바꾼다.
kubectl drain <node-to-drain> --ignore-daemonsets
```



### kubectl과 kubelet 업그레이드

- 모든 컨트롤 플레인 노드에서 kubelet 및 kubedtl을 업그레이드한다.

```bash
# replace x in 1.27.x-00의 x를 최신 패치 버전으로 바꾼다
apt-mark unhold kubelet kubectl && \
apt-get update && apt-get install -y kubelet=1.27.x-00 kubectl=1.27.x-00 && \
apt-mark hold kubelet kubectl
```



- kubelet을 다시 시작한다.

```bash
$ sudo systemctl daemon-reload
$ sudo systemctl restart kubelet
```



### 노드 uncordon

- 노드를 schedulable로 표시하여 노드를 다시 온라인 상태로 전환한다.

```bash
# <node-to-uncordon>을 드레인하려는 노드의 이름으로 바꾼다.
$ kubectl uncordon <node-to-uncordon>
```



## 워커 노드

워커 노드의 업그레이드 절차는 워크로드를 실행하는 데 필요한 최소 용량을 보장하면서, 한 번에 하나의 노드 또는 한 번에 몇 개의 노드로 실행해야 한다.



### kubeadm 업그레이드

- 모든 워커 노드에서 kubeadm을 업그레이드한다.

```
# 1.27.x-00의 x를 최신 패치 버전으로 바꾼다
$ apt-mark unhold kubeadm && \
$ apt-get update && apt-get install -y kubeadm=1.27.x-00 && \
$ apt-mark hold kubeadm
```



### kubeadm upgrade 호출

- 워커 노드의 경우 로컬 kubelet 구성을 업그레이드한다.

```bash
$ sudo kubeadm upgrade node
```



### 노드 드레인

- Unschedulable로 표시하고 워크로드를 축출하여 유지 보수할 노드를 준비한다.

```bash
# <node-to-drain>을 드레인하는 노드의 이름으로 바꾼다.
kubectl drain <node-to-drain> --ignore-daemonsets
```



### kubelet과 kubectl 업그레이드

- kubelet과 kubectl을 업그레이드 한다.

```
# 1.27.x-00의 x를 최신 패치 버전으로 바꾼다
$ apt-mark unhold kubelet kubectl && \
$ apt-get update && apt-get install -y kubelet=1.27.x-00 kubectl=1.27.x-00 && \
$ apt-mark hold kubelet kubectl
```



- kubelet을 다시 시작한다.

```
$ sudo systemctl daemon-reload
$ sudo systemctl restart kubelet
```



### 노드에 적용된 cordon 해제

- Schedulable로 표시하여 노드를 다시 온라인 상태로 만든다.

```
# <node-to-uncordon>을 노드의 이름으로 바꾼다.
$ kubectl uncordon <node-to-uncordon>
```



### 클러스터 상태 확인

- 모든 노드에서 kubelet을 업그레이드한 후 kubectl이 클러스터에 접근할 수 있는 곳에서 다음의 명령을 실행하여 모든 노드를 다시 사용할 수 있는지 확인한다.

```
$ kubectl get nodes
```

---



### 특정 Pod를 controlplane 노드에서 실행시키기

```bash
root@controlplane: $ kubectl uncordon node01
root@controlplane: $ kubectl get pods -o wide | grep gold (make sure this is scheduled on a node)
```



