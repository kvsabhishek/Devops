## To Check if you have access to create a service in k8s
```
kubectl auth can-i create <service_name>
```
### To check for specific user
```
kubectl auth can-i create <service_name> --as <user_name>
```

## To check all the services and their api versions 
```
kubctl api-resources
```

