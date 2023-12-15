* Retrieve an authentication token and authenticate your Docker client to your registry.
Use the AWS CLI:
```
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 591121532214.dkr.ecr.ap-south-1.amazonaws.com
```
Note: If you receive an error using the AWS CLI, make sure that you have the latest version of the AWS CLI and Docker installed.

* Build your Docker image using the following command.
```
docker build -t robo-shop .
```

* After the build completes, tag your image so you can push the image to this repository:
```
docker tag robo-shop:latest 591121532214.dkr.ecr.ap-south-1.amazonaws.com/robo-shop:latest
```

* Run the following command to push this image to your newly created AWS repository:
```
docker push 591121532214.dkr.ecr.ap-south-1.amazonaws.com/robo-shop:latest
```