# [secrets](https://kubernetes.io/docs/concepts/configuration/secret/)

### create a secret with name `apple` and keypair `banana`=`cherry`
`$ kubectl create secret generic apple --from-literal=banana=cherry`

### create a pod named `secret-file` using the redis image which mounts secret `apple` at `/secrets`
```
apiVersion: v1
kind: Pod
metadata:
  name: secret-file
spec:
  containers:
  - image: redis
    name: redis
    volumeMounts:
    - mountPath: /secrets
      name: foo
  volumes:
  - name: foo
    secret:
      secretName: apple
```

### Create a pod named secret-env using the redis image which exports `banana` as PASSWORD
```
apiVersion: v1
kind: Pod
metadata:
  name: secret-env
  namespace: default
spec:
  containers:
  - image: redis
    name: redis
    env:
    - name: PASSWORD
      valueFrom:
        secretKeyRef:
          name: apple
          key: banana
```
[Using secrets as files from a pod](https://kubernetes.io/docs/concepts/configuration/secret/#using-secrets-as-files-from-a-pod)       
[Using secrets as environment variables](https://kubernetes.io/docs/concepts/configuration/secret/#using-secrets-as-environment-variables)
