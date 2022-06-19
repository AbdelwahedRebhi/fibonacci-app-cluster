# Fibonacci App Cluster

[![Build Status](https://travis-ci.org/discorev/multi-k8s.svg?branch=master)](https://travis-ci.org/discorev/multi-k8s) 

## Introduction

This project is a designed to dockerize a react application and to deploy it using kubernetes. The web application allows users to enter a number and calculates the Fibonacci number at that index.

## Architecture

### Application Architecture

![Architecture](/Images/reactapp.png)


The user-facing deployment consits of the following:

* The Client: a simple ReactJS site with the form to enter a new index and shows the results of the calculations.
* The Server: an express server that acts as the REST API, reading and writing to the postgres database and redis instance.
* The Worker: a nodejs worker that subscribes to inserts into the redis instance and calcualtes the Fibonacci number for the given index, saving the result back into redis.

Also created:

* A Postgres database with a persistant volume claim
* A Redis instance
* NGINX Ingress routing requests to the `Client` or `Server` depending on the path component of the request.


### K8s Architecture

A high level overview of the architecture is down in the image below.

![Architecture](/Images/architecture.png)

## Requirements
Make sure you have these tools installed
- Docker
- Kubernetes
- Minikube and Kubectl

## Deployment

### On Docker
Make sure docker is running, and you are in the parent folder

```
cd Docker
docker-compose up 
```
Once docker containers are up, open the url http://localhost:3050/ to check the application

### Application Preview


![Architecture](/Images/dockercompose.png)


## On K8s Cluster

Run Minikube, we will be using Docker as the driver:
```
minikube start –-driver=docker
```

Check Minikube is running perfectly:
```
minikube status
```

We will create a secret for our Postgres instance password:
```
kubectl create secret generic pgpassword --from-literal PGPASSWORD=yourpassword
```
Make sure you are in the parent folder and apply the k8s configuration

```
cd Kubernetes
kubectl apply -f k8s
```


Run this command to create the Ingress Controller
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.2.0/deploy/static/provider/cloud/deploy.yaml
minikube addons enable ingress
```

To verify deployments/pods/persistent volumes/persistent volume claims/services status

```
kubectl get deployments
kubectl get pods
kubectl get services
kubectl get pv
kubectl get pvc
```

Once kubernetes pods and services are running, run

```
minikube tunnel
```
Open the url http://localhost/ to check the application


### Application Preview 

![Architecture](/Images/k8s.png)
