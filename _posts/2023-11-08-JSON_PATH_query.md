---
category: K8s
tags: [CKA]
usemath: [latex, ascii]
---

> Use JSON PATH query to retrieve the <code>osImage</code>s of all the nodes and store it in a file <code>/opt/outputs/nodes_os_x43kj56.txt</code>.



# JSON PATH Query

Kubectl supports JSONPath template.

JSONPath template is composed of JSONPath expressions enclosed by curly braces {}. Kubectl uses JSONPath expressions to filter on specific fields in the JSON object and format the output. In addition to the original JSONPath template syntax, the following functions and syntax are valid:

1. Use double quotes to quote text inside JSONPath expressions.
2. Use the <code>range</code>, <code>end</code> operators to iterate lists.
3. Use negative slice indices to step backwards through a list. Negative indices do not "wrap around" a list and are valid as long as <code>-index + listLength >= 0</code>.



Example using <code>kubectl</code> and JSONPath expressions:

```bash
kubectl get pods -o json
kubectl get pods -o=jsonpath='{@}'
kubectl get pods -o=jsonpath='{.items[0]}'
kubectl get pods -o=jsonpath='{.items[0].metadata.name}'
kubectl get pods -o=jsonpath="{.items[*]['metadata.name', 'status.capacity']}"
kubectl get pods -o=jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.startTime}{"\n"}{end}'
kubectl get pods -o=jsonpath='{.items[0].metadata.labels.kubernetes\.io/hostname}'
```



위의 내용을 참고하여 아래의 커맨드를 입력한다.

```bash
$ kubectl get pods -o=jsonpath='{.items[*].metadata.labels.version}'

# Output
Ubuntu 18.04.5 LTS 
```



파일로 출력하기 위해 다음의 커맨드를 입력한다.

```bash
$ kubectl get pods -o=jsonpath='{.items[*].metadata.labels.version}' > /opt/outputs/nodes_os_x43kj56.txt
```

