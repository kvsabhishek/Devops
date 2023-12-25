# EKS

* ### Create EKS cluster using eksctl
```
eksctl create cluster --config-file=kube-cluster.yaml
```

* ### Delete EKS cluster
```
eksctl delete cluster --config-file=kube-cluster.yaml
```

# Authentication 

* ### To Check if you have access to create a service in k8s
```
kubectl auth can-i create <service_name>
```

* ### To check for specific user
```
kubectl auth can-i create <service_name> --as <user_name>
```

# Cluster level

* ### To check all the services and their api versions 
```
kubctl api-resources
```

* ### To see all the fields supported to write a manifest file of a service 
```
kubectl explain <service_name>
```

* ### To create a pod manifest file
```
kubectl run <pod_name> --image=<image_name> --dry-run=client -o yaml > My_ManiestFile.yml
```

* ### To edit the configuration on fly(after deployment)
```
kubectl edit <resource_type> <resource_name>
```
```
kubectl set <resource_variable> <resource_type> <resource_name> <value>
```

* ### To get all the created resources based on selectors
```
kubectl get all --selector <app=redis>
```

# NameSpaces

* ### To set default NameSpace
```
kubens <name-space>
```
```
kubectl config set-context --current --namespace=<name>
```

* ### To create quota in namespace
```
kubectl create quota <quota-name> --hard pod=3,cpu=100,memory=500mi -n <namespace>
```

# Deployment

* ### To scale a deployment 
```
kubectl scale deployment <deployment_name> --replicas=<count>
```
* ### To check the rollout history of a deployment
```
kubectl rollout history deployment <deployment_name>
```
### Deployment types:
* __Re-Create__ -> All pods will be deleted and recreated. Application will have downtime
* __Rolling Update__ -> Pods will be taken down one by one and created. Application will have minimum downtime. This is the default strategy used by Kubernates.
* __Blue/Green__ -> Both versions of the deployment will be running simeltaneously and we will shift the network from old deployment to new one.
* __Canary__ -> This is similar to Blue/Green deployment. But we will gradually route the traffic to new deployment. Increase the route traffic to new deployment and reduce to old deployment.

* ### To move to particular revision
```
kubectl rollout history deployment <deployment_name> --revision=<revision_number>
```
* ### To move to previous revision
```
kubectl rollout undo deployment <deployment_name>
```

# Troubleshooting

* If pod is not creating we can use __kubectl describe pod__ to find pod details and events.

* Check for all the events happening on the kubernates cluster using __kubectl get events__

* There might also be some issue with application side. To check application logs use __kubectl logs pod__

