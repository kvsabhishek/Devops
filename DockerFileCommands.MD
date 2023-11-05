#### FROM - Used to specify the base image. Executes at image time.
```
FROM almalinux:8
```

#### LABEL - Used to add labels to the image. Can be used at the time filtering the images. Executes at image time.
```
LABEL COURSE=DevOps \
      TRAINER=Abhishek \
      DURATION=100  
```

#### USER - By default containers run as root user. But this is not recommended due to security issues. If anyone can enter into our container they can have root access. For this we use USER. Container will run with specific user mentioned.
```
FROM almalinux:8
RUN useradd sivakumar
USER sivakumar
```

#### ENV - Used to set environment variables inside continer.
```
ENV MY_ENV_VAR=my_value
```

#### ARG - Defines variables at build time for the Docker image.
```
ARG MY_BUILD_ARG=default_value
ENV MY_ENV_VAR=$MY_BUILD_ARG
RUN echo "MY_ENV_VAR is set to $MY_ENV_VAR"
```

#### WORKDIR - Used to create and set the working directory where all the tasks in the continer execute.
```
WORKDIR /app
```

#### COPY - Copies the files from our system to image at the time of creating image.
```
COPY index.html /app/
```

#### ADD - Same as COPY. But adds two more features.
* Copies content directly from internet to image.
* Unzip the files directly to the image.
```
ADD app.zip /app/app_files
ADD https://raw.githubusercontent.com/kvsabhishek/shell-script/main/package_install.sh ./package_install.sh
```

#### RUN - Used to run any tasks inside the container. Executes as container time.
```
RUN apt-get update && apt-get install -y nginx
```

#### EXPOSE - Doesnot add any functionality to the image. Just used to give information on which port the continer is active.
```
EXPOSE 80/tcp
```

#### CMD - Specify the commad to be executed when a container is run.
```
CMD ["nginx", "-g", "daemon off;"]
```

#### ENTRYPOINT - Specify the default commad to be executed when a container is run.
```
ENTRYPOINT ["/bin/sh", "-c"]
```
### CMD vs ENTRYPOINT
* ENTRYPOINT is the default command that executes in container.
* CMD is the command that will be replaced with the command used at the time for container creation.
* Container execution will be **ENTRYPOINT + CMD**

#### ONBUILD - If we want any command to run at the time of container creatiom instead of running at the time of image creation we use this.
```
FROM almalinux:8
RUN yum install nginx -y
RUN rm -rf /usr/share/nginx/html/index.html
ONBUILD ADD index.html /usr/share/nginx/html/index.html
CMD [ "nginx", "-g", "daemon off;" ]
```
* index.html file will be added to container at the time of container creating.