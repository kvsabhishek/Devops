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

### To Check if you have access to create a service in k8s
```
kubectl auth can-i create <service_name>
```

### To check for specific user
```
kubectl auth can-i create <service_name> --as <user_name>
```

# Cluster level

## To check all the services and their api versions 
```
kubctl api-resources
```

### To see all the fields supported to write a manifest file of a service 
```
kubectl explain <service_name>
```

### To create a pod manifest file
```
kubectl run <pod_name> --image=<image_name> --dry-run=client -o yaml > My_ManiestFile.yml
```

# NameSpaces

* ## To set default NameSpace
```
kubens <name-space>
```
```
kubectl config set-context --current --namespace=<name>
```

* ## To create quota in namespace
```
kubectl create quota <quota-name> --hard pod=3,cpu=100,memory=500mi -n <namespace>
```

# Troubleshooting

* If pod is not creating we can use __kubectl describe pod__ to find pod details and events.

* Check for all the events happening on the kubernates cluster using __kubectl get events__

* There might also be some issue with application side. To check application logs use __kubectl logs pod__