# [Network Policy](https://kubernetes.io/docs/tasks/administer-cluster/declare-network-policy/)

### Create a network policy for an nginx pod such that only pods with the label access=granted can talk to it.

First create nginx deployment and expose it.      
`kubectl run nginx --image=nginx`      
`kubectl expose deployment nginx --port=80`

Test access.
```
$ kubectl run busybox --rm -ti --image=busybox /bin/sh
If you don't see a command prompt, try pressing enter.
/ # wget --spider --timeout=1 nginx
Connecting to nginx (10.152.183.242:80)
/ #
```
Limit access with the network policy.

```
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: access-nginx
spec:
  podSelector:
    matchLabels:
      run: nginx
  ingress:
  - from:
    - podSelector:
        matchLabels:
          access: "granted"
```

###  Create a busybox pod and attempt to talk to nginx - should be blocked

```
$ kubectl run busybox --rm -ti --image=busybox /bin/sh
If you don't see a command prompt, try pressing enter.
/ # wget --spider --timeout=1 nginx
Connecting to nginx (10.152.183.242:80)
wget: download timed out  <------ this is what we want
```

###  Attach the label to busybox and try again - should be allowed

```
$ kubectl run busybox --rm -ti --labels="access=granted" --image=busybox /bin/sh
If you don't see a command prompt, try pressing enter.
/ # wget --spider --timeout=1 nginx
Connecting to nginx (10.152.183.242:80)
/ #
```
