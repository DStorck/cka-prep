# [updating a deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#updating-a-deployment)

### Create a deployment running nginx version 1.12.2 that will run in 2 pods
 1. Scale this to 4 pods.
 2. Scale it back to 2 pods.
 3. Upgrade this to 1.13.8 and record the update
 4. rollback to previous version
 5. show the rollout history


create the deployment   
`kubectl run nginx --image=nginx:1.12.2 --replicas=2`

scale it up    
`kubectl scale deployment nginx --replicas=4 --record `

scale it back to 2     
`kubectl scale deployment nginx --replicas=2 --record `

upgrade to 1.13.8     
`kubectl set image deployment nginx nginx=nginx:1.13.8 --record`

rollback to previous version     
`kubectl rollout undo deployment nginx --record`

check rollout history        
`kubectl rollout history deployment nginx`
