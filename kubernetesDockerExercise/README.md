# Developing and deploying a Node.js app from Docker to Kubernetes

## Assumptions:
- It is assumed that you already have node and npm installed
- It is also assumed that **minikube** environment for kubernetes is configured for installing single node cluster. Or else you also use the **kataconda** which is a cloud based simulation for running minikube.  

## Step 1: Make A Separate Directory And Initialize The Node Application

```
mkdir nodejs
cd nodejs/
npm init
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/kubernetesDockerExercise/screenshots/1.png">

## Step 2: Installing Express

```
npm install express --save
```
## Step 3: Make index.js File And Write Some Code
<img src="https://github.com/kuluruvineeth/Devops/blob/main/kubernetesDockerExercise/screenshots/2.png">

## Step 4: Dockerizing The Node Server
```
gedit Dockerfile
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/kubernetesDockerExercise/screenshots/14.png">
<img src="https://github.com/kuluruvineeth/Devops/blob/main/kubernetesDockerExercise/screenshots/3.png">

```
docker build -t node-server .
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/kubernetesDockerExercise/screenshots/4.png">
<img src="https://github.com/kuluruvineeth/Devops/blob/main/kubernetesDockerExercise/screenshots/5.png">

## Step 5: Create And Run The Container
```
docker run -d --name nodongo -p 3000:3000 node-server
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/kubernetesDockerExercise/screenshots/6.png">

## Step 6: Upload The Image To Docker Registry Docker Hub
<img src="https://github.com/kuluruvineeth/Devops/blob/main/kubernetesDockerExercise/screenshots/7.png">

```
docker tag node-server <username>/nodejs-starter
docker push <username>/nodejs-starter:1.1
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/kubernetesDockerExercise/screenshots/8.png">

## Step 7:
- We have used the Kataconda environment for the simulation of kubernetes clusters https://www.katacoda.com/courses/kubernetes/creating-kubernetes-yaml-definitions

## Step 8: Define YAML File To Create A Deployment In Kubernetes Cluster
- The deployment.yaml file is the one that we have used to specify the various parameters of our cluster like the number of pods, container features,etc.

## Step 9: Create Deployment In Kubernetes Cluster
```
kubectl create -f deployment.yaml
```
```
apiVersion: apps/v1 #1
kind: Deployment #2
metadata: #3
  name: nodejs-deployment #4
spec: #5
  replicas: 2 #6
  selector: #7
    matchLabels: #7
      app: nodejs #7
  template: #8
    metadata: #9
      labels: #10
        app: nodejs #11
    spec: #12
      containers: #13
      - name: nodongo #14
        image: <username>/nodejs-starter:1.1 #15
        ports: #16
        - containerPort: 3000 #17
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/kubernetesDockerExercise/screenshots/9.png">

## Step 10: Expose The Deployment To The Internet
```
kubectl expose deployment nodejs-deployment --type="LoadBalancer"
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/kubernetesDockerExercise/screenshots/10.png">

## Step 11: Using MetalLB In Your Minikube Environment(optional-do only in case if you use minikube environment for creating a kubernetes cluster)
```
kubectl apply -f https://raw.githubusercontent.com/google/metallb/v0.9.3/manifests/namespace.yaml
kubectl apply -f https://raw.githubusercontent.com/google/metallb/v0.9.3/manifests/metallb.yaml
kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey="$(openssl rand -base64 128)"
minikube ip
```
- Here, you’ll get your minikube IP — ours is 192.168.64.2. After this, we’ll create a config map for the address pool.
```
vim service.yaml
```
- Now copy paste this in the service.yaml file which is our configmap
```
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
     - name: default
       protocol: layer2
       addresses:
       - 192.168.79.61-192.168.79.71
```
```
kubectl create -f configmap.yaml
kubectl delete svc nodejs-deployment
kubectl expose deployment nodejs-deployment --type="LoadBalancer"
kubectl get svc
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/kubernetesDockerExercise/screenshots/11.png">
<img src="https://github.com/kuluruvineeth/Devops/blob/main/kubernetesDockerExercise/screenshots/12.png">
<img src="https://github.com/kuluruvineeth/Devops/blob/main/kubernetesDockerExercise/screenshots/13.png">



