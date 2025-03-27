# Go WebApp with Dockerized Image and Deployed in Kubernetes

## This is how it looks once setup is done completely

![Video Demo](./devops-project-ss.gif)


## Steps to prepare the project :-

### Golang Setup

- After installation of Golang, verify the variables _GOROOT_, _GOPATH_, _GOBIN_ are set or not.

  ```bash
  LoneRanger@DESKTOP-AM16DR1 MINGW64 ~
  $ echo $GOROOT

  LoneRanger@DESKTOP-AM16DR1 MINGW64 ~
  $ echo $GOPATH
  C:\Users\LoneRanger\go

  LoneRanger@DESKTOP-AM16DR1 MINGW64 ~
  $ echo $GOBIN
  
  ```


- Create a basic folder structure for a Go webapp and add the location of this path in _GOPATH_ in _System Environment Variables_. Open a new terminal session to verify whether the paths for the variables are set properly or not. 

  ```bash
  LoneRanger@DESKTOP-AM16DR1 MINGW64 /e/DevOps projects/Go WebApp with Dockerized Image and Deployed in Kubernetes
  $ echo $GOPATH
  E:\DevOps projects\Go WebApp with Dockerized Image and Deployment on Kubernetes\go-devops

  LoneRanger@DESKTOP-AM16DR1 MINGW64 /e/DevOps projects/Go WebApp with Dockerized Image and Deployed in Kubernetes
  $ echo $GOBIN
  E:\DevOps projects\Go WebApp with Dockerized Image and Deployed in Kubernetes\go-devops\bin

  ```

### Container Setup

- Once the code is done then build the project. Create a _dockerignore_ file and then create the docker file to dockerize it.
- Add content to the _dockerfile_ and meanwhile start the _minikube cluster_ too.
- Build the container using docker file, run it and verify from the logs.

  ```bash
  LoneRanger@DESKTOP-AM16DR1 MINGW64 /e/DevOps projects/Go WebApp with Dockerized Image and Deployment on Kubernetes/go-devops/src/go-microservices
  $ docker build -t go-app:v1 .
  [+] Building 17.3s (11/11) FINISHED
  => [internal] load build definition from Dockerfile                         1.5s 
  => => transferring dockerfile: 849B                                         0.7s 
  => [internal] load .dockerignore                                            0.7s 
  => => transferring context: 34B                                             0.2s 
  => [internal] load metadata for docker.io/library/golang:latest             9.7s 
  => [1/6] FROM docker.io/library/golang:latest@sha256:d3ca1795fc42c82a42442  0.1s 
  => => resolve docker.io/library/golang:latest@sha256:d3ca1795fc42c82a42442  0.1s 
  => [internal] load build context                                            0.3s 
  => => transferring context: 1.21kB                                          0.1s 
  => CACHED [2/6] WORKDIR /app                                                0.0s 
  => CACHED [3/6] COPY go.mod go.sum ./                                       0.0s 
  => CACHED [4/6] RUN go mod download                                         0.0s 
  => [5/6] COPY . .                                                           0.6s 
  => [6/6] RUN go build -o main .                                             4.1s 
  => exporting to image                                                       0.5s 
  => => exporting layers                                                      0.4s 
  => => writing image sha256:b30cb0fff7db468b34e2cc88231996c5386cbfffa648018  0.0s 
  => => naming to docker.io/library/go-app:v1                          0.0s

  LoneRanger@DESKTOP-AM16DR1 MINGW64 /e/DevOps projects/Go WebApp with Dockerized Image and Deployment on Kubernetes/go-devops/src/go-microservices
  $ docker run -d -p 80:80 --name go-app go-app:v1
  41b37f237085134cbf803db947c3ada4e0e031696355c48728e0031cfc69fbcb

  LoneRanger@DESKTOP-AM16DR1 MINGW64 /e/DevOps projects/Go WebApp with Dockerized Image and Deployment on Kubernetes/go-devops/src/go-microservices
  $ docker ps 
  CONTAINER ID   IMAGE       COMMAND    CREATED          STATUS          PORTS                NAMES 
  41b37f237085   go-app:v1   "./main"   24 seconds ago   Up 13 seconds   0.0.0.0:80->80/tcp   go-app         
 
  LoneRanger@DESKTOP-AM16DR1 MINGW64 /e/DevOps projects/Go WebApp with Dockerized Image and Deployment on Kubernetes/go-devops/src/go-microservices
  $ docker logs go-app
  2025/03/02 16:27:53 Web server has started at http://localhost   

  LoneRanger@DESKTOP-AM16DR1 MINGW64 /e/DevOps projects/Go WebApp with Dockerized Image and Deployment on Kubernetes/go-devops/src/go-microservices
  $ docker logs -f go-app
  2025/03/02 16:27:53 Web server has started at http://localhost
  2025/03/02 16:30:24 Homepage available at http://localhost/
  2025/03/02 16:30:47 Fetching the details http://localhost/details
  41b37f237085 172.17.0.2                      

  ```

- If it gives the expecetd results in the proper API endpoints then setup  a _docker-compose.yaml_ file and add the content required to create a docker container with dockerfile. Build the app, run it using the compose file.

  ```bash
  LoneRanger@DESKTOP-AM16DR1 MINGW64 /e/DevOps projects/Go WebApp with Dockerized Image and Deployment on Kubernetes/go-devops/src/go-microservices
  $ docker-compose build
  [+] Building 23.2s (11/11) FINISHED
  => [internal] load build definition from dockerfile                         2.4s 
  => => transferring dockerfile: 849B                                         1.4s 
  => [internal] load .dockerignore                                            2.3s 
  => => transferring context: 65B                                             0.8s 
  => [internal] load metadata for docker.io/library/golang:latest            14.3s 
  => [1/6] FROM docker.io/library/golang:latest@sha256:d3ca1795fc42c82a42442  0.5s 
  => => resolve docker.io/library/golang:latest@sha256:d3ca1795fc42c82a42442  0.5s 
  => [internal] load build context                                            4.4s 
  => => transferring context: 1.99MB                                          3.8s 
  => CACHED [2/6] WORKDIR /app                                                0.0s 
  => CACHED [3/6] COPY go.mod go.sum ./                                       0.0s 
  => CACHED [4/6] RUN go mod download                                         0.0s 
  => CACHED [5/6] COPY . .                                                    0.0s 
  => CACHED [6/6] RUN go build -o main .                                      0.0s 
  => exporting to image                                                       0.2s 
  => => exporting layers                                                      0.0s 
  => => writing image sha256:b30cb0fff7db468b34e2cc88231996c5386cbfffa648018  0.0s 
  => => naming to docker.io/library/go-app:v1                          0.0s

  Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them

  LoneRanger@DESKTOP-AM16DR1 MINGW64 /e/DevOps projects/Go WebApp with Dockerized Image and Deployment on Kubernetes/go-devops/src/go-microservices
  $ docker-compose up -d
  [+] Running 1/1
  ✔ Container go-microservices-web-1  Started                                                                                                                                                                   0.7s 

  LoneRanger@DESKTOP-AM16DR1 MINGW64 /e/DevOps projects/Go WebApp with Dockerized Image and Deployment on Kubernetes/go-devops/src/go-microservices
  $ docker ps
  CONTAINER ID   IMAGE       COMMAND    CREATED          STATUS         PORTS                NAMES
  34ecdf2bdc1a   go-app:v1   "./main"   52 minutes ago   Up 6 seconds   0.0.0.0:80->80/tcp   go-microservices-web-1 

  LoneRanger@DESKTOP-AM16DR1 MINGW64 /e/DevOps projects/Go WebApp with Dockerized Image and Deployment on Kubernetes/go-devops/src/go-microservices
  $ docker-compose stop
  [+] Stopping 1/1
  ✔ Container go-microservices-web-1  Stopped 


  ```

### Minikube Deployment

-  Run the `eval $(minikube docker-env)` command to returns a set of Bash environment variable exports to configure your local environment to re-use the Docker daemon inside the Minikube instance.

  ```bash
  LoneRanger@DESKTOP-AM16DR1 MINGW64 /e/DevOps projects/Go WebApp with Dockerized Image and Deployed in Kubernetes
  $ minikube status

  minikube
  type: Control Plane   
  host: Running
  kubelet: Running      
  apiserver: Running    
  kubeconfig: Configured

  ```

- Verify again whether the API endpoints are giving results or not. I yes then stop docker compose and prepare for the minikube deployment.Make a folder to hold all the kubernetes deployment objects, create abd add content to _deployment.yml and service.yml_, apply those files, verify the deployment

  ```bash
  LoneRanger@DESKTOP-AM16DR1 MINGW64 /e/DevOps projects/Go WebApp with Dockerized Image and Deployment on Kubernetes/go-devops/src/go-microservices
  $ kubectl get all
  NAME                                  READY   STATUS    RESTARTS       AGE
  pod/web-deployment-547978996b-n5f5t   1/1     Running   1 (8m2s ago)   34m
  pod/web-deployment-547978996b-wzwc7   1/1     Running   1 (8m2s ago)   34m

  NAME                  TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
  service/kubernetes    ClusterIP      10.96.0.1       <none>        443/TCP        66d
  service/nginxapp      LoadBalancer   10.106.96.107   <pending>     80:30958/TCP   66d
  service/web-service   NodePort       10.100.208.86   <none>        80:30688/TCP   34m

  NAME                             READY   UP-TO-DATE   AVAILABLE   AGE
  deployment.apps/web-deployment   2/2     2            2           34m

  NAME                                        DESIRED   CURRENT   READY   AGE
  replicaset.apps/web-deployment-547978996b   2         2         2       34m

  LoneRanger@DESKTOP-AM16DR1 MINGW64 /e/DevOps projects/Go WebApp with Dockerized Image and Deployment on Kubernetes/go-devops/src/go-microservices
  $ minikube service web-service
  W0303 19:04:49.821296    6444 main.go:291] Unable to resolve the current Docker CLI context "default": context "default": context not found: open C:\Users\LoneRanger\.docker\contexts\meta\37a8eec1ce19687d132fe29051dca629d164e2c4958ba141d5f4133a33f0688f\meta.json: The system cannot find the path specified.
  |-----------|-------------|-------------|---------------------------|
  | NAMESPACE |    NAME     | TARGET PORT |            URL            |
  |-----------|-------------|-------------|---------------------------|
  | default   | web-service |          80 | http://192.168.49.2:30688 |
  |-----------|-------------|-------------|---------------------------|
  🏃  Starting tunnel for service web-service.
  |-----------|-------------|-------------|------------------------|
  | NAMESPACE |    NAME     | TARGET PORT |          URL           |
  |-----------|-------------|-------------|------------------------|
  | default   | web-service |             | http://127.0.0.1:57032 |
  |-----------|-------------|-------------|------------------------|
  🎉  Opening service default/web-service in default browser...
  ❗  Because you are using a Docker driver on windows, the terminal needs to be open to run it.
  ```
