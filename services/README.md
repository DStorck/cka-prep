## Create a service that uses an external load balancer and points to a 3 pod deployment running nginx.

`kubectl run nginx --image=nginx --replicas=3`


`kubectl expose deployment nginx --port=80 --name=my-nginx-svc --type=Loadbalancer`
