# Dockercat Kubernetes Orchestration - Implementation Explanation

## Overview

This project builds on the Dockercat application developed in the previous CAT.

The objective was to take the existing containerized application and deploy it using Kubernetes orchestration concepts.

The application consists of a React frontend, Node.js/Express backend, and MongoDB database.

The project was first tested locally with Minikube before being prepared for Google Kubernetes Engine (GKE).

## 1. Choice of Kubernetes Objects

### Frontend Deployment

A Kubernetes Deployment was used for the React frontend.

The frontend runs two replicas to improve availability.

The Pods use the labels:

`app: dockercat`

`component: frontend`

The frontend image is:

`cyganouko/dockercat-frontend:v1`

The frontend container serves the React production build using Nginx.

### Backend Deployment

A Kubernetes Deployment was used for the Node.js/Express backend.

The backend runs two replicas.

The Pods use:

`app: dockercat`

`component: backend`

The backend image is:

`cyganouko/dockercat-backend:v1`

The backend listens on port 5000.

### Services

Kubernetes Services provide stable networking between application components.

The backend uses a ClusterIP Service named `backend`.

The frontend uses a NodePort Service during local Minikube testing.

Services select Pods using Kubernetes labels.

### MongoDB StatefulSet

MongoDB is deployed using a StatefulSet because it is a stateful workload.

A StatefulSet is appropriate for database workloads because the database requires stable identity and persistent storage.

## 2. Method Used to Expose Pods to Internet Traffic

During Minikube testing, the frontend was exposed using a NodePort Service.

The frontend Service uses:

`port: 3000`

`targetPort: 3000`

`nodePort: 30080`

The local service URL was obtained with:

`minikube service frontend --url`

For the final GKE deployment, the frontend will be exposed using a public Kubernetes Service and its external IP and port will be documented in `README.md`.

## 3. Persistent Storage

MongoDB uses a StatefulSet together with persistent storage.

Persistent storage ensures that database data survives the replacement or deletion of a MongoDB Pod.

This is important because Kubernetes Pods are temporary resources.

The database therefore requires storage that exists independently of the lifecycle of an individual Pod.

This implementation also satisfies the bonus requirement for using a StatefulSet for the database layer.

## 4. Application Testing

The backend was tested from inside the Kubernetes cluster.

The endpoint:

`http://backend:5000/api/products`

successfully returned HTTP 200.

A test product was successfully created through the API.

The returned MongoDB document included the product name, description and price.

A subsequent GET request returned the same product.

This demonstrated successful communication between the backend and MongoDB.

The backend logs also confirmed:

`Server listening on port 5000`

`Database connected successfully`

## 5. Frontend Debugging

The first frontend Docker build failed because imports in `ProductControl.js` were not located at the top of the module.

The build reported:

`Import in body of module; reorder to top`

The imports were reorganized and the API URL declaration was separated from the import statements.

The frontend was then rebuilt successfully.

The final build reported:

`Compiled successfully.`

The Docker image was successfully created as:

`cyganouko/dockercat-frontend:v1`

## 6. Docker Image Naming

Personalized Docker image names were used for the application components.

Backend:

`cyganouko/dockercat-backend:v1`

Frontend:

`cyganouko/dockercat-frontend:v1`

The naming convention identifies the Docker Hub owner, project, component and version.

The backend image was successfully pushed to Docker Hub.

## 7. Git Workflow

Git was used throughout the development process.

Each significant development step was committed independently with descriptive commit messages.

Examples include:

- Import application from previous CAT
- Add Kubernetes project structure
- Implement persistent MongoDB StatefulSet
- Add Kubernetes backend deployment and service
- Fix frontend API import ordering
- Add Kubernetes frontend deployment
- Expose frontend through Kubernetes NodePort

This workflow provides a clear history of how the application was converted into a Kubernetes deployment.

## 8. Labels and Annotations

Labels were used to identify application components.

Frontend:

`app: dockercat`

`component: frontend`

Backend:

`app: dockercat`

`component: backend`

The labels allow Kubernetes Services to select the correct Pods.

Annotations are also used to provide additional metadata for Kubernetes resources.

## 9. Application Availability

The frontend and backend each use two replicas.

This provides redundancy and allows Kubernetes Deployments to maintain the desired number of Pods.

During testing, both backend replicas reached:

`READY 2/2`

Both frontend replicas also reached:

`READY 2/2`

The Deployment controllers are responsible for maintaining these replicas.

## 10. Local Kubernetes Testing

Minikube was used as the local Kubernetes environment.

The following areas were tested:

- Docker image creation
- Kubernetes manifest validation
- Deployments
- Pods
- Services
- Service discovery
- Backend API
- MongoDB connectivity
- Frontend deployment
- Frontend Service exposure

Testing locally allowed configuration and application issues to be identified before GKE deployment.

## 11. GKE Deployment

Google Kubernetes Engine is the final deployment target for this CAT.

The Kubernetes manifests in the `k8s/` directory will be applied to the GKE cluster.

The final deployment will contain:

- Frontend Deployment
- Backend Deployment
- MongoDB StatefulSet
- Kubernetes Services
- Persistent storage
- Multiple application replicas

The resulting public GKE IP address and port will be recorded in the project README.

## 12. Final Architecture

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

## Conclusion

The Dockercat application has been successfully converted from the previous containerized project into a Kubernetes-orchestrated application.

The implementation demonstrates Deployments, Services, StatefulSets, persistent storage, replicas, labels, annotations, Docker image tagging, service discovery, testing, debugging and Git workflow.

The application has been successfully tested locally using Minikube and is now ready for deployment to Google Kubernetes Engine.
