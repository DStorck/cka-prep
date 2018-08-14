# [Static pods](https://kubernetes.io/docs/tasks/administer-cluster/static-pod/#static-pod-creation) 

## Configure the kubelet systemd-managed service to launch pod with image nginx named turtle automatically. Put the spec files in the /etc/kubernetes/manifests directory on the node

- ssh to the desired worker node
- for a typical node, edit the `/etc/systemd/system/kubelet.service` file by adding `--pod-manifest-path /etc/kubernetes/manifests`
- add the following file to `/etc/kubernetes/manifests`
```
kind: Pod
metadata:
  name: turtle
spec:
  containers:
    - name: turtle
      image: nginx
```

then run the following commands to restart the kubelet service
```
$ systemctl daemon-reload
$ systemctl restart kubelet.service
```

** note about clusters brought up with juju

to have the same effect I had to navigate to `/etc/systemd/system/snap.kubelet.daemon.service` to find the WorkingDirectory ex `/var/snap/kubelet/411` , then go there to add the pod manifest path to the `args` file in that directory
