# K8S-exam

1. Deploy a pod named nginx-pod using the nginx:alpine image. Name: nginx-pod-yourname Image: nginx:alpine 

`kubectl run nginx-pod-matanshikli --image=nginx:alpine
`
2. Deploy a messaging pod using the redis:alpine image with the labels set to tier=msg. Pod Name: messaging Image: redis:alpine Labels: tier=msg

`kubectl run messaging --image=redis:alpine --labels="tier=msg"
`
3.Create a namespace named apx-x998-yourname

`kubectl create namespace apx-x998-matanshikli
`
4. Get the list of nodes in JSON format and store it in a file at /tmp/nodes-yourname

`kubectl get nodes -o json > /tmp/nodes-matanshikli
`
5. Create a service messaging-service to expose the messaging application within the cluster on port 6379.

`kubectl expose pod messaging --name=messaging-service --type=ClusterIP --port=6379 --selector=app=messaging`

6. Create a service messaging-service to expose the messaging application within the cluster on port 6379. Service: messaging-service 
a. Port: 6379 
b. Type: ClusterIp 
c. Use the right labels

In yaml file `6-messaging-service.yaml`

7. Create a deployment named hr-web-app using the image kodekloud/webapp-color with 2 replicas. Name: hr-web-app 
a. Image: kodekloud/webapp-color 
b. Replicas: 2

In yaml file `7-hr-web-app.yaml`

8. Create a static pod named static-busybox on the master node that uses the busybox image and the command sleep 1000 Name: static-busybox 
a. Image: busybox 

In yaml file `8-static-busybox.yaml`

9. Create a POD in the finance-yourname namespace named temp-bus with the image redis:alpine  . Name: temp-bus a. Image Name: redis:alpine

`kubectl create namespace finance-matanshikli
`
`kubectl run temp-bus --image=redis:alpine -n finance-matanshikli
`
10. Create a Persistent Volume with the given specification. Volume Name: pv-analytics 
a. Storage: 100Mi 
b. Access modes: ReadWriteMany 
c. Host Path: /pv/data-analytics

In yaml file `10-pv-analytics.yaml`

11. Create a Pod called redis-storage-yourname with image: redis:alpine with a Volume of type emptyDir that lasts for the life of the Pod. specs:.
Pod named 'redis-storage-yourname'
a. Pod 'redis-storage-yourname' uses Volume type of emptyDir
b. Pod 'redis-storage-yourname' uses volumeMount with mountPath = /data/redis

In yaml file `11-redis-storage.yaml`

12. Create this pod and attached it a persistent volume called pv-1. Make sure the PV mountPath is hostbase : /data  

13. Create a new deployment called nginx-deploy, with image nginx:1.16 and 1 replica. Record the version. Next upgrade the deployment to version 1.17 using rolling update. Make sure that the version upgrade is recorded in the resource annotation.
a. Deployment : nginx-deploy. Image: nginx:1.16
b. Image: nginx:1.16 
c. Task: Upgrade the version of the deployment to 1:17 
d. Task: Record the changes for the image upgrade 

 In yaml file `13-nginx-deploy.yaml`

`kubectl rollout history deployment/nginx-deploy`

    deployment.apps/nginx-deploy
    REVISION  CHANGE-CAUSE
    1         nginx 1.16
    2         upgrade nginx to 1.17 


14. Create an nginx pod called nginx-resolver using image nginx, expose it internally with a service called nginx-resolver-service. Test that you are able to look up the service and pod names from within the cluster. Use the image: busybox:1.28 for dns lookup. Record results in /root/nginx-yourname.svc and /root/nginx-yourname.pod 

In yaml file `14-nginx-resolver.yaml`
`kubectl run busybox --image=busybox:1.28 --restart=Never --rm -it -- sh
`
And in files: nginx-matanshikli.svc, nginx-matanshikli.pod

15. Create a static pod on node01 called nginx-critical with image nginx. Create this pod on node01 and make sure that it is recreated/restarted automatically in case of a failure.

` kubectl label nodes minikube-m02 name=node01
`
 And in yaml file `15-nginx-critical.yaml`

16. Create a pod called multi-pod with two containers. Container 1, name: alpha, image: nginx Container 2: beta, image: busybox, command sleep 4800. a. Environment Variables: i.container 1: ii.name: alpha   iii.Container 2: iv.name: beta 

In yaml file `16-multi-pod.yaml`









