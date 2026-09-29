# Movie Pipeline CI/CD Project

## Project Overview

This project demonstrates a complete CI/CD pipeline for a movie
application using:

-   React frontend
-   Flask backend
-   Docker
-   Amazon ECR
-   Amazon EKS
-   Kubernetes
-   GitHub Actions

The project contains four GitHub Actions workflows:

1.  Frontend Continuous Integration
2.  Frontend Continuous Deployment
3.  Backend Continuous Integration
4.  Backend Continuous Deployment

## Application URLs

### Frontend Application

http://k8s-default-frontend-26bf13b672-6e1603d27ef89a56.elb.eu-north-1.amazonaws.com

### Backend API

http://k8s-default-backend-d3ab81effa-2e63817adef35fdd.elb.eu-north-1.amazonaws.com/movies

The backend `/movies` endpoint returns the movie data used by the
frontend.

## CI/CD Workflows

Workflow files:

``` text
.github/workflows/
├── backend-ci.yml
├── backend-cd.yaml
├── frontend-ci.yml
└── frontend-cd.yaml
```

### Backend CI

The backend CI workflow:

-   Checks out the source code
-   Sets up Python 3.10
-   Installs Pipenv and development dependencies
-   Runs linting
-   Runs backend tests
-   Builds the backend Docker image

### Backend CD

The backend CD workflow:

-   Runs backend linting and tests
-   Builds the backend Docker image
-   Pushes the image to Amazon ECR
-   Updates the EKS kubeconfig
-   Updates the Kubernetes backend deployment image
-   Deploys the backend using Kustomize

### Frontend CI

The frontend CI workflow:

-   Checks out the source code
-   Sets up Node.js 18
-   Installs npm dependencies
-   Runs linting
-   Runs frontend tests
-   Builds the React application
-   Builds the frontend Docker image

### Frontend CD

The frontend CD workflow:

-   Runs frontend linting and tests
-   Builds the React frontend with the backend API URL
-   Pushes the frontend image to Amazon ECR
-   Updates the EKS kubeconfig
-   Updates the Kubernetes frontend deployment image
-   Deploys the frontend using Kustomize

## AWS Environment

AWS Region:

``` text
eu-north-1
```

EKS Cluster:

``` text
movie-pipeline-cluster
```

ECR repositories:

``` text
frontend
backend
```

## Kubernetes Deployment

The application is deployed in the Kubernetes `default` namespace.

Frontend:

``` text
Port: 3000
Service: LoadBalancer
External access: AWS LoadBalancer
```

Backend:

``` text
Port: 5000
Service: LoadBalancer
External access: AWS LoadBalancer
```

## Current Deployment

Both application deployments are running with one available replica.

Frontend image:

``` text
986449754903.dkr.ecr.eu-north-1.amazonaws.com/frontend:915424e8a6100f13d800330417a08effd18646e1
```

Backend image:

``` text
986449754903.dkr.ecr.eu-north-1.amazonaws.com/backend:915424e8a6100f13d800330417a08effd18646e1
```

## Verification Commands

Check all Kubernetes resources:

``` bash
kubectl get all
```

Describe the frontend deployment:

``` bash
kubectl describe deployment frontend
```

Describe the backend deployment:

``` bash
kubectl describe deployment backend
```

## CI/CD Verification

All four required GitHub Actions workflows have successful runs:

-   Backend Continuous Integration
-   Backend Continuous Deployment
-   Frontend Continuous Integration
-   Frontend Continuous Deployment

## Project Structure

``` text
.
├── .github/
│   └── workflows/
│       ├── backend-ci.yml
│       ├── backend-cd.yaml
│       ├── frontend-ci.yml
│       └── frontend-cd.yaml
├── starter/
│   ├── backend/
│   │   ├── k8s/
│   │   └── ...
│   └── frontend/
│       ├── k8s/
│       └── ...
└── README.md
```

## Notes

The frontend React application is built with the backend API
LoadBalancer URL as the `REACT_APP_MOVIE_API_URL` build argument. This
allows the deployed frontend to communicate with the backend running in
Amazon EKS.
