---

---

# Q1

Create a new service account with the name `pvviewer`. Grant this Service account access to `list` all PersistentVolumes in the cluster by creating an appropriate cluster role called `pvviewer-role` and ClusterRoleBinding called `pvviewer-role-binding`.
Next, create a pod called `pvviewer` with the image: `redis` and serviceAccount: `pvviewer` in the default namespace.

Pods authenticate to the API Server using ServiceAccounts. If the serviceAccount name is not specified, the default service account for the namespace is used during a pod creation.

Reference: `https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/`

Now, create a service account `pvviewer`:

```sh
kubectl create serviceaccount pvviewer
```

To create a clusterrole:

```sh
kubectl create clusterrole pvviewer-role --resource=persistentvolumes --verb=list
```

To create a clusterrolebinding:

```sh
kubectl create clusterrolebinding pvviewer-role-binding --clusterrole=pvviewer-role --serviceaccount=default:pvviewer
```

Solution manifest file to create a new pod called `pvviewer` as follows:

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: pvviewer
  name: pvviewer
spec:
  containers:
  - image: redis
    name: pvviewer
  # Add service account name
  serviceAccountName: pvviewer
```

---

# Q2

List the `InternalIP` of all nodes of the cluster. Save the result to a file `/root/CKA/node_ips`.

Answer should be in the format: `InternalIP of controlplane`<space>`InternalIP of node01` (in a single line)

- internalIP 를 확인하기 위해 아래와 같은 커맨드 입력

```bash
$ kubectl get nodes -o JSON | grep -i internaip
```

![image-20231113202657784](../assets/img/Mock_3/image-20231113202657784.png)



- internalIP 항목의 포함관계를 알아보기 위해 전 코드 100줄을 불러오는 커맨드 입력

```bash
$ kubectl get nodes -o json | grep -i internalip -B 100
```

![image-20231113202753860](../assets/img/Mock_3/image-20231113202753860.png)

위 코드로 items &rarr; status &rarr; address &rarr; InternalIP 임을 알 수 있다.



Explore the jsonpath loop.
`kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="InternalIP")].address}'` > /root/CKA/node_ips



JSON 쿼리를 해석하면

**'{.items[*].status.addresses[?(@.type=="InternalIP")].address}'**

- '{.items[*]}': 
  - 최상위 수준의 JSON 객체에서 'items' 배열의 모든 요소를 선택한다.
- '.status.addresses':
  - 각 노드의 'status' 객체 내에 있는 'addresses' 배열을 선택한다. 이 배열에는 노드의 여러 주소가 포함되어 있다.
- '[?(@.type=="InternalIP")]':
  - "addresses" 배열에서 'type' 속성이 'InternalIP'인 항목을 선택한다. 이것은 노드의 InternalIP 주소를 나타낸다.
- '.address': 
  - 선택된 "InternalIP" 항목에서 'address' 속성을 선택한다. 이것이 최종적으로 필터링 된 내부 IP 주소이다.

---

# Q3

Create a pod called `multi-pod` with two containers.
Container 1, name: `alpha`, image: `nginx`
Container 2: name: `beta`, image: `busybox`, command: `sleep 4800`

Environment Variables:
container 1:
`name: alpha`

Container 2:
`name: beta`

Solution manifest file to create a multi-container pod `multi-pod` as follows:

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: multi-pod
spec:
  containers:
  - image: nginx
    name: alpha
    env:
    - name: name
      value: alpha
  - image: busybox
    name: beta
    command: ["sleep", "4800"]
    env:
    - name: name
      value: beta
```

---

# Q4

Create a Pod called `non-root-pod` , image: `redis:alpine`

runAsUser: 1000

fsGroup: 2000

Solution manifest file to create a pod called `non-root-pod` as follows:

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: non-root-pod
spec:
  securityContext:
    runAsUser: 1000
    fsGroup: 2000
  containers:
  - name: non-root-pod
    image: redis:alpine
```

Verify the user and group IDs by using below command:

```
kubectl exec -it non-root-pod -- id
```

---

# Q5

We have deployed a new pod called `np-test-1` and a service called `np-test-service`. Incoming connections to this service are not working. Troubleshoot and fix it.
Create NetworkPolicy, by the name `ingress-to-nptest` that allows incoming connections to the service over port `80`.

Important: Don't delete any current objects deployed.

Solution manifest file to create a network policy `ingress-to-nptest` as follows:

```yaml
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: ingress-to-nptest
  namespace: default
spec:
  podSelector:
    matchLabels:
      run: np-test-1
  policyTypes:
  - Ingress
  ingress:
  - ports:
    - protocol: TCP
      port: 80
```

---

# Q6

Taint the worker node `node01` to be Unschedulable. Once done, create a pod called `dev-redis`, image `redis:alpine`, to ensure workloads are not scheduled to this worker node. Finally, create a new pod called `prod-redis` and image: `redis:alpine` with toleration to be scheduled on `node01`.

key: `env_type`, value: `production`, operator: `Equal` and effect: `NoSchedule`

To add taints on the `node01` worker node:

```sh
kubectl taint node node01 env_type=production:NoSchedule
```

Now, deploy `dev-redis` pod and to ensure that workloads are not scheduled to this `node01` worker node.

```sh
kubectl run dev-redis --image=redis:alpine
```

To view the node name of recently deployed pod:

```sh
kubectl get pods -o wide
```

Solution manifest file to deploy new pod called `prod-redis` with toleration to be scheduled on `node01` worker node.

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: prod-redis
spec:
  containers:
  - name: prod-redis
    image: redis:alpine
  tolerations:
  - effect: NoSchedule
    key: env_type
    operator: Equal
    value: production     
```

To view only `prod-redis` pod with less details:

```sh
kubectl get pods -o wide | grep prod-red
```

---

# Q7

Create a pod called `hr-pod` in `hr` namespace belonging to the `production` environment and `frontend` tier .
image: `redis:alpine`

Use appropriate labels and create all the required objects if it does not exist in the system already.

Create a namespace if it doesn't exist:

```shell
kubectl create namespace hr
```

and then create a `hr-pod` with given details:

```shell
kubectl run hr-pod --image=redis:alpine --namespace=hr --labels=environment=production,tier=frontend
```

---

# Q8

A kubeconfig file called `super.kubeconfig` has been created under `/root/CKA`. There is something wrong with the configuration. Troubleshoot and fix it.

Verify host and port for `kube-apiserver` are correct.

Open the `super.kubeconfig` in vi editor.

Change the 9999 port to 6443 and run the below command to verify:

```bash
kubectl cluster-info --kubeconfig=/root/CKA/super.kubeconfig
```

---

We have created a new deployment called `nginx-deploy`. scale the deployment to 3 replicas. Has the replica's increased? Troubleshoot the issue and fix it.

Use the command `kubectl scale` to increase the replica count to 3.

```sh
kubectl scale deploy nginx-deploy --replicas=3
```

The `controller-manager` is responsible for scaling up pods of a replicaset. If you inspect the control plane components in the `kube-system` namespace, you will see that the `controller-manager` is not running.

```sh
kubectl get pods -n kube-system
```

The command running inside the `controller-manager` pod is incorrect.
After fix all the values in the file and wait for `controller-manager` pod to restart.

Alternatively, you can run `sed` command to change all values at once:

```
sed -i 's/kube-contro1ler-manager/kube-controller-manager/g' /etc/kubernetes/manifests/kube-controller-manager.yaml
```

This will fix the issues in `controller-manager` yaml file.

At last, inspect the deployment by using below command:

```sh
kubectl get deploy
```