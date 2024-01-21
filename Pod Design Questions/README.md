# Pod Design Questions


1. Type the command for:  Get pods with label information

` kubectl get pods --show-labels
`

2. Create 5 nginx pods in which two of them is labeled env=prod and three of them is labeled env=dev

`for i in 1 2; do
    kubectl run nginx-prod-$i --image=nginx --labels="env=prod"
done`

`for i in 1 2 3; do
    kubectl run nginx-dev-$i --image=nginx --labels="env=dev"
done`

3. Verify all the pods are created with correct labels

`    kubectl get pods --show-labels | grep "env="
`

    nginx-dev-1                     1/1     Running   0               2m20s   env=dev
    nginx-dev-2                     1/1     Running   0               2m20s   env=dev
    nginx-dev-3                     1/1     Running   0               2m20s   env=dev
    nginx-prod-1                    1/1     Running   0               2m22s   env=prod
    nginx-prod-2                    1/1     Running   0               2m22s   env=prod



4. Get the pods with label env=dev

`    kubectl get pods -l env=dev
`

    NAME          READY   STATUS    RESTARTS   AGE
    nginx-dev-1   1/1     Running   0          3m45s
    nginx-dev-2   1/1     Running   0          3m45s
    nginx-dev-3   1/1     Running   0          3m45s

5. Get the pods with label env=dev and also output the labels

`kubectl get pods -l env=dev --show-labels
`

    NAME          READY   STATUS    RESTARTS   AGE     LABELS
    nginx-dev-1   1/1     Running   0          5m27s   env=dev
    nginx-dev-2   1/1     Running   0          5m27s   env=dev
    nginx-dev-3   1/1     Running   0          5m27s   env=dev

6. Get the pods with label env=prod

`    kubectl get pods -l env=prod
`

    NAME           READY   STATUS    RESTARTS   AGE
    nginx-prod-1   1/1     Running   0          6m15s
    nginx-prod-2   1/1     Running   0          6m15s


7. Get the pods with label env=prod and also output the labels

`kubectl get pods -l env=prod --show-labels
`

    NAME           READY   STATUS    RESTARTS   AGE     LABELS
    nginx-prod-1   1/1     Running   0          6m21s   env=prod
    nginx-prod-2   1/1     Running   0          6m21s   env=prod

8.  Get the pods with label env

`kubectl get pods -l env
`

    NAME           READY   STATUS    RESTARTS   AGE
    nginx-dev-1    1/1     Running   0          7m25s
    nginx-dev-2    1/1     Running   0          7m25s
    nginx-dev-3    1/1     Running   0          7m25s
    nginx-prod-1   1/1     Running   0          7m27s
    nginx-prod-2   1/1     Running   0          7m27s

9. Get the pods with labels env=dev and env=prod

`kubectl get pods -l 'env in (prod, dev)'
`

    NAME           READY   STATUS    RESTARTS   AGE
    nginx-dev-1    1/1     Running   0          12m
    nginx-dev-2    1/1     Running   0          12m
    nginx-dev-3    1/1     Running   0          12m
    nginx-prod-1   1/1     Running   0          12m
    nginx-prod-2   1/1     Running   0          12m

10. Get the pods with labels env=dev and env=prod and output the labels as well

`kubectl get pods -l 'env in (prod, dev)' --show-labels
`

    NAME           READY   STATUS    RESTARTS   AGE   LABELS
    nginx-dev-1    1/1     Running   0          13m   env=dev
    nginx-dev-2    1/1     Running   0          13m   env=dev
    nginx-dev-3    1/1     Running   0          13m   env=dev
    nginx-prod-1   1/1     Running   0          13m   env=prod
    nginx-prod-2   1/1     Running   0          13m   env=prod

11. Change the label for one of the pod to env=uat and list all the pods to verify 

`kubectl label pod nginx-dev-3 env=uat --overwrite
`


`kubectl get pods -l 'env in (uat, prod, dev)' --show-labels
`

    NAME           READY   STATUS    RESTARTS   AGE   LABELS
    nginx-dev-1    1/1     Running   0          15m   env=dev
    nginx-dev-2    1/1     Running   0          15m   env=dev
    nginx-dev-3    1/1     Running   0          15m   env=uat
    nginx-prod-1   1/1     Running   0          15m   env=prod
    nginx-prod-2   1/1     Running   0          15m   env=prod
    
12. Remove the labels for the pods that we created now and verify all the labels are removed

` kubectl label pods -l 'env in (prod, dev, uat)' env-
`

`kubectl get pods --show-labels | grep -E 'nginx-dev|nginx-prod'
`

    nginx-dev-1                     1/1     Running   0             21m    <none>
    nginx-dev-2                     1/1     Running   0             21m    <none>
    nginx-dev-3                     1/1     Running   0             21m    <none>
    nginx-prod-1                    1/1     Running   0             21m    <none>
    nginx-prod-2                    1/1     Running   0             21m    <none>


13.  Let’s add the label app=nginx for all the pods and verify 

`kubectl label pods --all app=nginx
`

    
    NAME                            READY   STATUS    RESTARTS      AGE    LABELS
    hr-web-app-7768c99859-5sznh     1/1     Running   0             114m   app=nginx,pod-template-hash=7768c99859
    hr-web-app-7768c99859-rbbsb     1/1     Running   0             114m   app=nginx,pod-template-hash=7768c99859
    messaging                       1/1     Running   0             133m   app=nginx,tier=msg
    multi-pod                       2/2     Running   0             28m    app=nginx
    nginx-critical                  1/1     Running   0             32m    app=nginx
    nginx-deploy-6cd9d98cfb-bg9n2   1/1     Running   0             76m    app=nginx,pod-template-hash=6cd9d98cfb
    nginx-dev-1                     1/1     Running   0             23m    app=nginx
    nginx-dev-2                     1/1     Running   0             23m    app=nginx
    nginx-dev-3                     1/1     Running   0             23m    app=nginx
    nginx-pod-matanshikli           1/1     Running   0             135m   app=nginx,run=nginx-pod-matanshikli
    nginx-prod-1                    1/1     Running   0             23m    app=nginx
    nginx-prod-2                    1/1     Running   0             23m    app=nginx
    nginx-resolver                  1/1     Running   0             43m    app=nginx
    redis-storage-matanshikli       1/1     Running   0             101m   app=nginx
    static-busybox                  1/1     Running   6 (13m ago)   113m   app=nginx,role=myrole
    use-pvspec-matanshikli          1/1     Running   0             96m    app=nginx,run=use-pv
    


14.  Get all the nodes with labels

` kubectl get nodes --show-labels
`

     NAME           STATUS   ROLES           AGE    VERSION   LABELS
    minikube       Ready    control-plane   35d    v1.28.3   beta.kubernetes.io/arch=arm64,beta.kubernetes.io/os=linux,kubernetes.io/arch=arm64,kubernetes.io/hostname=minikube,kubernetes.io/os=linux,minikube.k8s.io/commit=8220a6eb95f0a4d75f7f2d7b14cef975f050512d,minikube.k8s.io/name=minikube,minikube.k8s.io/primary=true,minikube.k8s.io/updated_at=2023_12_17T18_50_05_0700,minikube.k8s.io/version=v1.32.0,node-role.kubernetes.io/control-plane=,node.kubernetes.io/exclude-from-external-load-balancers=
    minikube-m02   Ready    <none>          137m   v1.28.3   beta.kubernetes.io/arch=arm64,beta.kubernetes.io/os=linux,kubernetes.io/arch=arm64,kubernetes.io/hostname=minikube-m02,kubernetes.io/os=linux,name=node01
    minikube-m03   Ready    <none>          33m    v1.28.3   beta.kubernetes.io/arch=arm64,beta.kubernetes.io/os=linux,kubernetes.io/arch=arm64,kubernetes.io/hostname=minikube-m03,kubernetes.io/os=linux

15. Label the worker node nodeName=nginxnode

`kubectl label nodes minikube-m03 nodeName=nginxnode
`

`kubectl get nodes -l nodeName=nginxnode
`

    NAME           STATUS   ROLES    AGE   VERSION
    minikube-m03   Ready    <none>   36m   v1.28.3

16. . Create a Pod that will be deployed on the worker node with the label nodeName=nginxnode

in yaml file `16-nginxnode.yaml`

17. Verify the pod that it is scheduled with the node selector on the right node... fix it if it’s not behind scheduled. 

`kubectl get pod nginx -o wide
`
    NAME    READY   STATUS    RESTARTS   AGE   IP           NODE           NOMINATED NODE   READINESS GATES
    nginx   1/1     Running   0          50s   10.244.2.9   minikube-m03   <none>           <none>


18. Verify the pod nginx that we just created has this label

`  kubectl get pod nginx --show-labels
`

    nginx   1/1     Running   0          2m22s   run=nginx
    

