# [Custom Resources](https://kubernetes.io/docs/tasks/access-kubernetes-api/custom-resources/custom-resource-definitions/)

## Create a custom resource definition and display it in the API with curl

#### create the crd 
```
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  # name must match the spec fields below, and be in the form: <plural>.<group>
  name: tacos.stable.example.com
spec:
  # group name to use for REST API: /apis/<group>/<version>
  group: stable.example.com
  # version name to use for REST API: /apis/<group>/<version>
  version: v1
  # either Namespaced or Cluster
  scope: Namespaced
  names:
    # plural name to be used in the URL: /apis/<group>/<version>/<plural>
    plural: tacos
    # singular name to be used as an alias on the CLI and for display
    singular: taco
    # kind is normally the CamelCased singular type. Your resource manifests use this.
    kind: Taco
    # shortNames allow shorter string to match your resource on the CLI
    shortNames:
    - tc
```

```
$ kubectl create -f crd.yaml
customresourcedefinition.apiextensions.k8s.io "tacos.stable.example.com" created
```
  
#### create and deploy a custom object for your new resource 
```
apiVersion: "stable.example.com/v1"
kind: Taco
metadata:
  name: my-new-taco-object
spec:
  image: my-delicious-taco
```

```
$ kubectl create -f taco-cr.yaml
taco.stable.example.com "my-new-taco-object" created
```
  
#### now you can check on your new taco 
```$ kubectl get taco
NAME                 AGE
my-new-taco-object   25s
```

#### or to see all the details
```$ k get taco
NAME                 AGE
my-new-taco-object   25s

$ kubectl get taco  -o yaml
apiVersion: v1
items:
- apiVersion: stable.example.com/v1
  kind: Taco
  metadata:
    clusterName: ""
    creationTimestamp: 2018-07-06T20:46:16Z
    generation: 1
    name: my-new-taco-object
    namespace: default
    resourceVersion: "514464"
    selfLink: /apis/stable.example.com/v1/namespaces/default/tacos/my-new-taco-object
    uid: a0f86fca-815d-11e8-af54-0283f356c4ba
  spec:
    image: my-delicious-taco
kind: List
metadata:
  resourceVersion: ""
  selfLink: ""
```

#### ssh to master node and check it out
```
$ curl http://localhost:8080/apis/stable.example.com/v1
{
  "kind": "APIResourceList",
  "apiVersion": "v1",
  "groupVersion": "stable.example.com/v1",
  "resources": [
    {
      "name": "tacos",
      "singularName": "taco",
      "namespaced": true,
      "kind": "Taco",
      "verbs": [
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "create",
        "update",
        "watch"
      ],
      "shortNames": [
        "tc"
      ]
    }
  ]
```