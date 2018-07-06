# [Configuring a pod to use a PV for storage](https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/)

### make a PV that is backed by physical storage. Do not associate the volume with any pod

```
kind: PersistentVolume
apiVersion: v1
metadata:
  name: my-pv
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"

```
### create the PVC, which gets automatically bound to a suitable PV
```
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: my-pvc   # this is what your pod will reference
spec:
  storageClassName: manual  # this will grab the specific PV with the corresponding storageClassName
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
```

### create a pod that uses the PVC as storage
```
kind: Pod
apiVersion: v1
metadata:
  name: task-pv-pod
spec:
  volumes:
    - name: my-pod-with-pv
      persistentVolumeClaim:
       claimName: my-pvc     # this must match the pvc .metadata.name
  containers:
    - name: task-pv-container
      image: nginx
      ports:
        - containerPort: 80
          name: "http-server"
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: my-pod-with-pv
```


[PV access modes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes) 
