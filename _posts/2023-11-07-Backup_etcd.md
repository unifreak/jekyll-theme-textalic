---
category: K8s
tags: [CKA]
usemath: [latex, ascii]
---

# Back up ETCD 

## Snapshot using etcdctl options

Also take the snapshot using various options given by etcdctl.

```bash
$ ETCDCTL_API=3 etcdctl -h
```

will list various options available from etcdctl.

```bash
$ ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
  --cacert=<trusted-ca-file> --cert=<cert-file> --key=<key-file> \
  snapshot save <backup-file-location>
```

where <mark>trusted-ca-file</mark>, <mark>cert-file</mark>, and <mark>key-file</mark> can be obtained from the description of the etcd Pod.



CKA Lightning Lab에서는 다음과 같은 커맨드를 입력한다.

```bash
$ export ETCDCTL_API=3
etcdctl snapshot save --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key --endpoints=127.0.0.1:2379 /opt/etcd-backup.db
```

s

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

