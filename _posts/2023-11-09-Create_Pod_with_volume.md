---
category: K8s
tags: [CKA]
usemath: [latex, ascii] 
---



> Create a Pod called <code>redis-storage</code> with iamge: <code>redis:alpine</code> with a Volume of type <code>emptyDir</code> that lasts for the life of the Pod.



# emptyDir

 For a Pod that defines an <code>emptyDir</code> volume, the volume is created when the Pod is assigned to a node. As the name says, the <code>emptyDir</code> volume is initially empty. All containers in the Pod can read and write the same files in the <code>emptyDir</code> volume, through that volume can be mounted at the same or different paths in each container. When a Pod is removed from a node for any reason, the data in the <code>emptyDir</code> is deleted permanently.

 Some uses for an <code>emptyDir</code> are:

- scratch space, such as for a disk-based merge sort
- checkpointing a long computation for recovery from crashes
- holding files that a content-manager container fetches while a webserver container serves the data

### emptyDir configuration example

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-pd
spec:
  containers:
  - image: registry.k8s.io/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /cache
      name: cache-volume
  volumes:
  - name: cache-volume
    emptyDir:
      sizeLimit: 500Mi
```

---



# 문제 풀이

1. emptyDir 없이 이미지를 --dry-run을 이용해 생성

```bash
kubectl run redis-storage --iamge=redis:alpine --dry-run=client -o yaml > <filename>.yaml
```



2. 쿠버네티스 공식 문서에서 emptyDir 검색 후 1에서 복사한 내용과 적절하게 조합한다.

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: redis-storage
  name: redis-storage
spec:
  containers:
  - image: redis:alpine
    name: redis-storage
    resources: {}
    volumeMounts:
    - mountPath: /data/redis
      name: redis-empty
  volumes:
  - name: redis-empty
    emptyDir:
      sizeLimit: 500Mi
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

