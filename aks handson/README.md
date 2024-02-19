setup the cluster in AKS

nodecount: 2

![Alt text](image.png)


it provides:
 - vmscaleset
 - nsg
 - route table
 - virtual network
 - metric alert rule
 - data collection endpoint
- data collection rule
- azure monitor workspace
- public ip address
- load balancer
- metric alert rule
- network watcher
- action group
- kubernetes service cluster
- managed identity( to orchestrate the vmss nodes)

verify if az cli and kubectl is installed

![Alt text](image-1.png)



connect to the cluster

az account set --subscription XXXX

az aks get-credentials --resource-group samplerg --name thulasiaks --overwrite-existing



check if the cluster is up and running

`kubectl get deployments --all-namespaces=true`
![Alt text](image-2.png)

check if the node pool is ready and running

`kubectl get nodes`
![Alt text](image-9.png)

# WRITING MANIFEST FILES

instead of passing arguments / cli while creating pod, deployment, replicaset, service....

we can write the definition of the instance using yaml manifest files

things are mandatory :
    
    - apiVersion
    - kind ( deployment, service, rs, pod)
    - metadata ( data about apps, label,)
    - spec ( container count, image, port)


like man/get-help, in kubernetes to get the details about definition/manifest

kubectl explain pod

![Alt text](image-7.png)  

kubectl explain deployment
![Alt text](image-8.png)



create a deployment
deploy.yml
 
 ![Alt text](image-5.png)

apply the deployment

`kubectl apply -f deploy.yml`


![Alt text](image-4.png)


check the status of the deployment

`kubectl get deployment`

![Alt text](image-10.png)


in kubernetes, when we create a deployment, by default it automatically creates a replica set for set, to maintain the desired configuration env.

to list the replicaset

`kubectl get replicaset`

![Alt text](image-11.png)

kubectl get pods

![Alt text](image-6.png)
![Alt text](image-12.png)


delete a pod and check if the desired state is retained automatically using replica set
`kubectl delete pod PODNAME`
 ![Alt text](image-14.png)


 to access the application, we need to expose the application using 
 # service


 ## CREATE A SERVICE

 service.yml

 ![Alt text](image-15.png)

 get the status of the service

 ![Alt text](image-16.png)


and try to access the application using loadbalancer IP and with the nodeport

![Alt text](image-17.png)

 # what if we need to change the deployment, from replica 5 to 15


 - just edit the deploy.yml and reapply

![Alt text](image-18.png)
![Alt text](image-19.png)



Delete a deployment

`kubectl delete deployment webdeploy`

![Alt text](image-21.png)


Delete a service

`kubectl delete service web-service`
![Alt text](image-22.png)

# dry run

- instead of we memorizing the yaml manifest / definition, copying it from the opensource / docs

we can use the dry run method and export the template to yaml



`kubectl create deploy mydeployment --image=nginx --port=80 --dry-run=client -o yaml > example-deploy.yaml`

![Alt text](image-20.png)


## - to generate the template for service

kubectl expose deploy



# scale up/down the deployment instead of reapplying the deployment

`kubectl scale deploy webdeploy --replicas=20`

![Alt text](image-23.png)


`kubectl scale deploy webdeploy --replicas=2`
 
 ![Alt text](image-24.png)


 # roll out new version

 `kubectl set image deployment webdeploy myweb=nginx:1.25.4-alpine `
 `kubectl set image deployment DEPLOYMENTNAME CONTAINERNAME=IMAGENAME`

 ![Alt text](image-25.png)


 to undo the changes





# deployment pods auto scaling

kubectl autoscale deploy webdeploy --min=2 --max=25 --cpu-percent=80

to get the list of autoscaling configuration


![Alt text](image-26.png)
