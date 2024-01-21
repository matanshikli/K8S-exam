1. Create a deployment called webapp with image nginx with 5 replicas a. Use the below command to create a yaml file. i.kubectl create deploy webapp --image=nginx --dry-run -o yaml > webapp.yaml  ii.Edit it and add 5 replica’s

kubectl create deploy webapp --image=nginx --dry-run=client -o yaml > webapp.yaml

and webapp.yaml

2. Get the deployment rollout status
kubectl rollout status deployment/webapp

deployment "webapp" successfully rolled out

3. Get the replicaset that created with this deployment
kubectl get replicaset -l app=webapp

NAME                DESIRED   CURRENT   READY   AGE
webapp-78d6769cf8   5         5         5       106s

4.  EXPORT the yaml of the replicaset and pods of this deployment 

kubectl get replicaset -l app=webapp -o yaml  > webapp-replicaset.yaml
kubectl get pods -l app=webapp -o yaml > webapp-pods.yaml
webapp-replicaset.yaml
webapp-pods.yaml

5. Delete the deployment you just created and watch all the pods are also being deleted
kubectl delete deployment webapp --cascade=orphan --wait=true

6. Create a deployment of webapp with image nginx:1.17.1 with container port 80 and verify the image version 
 . kubectl create deploy webapp --image=nginx:1.17.1 --dry-run -o yaml > webapp.yaml a. add the port section (80)  and create the deployment

 kubectl create deploy webapp --image=nginx:1.17.1 --dry-run -o yaml > webapp.yaml

 and 6-webapp.yaml
 kubectl describe deployment webapp | grep "Image:"
     Image:        nginx:1.17.1

7. Update the deployment with the image version 1.17.4 and verify

 kubectl describe deployment webapp | grep "Image:"

    Image:        nginx:1.17.4


8. Check the rollout history and make sure everything is ok after the update

kubectl rollout history deployment/webapp
kubectl rollout status deployment/webapp

9. Undo the deployment to the previous version 1.17.1 and verify Image has the previous version 

kubectl rollout undo deployment/webapp

kubectl describe deployment webapp | grep "Image:"
     Image:        nginx:1.17.1

10. Update the deployment with the wrong image version 1.100 and verify something is wrong with the deployment  . Expect: kubectl get pods (ImagePullErr) a.  Undo the deployment with the previous version and verify
kubectl get pods -l app=webapp

NAME                      READY   STATUS             RESTARTS   AGE
webapp-86c5d47987-tjqlj   0/1     ImagePullBackOff   0          87s
webapp-94c6fd8f8-b6kh2    1/1     Running            0          2m31s


a. kubectl rollout undo deployment/webapp

kubectl get pods -l app=webapp

NAME                      READY   STATUS    RESTARTS   AGE
webapp-7f88774d9b-2zbhq   1/1     Running   0          6s

b. kubectl rollout history deploy webapp --revision=7 

deployment.apps/webapp with revision #7
Pod Template:
  Labels:	app=webapp
	pod-template-hash=86c5d47987
  Containers:
   nginx:
    Image:	nginx:1.100
    Port:	80/TCP
    Host Port:	0/TCP
    Environment:	<none>
    Mounts:	<none>
  Volumes:	<none>

c.  Check the history of the specific revision of that deployment
kubectl rollout history deployment/webapp --revision=5

deployment.apps/webapp with revision #5
Pod Template:
  Labels:	app=webapp
	pod-template-hash=5768b68bf
  Containers:
   nginx:
    Image:	nginx:1.17.1
    Port:	80/TCP
    Host Port:	0/TCP
    Environment:	<none>
    Mounts:	<none>
  Volumes:	<none>

d. update the deployment with the image version latest and check the history and verify nothing is going on

in Yaml 6-webapp.yaml


11. Apply the autoscaling to this deployment with minimum 10 and maximum 20 replicas and target CPU of 85% and verify hpa is created and replicas are increased to 10 from 1

kubectl autoscale deployment webapp --cpu-percent=85 --min=10 --max=20

kubectl get pods -l app=webapp

NAME                      READY   STATUS    RESTARTS   AGE
webapp-84648db54b-47brb   1/1     Running   0          13s
webapp-84648db54b-5klbb   1/1     Running   0          13s
webapp-84648db54b-5tz8k   1/1     Running   0          13s
webapp-84648db54b-6qfxf   1/1     Running   0          13s
webapp-84648db54b-j8wcs   1/1     Running   0          13s
webapp-84648db54b-pngtp   1/1     Running   0          13s
webapp-84648db54b-rxvd2   1/1     Running   0          13s
webapp-84648db54b-szl9w   1/1     Running   0          13s
webapp-84648db54b-wqmkv   1/1     Running   0          4m32s
webapp-84648db54b-wr6sp   1/1     Running   0          13s


13. Clean the cluster by deleting deployment and hpa you just created

kubectl delete deployment webapp
kubectl delete hpa webapp


14.  Create a job and make it run 10 times one after one (run > exit > run >exit ..) using the following configuration: kubectl create job hello-job --image=busybox --dry-run -o yaml -- echo "Hello I am from job" > hello-job.yaml

in 14-hello-job.yaml










