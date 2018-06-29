## Create and expose a deployment named nginx-hooray. Use the utility nslookup to ensure that the service and pod are accessible via their respective DNS records.

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
$ kubectl run busybox --image=busybox --rm --restart=OnFailure -ti -- /bin/nslookup nginx-hooray
Server:    10.152.183.134
Address 1: 10.152.183.134 kube-dns.kube-system.svc.cluster.local

Name:      nginx-hooray
Address 1: 10.152.183.173 nginx-hooray.default.svc.cluster.local
```