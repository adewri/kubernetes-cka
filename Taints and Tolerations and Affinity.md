### Add Taint to Node:
`kubectl taint nodes node1 key1=value1:NoSchedule`
###Remove Taint from Node:
`kubectl taint nodes node1 key1=value1:NoSchedule-`
### Add Toleration to pod:

```coffee
tolerations:
- key: "key1"
  operator: "Equal"
  value: "value1"
  effect: "NoSchedule"
```

```coffee
tolerations:
- key: "key1"
  operator: "Exists"
  effect: "NoSchedule"
```

The default value for operator is Equal

Here's an example of a pod that uses tolerations:

```coffee
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  tolerations:
  - key: "example-key"
    operator: "Exists"
    effect: "NoSchedule"
```

For example, imagine you taint a node like this
`kubectl taint nodes node1 key1=value1:NoSchedule`
`kubectl taint nodes node1 key1=value1:NoExecute`
`kubectl taint nodes node1 key2=value2:NoSchedule`

And a pod has two tolerations:

```coffee
tolerations:
- key: "key1"
  operator: "Equal"
  value: "value1"
  effect: "NoSchedule"
- key: "key1"
  operator: "Equal"
  value: "value1"
  effect: "NoExecute"
```

Here's an example of a pod that uses tolerations and node affinity:

```coffee
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  tolerations:
  - key: "example-key"
    operator: "Exists"
    effect: "NoSchedule"
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: test-node-affinity
            operator: In
            values:
            - test
```
