# [dns](https://kubernetes.io/docs/tasks/administer-cluster/dns-debugging-resolution/) 

## Create and expose a deployment named nginx-hooray. Check accessibility of its dns records. 

### create deployment
```
$ kubectl run nginx-hooray --image=nginx --port=80
deployment "nginx-hooray" created
```

### back it by a service 
```
$ kubectl expose deployment nginx-hooray
service "nginx-hooray" exposed
```

### use nslookup to check accessibility 
```
$ kubectl run busybox --image=busybox:1.28 --rm --restart=OnFailure -ti -- /bin/nslookup nginx-hooray
Server:    10.152.183.134
Address 1: 10.152.183.134 kube-dns.kube-system.svc.cluster.local

Name:      nginx-hooray
Address 1: 10.152.183.173 nginx-hooray.default.svc.cluster.local
```

** There is an [issue](https://github.com/docker-library/busybox/issues/48) with Kubernetes and the latest version of busybox, for now you must use 1.27 or 1.28 for nslookup to work properly. 