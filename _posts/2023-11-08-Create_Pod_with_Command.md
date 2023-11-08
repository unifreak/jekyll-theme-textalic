---
category: K8s
tags: [CKA]
usemath: [latex, ascii]
---

> Create a static pod named <kbd>static-busybox</kbd> on the controlplane node that uses the <kbd>busybox</kbd> image and the command <kbd>sleep 1000</kbd>.



# Static Pods

<mark> Static Pod</mark>s are managed directly by the kubelet daemon on a specific node, without the API server observing them. Whereas most Pods are managed by the control plane(for example, a Deployment), for <mark>statkc Pod</mark>s, the kubelet directly supervises each <mark>static Pod</mark>(and restarts it if it fails).

 <mark>Static Pod</mark>s are always bound to one Kubelet on a specific node. The main use for <mark>static Pod</mark>s is to run a self-hosted control plane: in other words, using the kubelet to supervise the individual control plane components.

 The kubelet automatically tries to create a mirror Pod on the Kubernetes API server for each <mark>static Pod</mark>s is to run a self-hosted control plane: in other words, using the kubelet to supervise the individual control plane components.

The kubelet automatically tries to create a mirror Pod on the Kubernetes API server for each <mark>static Pod</mark>. This means that the Pods running on a node are visible on the API server, but cannot be controlled from there.



## Create static Pods

### Filesystem-hosted static Pod manifest

Manifests are standard Pod definitions in JSON or YAML format in a specific directory. Use the <code>staticPodPath: <the directory> </code> field in the kubelet configuration file, which periodically scans the directory and creates/deletes static Pods as YAML/JSON files appear/disappear there. Note that the kubelet will ignore files starting with dots when scanning the specified directory.



Using command:

```bash
$ kubectl run static-busybox --image=busybox --dry-run=client -0 yaml --command -- sleep 1000 > static-busybox.yaml
```

- dry run: 모의 전투[실제 결과가 발생하지 않는다는 의미]

- <code>--dry-run=client</code>: Kubernetes API 서버에 어떠한 요청도 전송하지 않는다는 의미

  

```bash
$ cat static-busybox.yaml
$ mv static-busybox.yaml /etc/kubernetes/manifests/
# yaml 파일 출력을 복사해서 /etc/kubernetes/manifests의 디렉토리에 yaml 파일로 저장한다. 
# Apply 커맨드를 실행하지 않아도 mv 커맨드를 이용해 디렉토리에 옮기면 Pod가 자동으로 생성된다.
```





