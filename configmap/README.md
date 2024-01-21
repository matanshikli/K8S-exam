# configmap


1. Create a file called config.txt with two values key1=value1 and key2=value2 and verify the file

In file `config.txt `

2. Create a configmap named keyvalcfgmap and read data from the file config.txt and verify that configmap is created correctly

`kubectl create configmap keyvalcfgmap --from-file=config.txt
`

`kubectl get configmap keyvalcfgmap -o yaml > keyvalcfgmap.yaml
`

3. Create an nginx pod and load environment values from the above configmap keyvalcfgmap and exec into the pod and verify the environment variables and delete the pod // first run this command to save the pod yml kubectl run nginx --image=nginx --restart=Never --dry-run -o yaml > nginx-pod.yml

in file `nginx-pod.yml`

in file `keyvalcfgmap.yaml`


     kubectl exec -it nginx -- env | grep "key"
    
    config.txt=key1=value1
    key2=value2
    


