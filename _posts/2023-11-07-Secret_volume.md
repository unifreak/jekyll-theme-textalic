---
category: K8s
tags: [CKA]
usemath: [latex, ascii]
---



Use the command <code>kubectl run</code> to create a pod definition file. Add secret volume and update container name on it.

Alternatively, run the following command:

```
$ kubectl run secret-1401 -n admin1401 --iamge=busyboc --dry-run=client -oyaml --comand -- sleep 4800 > admin.yaml
```



Add the <mark>secret</mark> volume and mount path to create a pod called <mark>secret-1401</mark> in the <mark>admin1401</mark> namespaces as follows:

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: secret-1401
  name: secret-1401
  namespace: admin1401
spec:
  volumes:
  - name: secret-volume
    # secret volume
    secret:
      secretName: dotfile-secret
  containers:
  - command:
    - sleep
    - "4800"
    image: busybox
    name: secret-admin
    # volumes' mount path
    volumeMounts:
    - name: secret-volume
      readOnly: true
      mountPath: "/etc/secret-volume"
```

