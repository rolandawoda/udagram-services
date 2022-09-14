# Udagram Image Filtering Application

Udagram is a simple cloud application developed alongside the Udacity Cloud Engineering Nanodegree. It allows users to register and log into a web client, post photos to the feed, and process photos using an image filtering microservice.

The project is split into two parts:

1. Frontend - Angular web application built with Ionic Framework
2. Backend RESTful API - Node-Express application

## Getting Started

> _tip_: it's recommended that you start with getting the backend API running since the frontend web application depends on the API.

### Prerequisite

1. The depends on the Node Package Manager (NPM). You will need to download and install Node from [https://nodejs.com/en/download](https://nodejs.org/en/download/). This will allow you to be able to run `npm` commands.
2. Environment variables will need to be set. These environment variables include database connection details that should not be hard-coded into the application code.

#### Environment Script

A file named `set_env.sh` has been prepared as an optional tool to help you configure these variables on your local development environment.

We do _not_ want your credentials to be stored in git. After pulling this `starter` project, run the following command to tell git to stop tracking the script in git but keep it stored locally. This way, you can use the script for your convenience and reduce risk of exposing your credentials.
`git rm --cached set_env.sh`

Afterwards, we can prevent the file from being included in your solution by adding the file to our `.gitignore` file.

### 1. Database

Create a PostgreSQL database either locally or on AWS RDS. The database is used to store the application's metadata.

- We will need to use password authentication for this project. This means that a username and password is needed to authenticate and access the database.
- The port number will need to be set as `5432`. This is the typical port that is used by PostgreSQL so it is usually set to this port by default.

Once your database is set up, set the config values for environment variables prefixed with `POSTGRES_` in `set_env.sh`.

- If you set up a local database, your `POSTGRES_HOST` is most likely `localhost`
- If you set up an RDS database, your `POSTGRES_HOST` is most likely in the following format: `***.****.us-west-1.rds.amazonaws.com`. You can find this value in the AWS console's RDS dashboard.

### 2. S3

Create an AWS S3 bucket. The S3 bucket is used to store images that are displayed in Udagram.

Set the config values for environment variables prefixed with `AWS_` in `set_env.sh`.

### 3. Backend API

Launch the backend API locally. The API is the application's interface to S3 and the database.

- To download all the package dependencies, run the command from the directory `udagram-api/`:
  ```bash
  npm install .
  ```
- To run the application locally, run:
  ```bash
  npm run dev
  ```
- You can visit `http://localhost:8080/api/v0/feed` in your web browser to verify that the application is running. You should see a JSON payload. Feel free to play around with Postman to test the API's.

### 4. Frontend App

Launch the frontend app locally.

- To download all the package dependencies, run the command from the directory `udagram-frontend/`:
  ```bash
  npm install .
  ```
- Install Ionic Framework's Command Line tools for us to build and run the application:
  ```bash
  npm install -g ionic
  ```
- Prepare your application by compiling them into static files.
  ```bash
  ionic build
  ```
- Run the application locally using files created from the `ionic build` command.
  ```bash
  ionic serve
  ```
- You can visit `http://localhost:8100` in your web browser to verify that the application is running. You should see a web interface.

## Tips

1. Take a look at `udagram-api` -- does it look like we can divide it into two modules to be deployed as separate microservices?
2. The `.dockerignore` file is included for your convenience to not copy `node_modules`. Copying this over into a Docker container might cause issues if your local environment is a different operating system than the Docker image (ex. Windows or MacOS vs. Linux).
3. It's useful to "lint" your code so that changes in the codebase adhere to a coding standard. This helps alleviate issues when developers use different styles of coding. `eslint` has been set up for TypeScript in the codebase for you. To lint your code, run the following:
   ```bash
   npx eslint --ext .js,.ts src/
   ```
   To have your code fixed automatically, run
   ```bash
   npx eslint --ext .js,.ts src/ --fix
   ```
4. `set_env.sh` is really for your backend application. Frontend applications have a different notion of how to store configurations. Configurations for the application endpoints can be configured inside of the `environments/environment.*ts` files.
5. In `set_env.sh`, environment variables are set with `export $VAR=value`. Setting it this way is not permanent; every time you open a new terminal, you will have to run `set_env.sh` to reconfigure your environment variables. To verify if your environment variable is set, you can check the variable with a command like `echo $POSTGRES_USERNAME`.

## Docker build all service

    docker-compose -f docker-compose-build.yaml build --parallel

     docker compose --env-file ./.env up

    http://localhost:8100

docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname

## AWS Kubernetes

aws sts get-caller-identity
kubectl config view --minify
kubectl get svc
kubectl get nodes
kubectl get nodes -o wide

kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

kubectl replace --force -f reverseproxy.yaml

kubectl get pods
kubectl get pods -o wide
kubectl get pods --watch
kubectl get pods -n kube-system
kubectl logs podname -p
kubectl describe pod podname

kubectl get deployment -o wide
kubectl get deployments

kubectl autoscale deployment backend-user --cpu-percent=70 --min=3 --max=5
kubectl get hpa

kubectl get services
kubectl describe services
{
Name: my-app,
Type: ClusterIP
}
kubectl cluster info dump

connect to pod
kubectl exec -it {POD NAME} bash
curl http://my-app2-svc:8080/health

To attach to a container in a pod, we can use the following command
kubectl exec -it {pod_name} sh

# The command below will ceates an external load balancer and assigns a fixed, external IP to the Service.

kubectl expose deployment frontend --type=LoadBalancer --name=publicfrontend

Check the deployment names and their pod status
kubectl get deployments

Create a Service object that exposes the frontend deployment
The command below will ceates an external load balancer and assigns a fixed, external IP to the Service.
kubectl expose deployment frontend --type=LoadBalancer --name=publicfrontend
kubectl expose deployment reverseproxy --type=LoadBalancer --name=publicreverseproxy

publicfrontend LoadBalancer 10.100.183.208 ad0b2c3be212c439e9eecfe776b926d9-227962102.us-east-1.elb.amazonaws.com 80:32432/TCP 2m16s
publicreverseproxy LoadBalancer 10.100.247.122 ad73f7e80bad94c088de0ff341b20a51-404723933.us-east-1.elb.amazonaws.com 80:30532/TCP 74s

Run these commands from the /udagram-frontend directory
docker build . -t [Dockerhub-username]/udagram-frontend:v6
docker push [Dockerhub-username]/udagram-frontend:v6

Run these commands from the /udagram-deployment directory
Rolling update the containers of "frontend" deployment
kubectl set image deployment frontend frontend=[Dockerhub-username]/udagram-frontend:v6
