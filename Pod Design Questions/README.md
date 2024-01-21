1. Type the command for:  Get pods with label information
 kubectl get pods --show-labels

2. Create 5 nginx pods in which two of them is labeled env=prod and three of them is labeled env=dev
for i in 1 2; do
    kubectl run nginx-prod-$i --image=nginx --labels="env=prod"
done

for i in 1 2 3; do
    kubectl run nginx-dev-$i --image=nginx --labels="env=dev"
done

3. Verify all the pods are created with correct labels
kubectl get pods --show-labels | grep "env="

nginx-dev-1                     1/1     Running   0               2m20s   env=dev
nginx-dev-2                     1/1     Running   0               2m20s   env=dev
nginx-dev-3                     1/1     Running   0               2m20s   env=dev
nginx-prod-1                    1/1     Running   0               2m22s   env=prod
nginx-prod-2                    1/1     Running   0               2m22s   env=prod



4. Get the pods with label env=dev
kubectl get pods -l env=dev

NAME          READY   STATUS    RESTARTS   AGE
nginx-dev-1   1/1     Running   0          3m45s
nginx-dev-2   1/1     Running   0          3m45s
nginx-dev-3   1/1     Running   0          3m45s

5. Get the pods with label env=dev and also output the labels

kubectl get pods -l env=dev --show-labels

NAME          READY   STATUS    RESTARTS   AGE     LABELS
nginx-dev-1   1/1     Running   0          5m27s   env=dev
nginx-dev-2   1/1     Running   0          5m27s   env=dev
nginx-dev-3   1/1     Running   0          5m27s   env=dev

6. Get the pods with label env=prod

NAME           READY   STATUS    RESTARTS   AGE
nginx-prod-1   1/1     Running   0          6m15s
nginx-prod-2   1/1     Running   0          6m15s

7. Get the pods with label env=prod and also output the labels

NAME           READY   STATUS    RESTARTS   AGE     LABELS
nginx-prod-1   1/1     Running   0          6m21s   env=prod
nginx-prod-2   1/1     Running   0          6m21s   env=prod


8.  Get the pods with label env
kubectl get pods -l env
NAME           READY   STATUS    RESTARTS   AGE
nginx-dev-1    1/1     Running   0          7m25s
nginx-dev-2    1/1     Running   0          7m25s
nginx-dev-3    1/1     Running   0          7m25s
nginx-prod-1   1/1     Running   0          7m27s
nginx-prod-2   1/1     Running   0          7m27s

9. Get the pods with labels env=dev and env=prod
kubectl get pods -l 'env in (prod, dev)'

NAME           READY   STATUS    RESTARTS   AGE
nginx-dev-1    1/1     Running   0          12m
nginx-dev-2    1/1     Running   0          12m
nginx-dev-3    1/1     Running   0          12m
nginx-prod-1   1/1     Running   0          12m
nginx-prod-2   1/1     Running   0          12m

10. Get the pods with labels env=dev and env=prod and output the labels as well

kubectl get pods -l 'env in (prod, dev)' --show-labels

NAME           READY   STATUS    RESTARTS   AGE   LABELS
nginx-dev-1    1/1     Running   0          13m   env=dev
nginx-dev-2    1/1     Running   0          13m   env=dev
nginx-dev-3    1/1     Running   0          13m   env=dev
nginx-prod-1   1/1     Running   0          13m   env=prod
nginx-prod-2   1/1     Running   0          13m   env=prod

11. Change the label for one of the pod to env=uat and list all the pods to verify 

kubectl label pod nginx-dev-3 env=uat --overwrite
kubectl get pods -l 'env in (uat, prod, dev)' --show-labels

NAME           READY   STATUS    RESTARTS   AGE   LABELS
nginx-dev-1    1/1     Running   0          15m   env=dev
nginx-dev-2    1/1     Running   0          15m   env=dev
nginx-dev-3    1/1     Running   0          15m   env=uat
nginx-prod-1   1/1     Running   0          15m   env=prod
nginx-prod-2   1/1     Running   0          15m   env=prod

12. Remove the labels for the pods that we created now and verify all the labels are removed
 kubectl label pods -l 'env in (prod, dev, uat)' env-


