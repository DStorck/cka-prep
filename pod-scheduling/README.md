# Pod Scheduling

## Create 2 pod definitions: the second pod should be scheduled to run anywhere the first pod is running - 2nd pod runs alongside the first pod.

#### Create a pod with a specific label in the pod specification
```
apiVersion: v1
kind: Pod
metadata:
  name: security-supersecret
  labels:
    security: supersecret
spec:
  containers:
  - name: security-supersecret
    image: docker.io/ocpqe/hello-pod
```

#### When creating other pods, edit the pod specification as follows:
- Use the `podAffinity` stanza to configure the `requiredDuringSchedulingIgnoredDuringExecution` parameter or `preferredDuringSchedulingIgnoredDuringExecution` parameter:
- Specify the key and value that must be met. If you want the new pod to be scheduled with the other pod, use the same key and value parameters as the label on the first pod.
- Specify an operator. The operator can be `In`, `NotIn`,`Exists`, or `DoesNotExist`. For example, use the operator `In` to require the label to be in the node.
- Specify a `topologyKey`, which is a prepopulated Kubernetes label that the system uses to denote such a topology domain.


```
apiVersion: v1
kind: Pod
metadata:
  name: with-pod-affinity
spec:
  affinity:
    podAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchExpressions:
          - key: security
            operator: In
            values:
            - supersecret
        topologyKey: failure-domain.beta.kubernetes.io/zone
  ...
```


References:        
https://kubernetes.io/blog/2017/03/advanced-scheduling-in-kubernetes/
https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#inter-pod-affinity-and-anti-affinity-beta-feature
https://docs.openshift.org/latest/admin_guide/scheduling/pod_affinity.html#admin-guide-sched-affinity-examples1-pods
