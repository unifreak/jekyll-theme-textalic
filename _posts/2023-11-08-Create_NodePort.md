---
category: K8s
tags: [CKA]
usemath: [latex, ascii]
---



# NodePort

 If you set the <code>type</code> field to <code>NodePort</code>, the Kubernetes control plane allocates a port from a range specified by <code>--service-node-port-range</code> flag (default: 30000-32767). Each node proxies that port (the same port number on every Node) into your Service. Your Service reports the allocated port in its <code>.spec.ports[*].nodePort</code> field.

 Using a NodePort gives you the freedom to set up your own load balancing solution,  to configure environments that are not fully supported by Kubernetes, or even to expose one or more nodes'IP addresses directly.

 For a node port Service, Kubernetes additionally allocates a port (TCP, UDP or SCTP to match the protocol of the Service). Every node in the cluster configures itself to listen on that assigned port and to forward traffic to one of the ready endpoints associated with that Service. You'll be able to contact the <code>type: NodePort</code> Service, from outside the clsuter, by connecting to any node using the appropriate protocol and the appropriate port.



- yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  selector:
    app.kubernetes.io/name: MyApp
  ports:
    - port: 80
      # By default and for convenience, the `targetPort` is set to
      # the same value as the `port` field.
      targetPort: 80
      # Optional field
      # By default and for convenience, the Kubernetes control plane
      # will allocate a port from a range (default: 30000-32767)
      nodePort: 30007
```

