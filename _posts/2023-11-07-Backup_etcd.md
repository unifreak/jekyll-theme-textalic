---
category: K8s
tags: [CKA]
usemath: [latex, ascii]
---

# Backing up an etcd cluster

All Kubernetes objects are stored on etcd. Periodically backing up the etcd cluster data is important to recover Kubernetes under disaster scenarios, such as losing all control plane nodes. The snapshot file contains all the Kubernetes states and critical information. In order to keep the sensitive Kubernetes data safe, encrypt the snapshot files.

Backing up an etcd cluster can be accomplished in two ways: etcd built-in snapshot and volume snapshot.

## Built-in snapshot

 etcd supports built-in snapshot. A snapshot may either be taken from a live member with the <code>etcdctl snapshot save</code> command or by copying the <code>member/snap/db</code> file from an etcd data directory taht is not currently used by an etcd process. Taking the snapshot will not affect the performance of the member.

 Below is an example for taking a snapshot of the keyspace served by <code>$ENDPOINT</code> to the file <code>snapshot.db</code>:

```bash
ETCDCTL_API=3 etcdctl --endpoints $ENDPOINT snapshot save snapshot.db
```

## Volume snapshot

CKA에서 다루지 않으므로 생략함



### Snapshot using etcdctl options

We can also take the snapshot using various options given by etcdctl. For example

```bash
ETCDCTL_API=3 etcdctl -h
```

will list various options available from etcdctl. For example, you can take a snapshot by specifying the endpoint, certificates etc as shown below:

```bash
ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
  --cacert=<trusted-ca-file> --cert=<cert-file> --key=<key-file> \
  snapshot save <backup-file-location>
```



이 과정에서 <trusted-ca-file>, <cert-file>, <key-file>의 경로를 파악하기 위해 아래와 같은 커맨드를 사용한다.

```bash
cat /etc/kubernetes/manifests/etcd/yaml | grep file
```



또한, endpoints의 IP 주소를 얻기 위해 

```
vi /etc/kubernetes/manifests/etcd/yaml
```

실행 후, <code>--listen-client-urls</code>를 복사한다.



## Restoring an etcd cluster

Before starting the restore operation, a snapshot file must be present. It can either be a snapshot file from a previous <mark>backup</mark> operation, or from a remaining data directory.



Here is an example:

```bash
$ ETCDCTL_API=3 etcdctl --endpoints 10.2.0.9:2379 snapshot restore snapshot.db
```

Another example for restoring using <mark>etcdctl</mark> options:

```bash
$ ETCDCTL_API=3 etcdctl --data-dir <data-dir-location> snapshot restore snapshot.db
```

where <code><data-dir-location></code> is a directory that will be created during the restore process.



Yet another example would be to first export the <kbd>ETCDCTL_API</kbd> environment variable:

```bash
$ export ETCDCTL_API=3
etcdctl --data-dir <data-dir-location> snapshot restore snapshot.db
```

