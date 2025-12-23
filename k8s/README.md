# Flask MongoDB Application on Kubernetes

## Project Overview


This project is a simple Flask-based REST API connected to MongoDB, containerized using Docker and deployed on Kubernetes using Minikube.
The application allows inserting and retrieving data from MongoDB and demonstrates Kubernetes concepts such as Deployments, StatefulSets, Services, Persistent Volumes, and Horizontal Pod Autoscaling.

The goal of this assignment is to show how a backend application can be deployed, managed, and scaled inside a Kubernetes cluster.

Technologies Used
- Python with Flask
- MongoDB
- Docker
- Kubernetes (Minikube)
- Horizontal Pod Autoscaler
- metrics-server



Application Endpoints
- Method	Endpoint	Description
- GET	/	Returns a welcome message
- POST	/data	Inserts JSON data into MongoDB
- GET	/data	Fetches stored data from MongoDB

json reponse:
{
  "name": "Aparna",
  "status": "K8s working"
}

## Docker Setup
Build the image
docker build -t ad21144004/flask-mongodb-app:v1 .

Push the image
docker push ad21144004/flask-mongodb-app:v1

## Kubernetes Setup

The following Kubernetes components are used in this project:

PersistentVolume and PersistentVolumeClaim for MongoDB storage

Secret for MongoDB authentication

StatefulSet for MongoDB

Deployment for the Flask application

NodePort Service to expose the Flask app

Horizontal Pod Autoscaler for scaling Flask pods

Apply Kubernetes manifests
- kubectl apply -f k8s/mongo-pv-pvc.yaml
- kubectl apply -f k8s/mongo-secret.yaml
- kubectl apply -f k8s/mongo-service.yaml
- kubectl apply -f k8s/mongo-statefulset.yaml
- kubectl apply -f k8s/flask-deployment.yaml
- kubectl apply -f k8s/flask-service.yaml
- kubectl apply -f k8s/flask-hpa.yaml


Check resources:

- kubectl get pods
- kubectl get svc
- kubectl get hpa

## Autoscaling Results

Horizontal Pod Autoscaler was configured with:

Minimum replicas set to 2

Maximum replicas set to 5

CPU utilization target set to 70 percent

Metrics-server was enabled in Minikube, and CPU metrics were successfully observed:

cpu: 1% / 70%


During testing, continuous HTTP requests were sent to the Flask service to generate load.
Since CPU usage remained below the defined threshold, the HPA correctly kept the number of replicas at 2.

This confirms that autoscaling is active and functioning as expected.
In an environment with higher sustained CPU usage, the application would automatically scale up based on the defined policy.

## Testing the Application
Insert data
Invoke-RestMethod -Method POST `
  -Uri http://127.0.0.1:5000/data `
  -Headers @{ "Content-Type" = "application/json" } `
  -Body '{"name":"Aparna","status":"K8s working"}'

Retrieve data
Invoke-RestMethod -Uri http://127.0.0.1:5000/data

## Screenshots

The README includes screenshots showing:

Running Flask and MongoDB pods

Horizontal Pod Autoscaler status with CPU metrics

Successful POST and GET API responses

Load generation command used for autoscaling testing
## Screenshots

### Running Pods
![Running Pods](screenshots/pods.jpg)

### HPA Status
![HPA Status](screenshots/hpa.jpg)

### Autoscaling Observation
![Autoscaling](screenshots/autoscaling.jpg)

### API POST and GET Results
![API Results](screenshots/data.jpg)


### Browser Response
![Browser response](screenshots/browser.jpg)

## Conclusion

This project demonstrates a complete workflow for deploying a Flask application with MongoDB on Kubernetes.
It covers containerization, persistent storage, service exposure, and autoscaling using Kubernetes-native tools.

All requirements mentioned in the assignment have been implemented and verified.

### Author
### Aparna Dubey