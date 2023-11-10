---
category: K8s
tags: [CKA]
usemath: [latex, ascii] 
---



# Static Pod

*Static Pods* are managed directly by the kubelet daemon on a specific node, without the API server observing them. Unlike Pods that are managed by the control plane; instead, the kubelet watches each static Pod (and restart it if it fails).

Static Pods are always bound to on Kubelet on a specific node.

The kubelet automatically tries to create a mirror Pod on the Kubernetes API server for each static Pod. This means that the Pods running on a node are visible on the API server, but cannot be controlled from there. The Pod names will be suffixed with the node hostname with a leading hyphen.



## Create a static pod

You can configure a static Pod with either a file system hosted configuration file or a web hosted configuration file.

### Filesystem-hosted static Pod manifest

Manifests are standard Pod definitions in JSON or YAML format in a specific directory. Use the `staticPodPath: <the directory>` field in the kubelet configuration file, which periodically scans the directory and creates/deletes static Pods as YAML/JSON files appear/disappear there. Note that the kubelet will ignore files starting with dots when scanning the specified directory.

This is how to start a simple web server as a static Pod:

1. Choose a node where you want to run the static Pod. In this example, it's `node01`

```bash
$ ssh node01
```



2. Choose a directory, `/etc/kubernetes/manifests` and place a Pod definition there, for example `/etc/kubernetes/manifests/static-pod.yaml`

```bash
 # Run this command on the node where kubelet is running
$ mkdir -p /etc/kubernetes/manifests/
cat <<EOF >/etc/kubernetes/manifests/static-web.yaml
apiVersion: v1
kind: Pod
metadata:
  name: static-web
  labels:
    role: myrole
spec:
  containers:
    - name: web
      image: nginx
      ports:
        - name: web
          containerPort: 80
          protocol: TCP
EOF
```

 