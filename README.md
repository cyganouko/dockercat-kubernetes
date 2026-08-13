# Dockercat Kubernetes

## Kubernetes Orchestration CAT

This project is the Kubernetes orchestration implementation of the Dockercat application developed in the previous CAT.

The application consists of:
- React frontend
- Node.js/Express backend
- MongoDB database

## Live Application

### GKE URL

PENDING_GKE_DEPLOYMENT

The final GKE external IP and port will be documented here after deployment.

## Architecture

    Internet
       |
       v
    Frontend Service
       |
       v
    Frontend Deployment
       |-- Frontend Pod
       |-- Frontend Pod
       |
       v
    Backend Service
       |
       v
    Backend Deployment
       |-- Backend Pod
       |-- Backend Pod
       |
       v
    MongoDB Service
       |
       v
    MongoDB StatefulSet
       |
       v
    Persistent Storage

## Docker Images

Backend:

`cyganouko/dockercat-backend:v1`

Frontend:

`cyganouko/dockercat-frontend:v1`

## Kubernetes Objects

### Frontend
- Deployment
- Service
- 2 replicas

### Backend
- Deployment
- Service
- 2 replicas

### Database
- StatefulSet
- Service
- Persistent storage

## Kubernetes Manifests

All Kubernetes manifests are located in the `k8s/` directory.

## Local Testing

The application was tested locally using Minikube.

The backend was successfully tested through the Kubernetes Service:

`http://backend:5000/api/products`

A test product was successfully created and retrieved from MongoDB.

Backend logs confirmed:

`Server listening on port 5000`

`Database connected successfully`

The frontend was also successfully built and deployed.

## Git Workflow

The project uses descriptive commits for each significant development step.

Important commits include:

- Import application from previous CAT
- Add Kubernetes project structure
- Implement persistent MongoDB StatefulSet
- Add Kubernetes backend deployment and service
- Fix frontend API import ordering
- Add Kubernetes frontend deployment
- Expose frontend through Kubernetes NodePort

## GKE Deployment

The final application will be deployed to Google Kubernetes Engine.

After successful deployment, the public GKE URL will be added to this README.

## Documentation

Detailed implementation decisions are documented in `explanation.md`.

## Author

Cygan Ouko
