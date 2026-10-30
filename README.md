# Kubernetes – Flask Backend and Express Frontend

## 1. Project Overview

This project demonstrates how to deploy a Flask backend and an Express frontend to a local Kubernetes cluster using Minikube.

The objective is to containerize the applications, define Kubernetes Deployments and Services, deploy the applications, and verify that the resulting Pods and Services are running correctly.

Kubernetes provides a platform for deploying, managing, and scaling containerized applications.

## 2. Technologies Used

- Kubernetes
- Minikube
- kubectl
- Docker
- Docker Hub
- Python
- Flask
- Node.js
- Express.js
- Git and GitHub
- YAML

## 3. Project Structure

A typical project structure is:

```text
kubernetes/
├── backend/
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
├── frontend/
│   ├── app.js
│   ├── package.json
│   └── Dockerfile
├── k8s/
│   ├── backend-deployment.yaml
│   ├── backend-service.yaml
│   ├── frontend-deployment.yaml
│   └── frontend-service.yaml
└── README.md
```

Note: Use the actual manifest filenames and directories in your repository. If your assignment provides a different structure, retain that structure.

## 4. Application Architecture

The deployment contains two applications:

**Flask Backend**
- Runs in a Kubernetes Pod.
- Is managed by a Kubernetes Deployment.
- Is exposed internally through a Kubernetes Service.

**Express Frontend**
- Runs in a separate Kubernetes Pod.
- Is managed by its own Deployment.
- Is exposed through a Kubernetes Service.

The backend and frontend communicate through Kubernetes networking. The frontend must use the backend Service name and the correct Service port to reach the backend.

The frontend Service can be exposed outside the cluster using a NodePort or another method defined in the Kubernetes manifest.

## 5. Prerequisites

Install the following tools:

- Docker Desktop or another supported container runtime
- Minikube
- kubectl
- Git
- A Docker Hub account if using published images

Verify the installations:

```bash
docker --version
minikube version
kubectl version --client
git --version
```

## 6. Start the Minikube Cluster

Start Minikube:

```bash
minikube start
```

Check the cluster status:

```bash
minikube status
```

Check the Kubernetes nodes:

```bash
kubectl get nodes
```

The node should report a `Ready` status once the cluster is operational.

## 7. Build or Obtain the Container Images

The Flask and Express applications must have container images available to Kubernetes.

If the images are published to Docker Hub, configure your Kubernetes manifests to reference the actual image names and tags.

For example:

```text
<DOCKERHUB_USERNAME>/krishna-backend:1.0
<DOCKERHUB_USERNAME>/krishna-frontend:1.0
```

Replace `<DOCKERHUB_USERNAME>` with your actual Docker Hub username.

If you build images locally for Minikube, use a supported Minikube image-loading workflow and make sure the manifests reference the correct local image names.

## 8. Configure Kubernetes Manifests

Create or review the Kubernetes YAML manifests for both applications.

### Backend Deployment

The backend Deployment defines:
- The Flask application container image.
- The number of replicas.
- Container port `5000`.
- The desired Pod configuration.

### Backend Service

The backend Service provides a stable network endpoint for the backend Pods.

The Service's selector must match the labels on the backend Deployment's Pods. Its target port must match the port on which Flask listens.

### Frontend Deployment

The frontend Deployment defines:
- The Express application container image.
- The number of replicas.
- Container port `3000`.
- The desired Pod configuration.

### Frontend Service

The frontend Service exposes the Express application according to the configured Service type and port mappings.

For local browser access, use the method supported by your manifest, such as Minikube's Service URL or a NodePort.

## 9. Deploy the Applications

Navigate to the directory containing your Kubernetes manifests.

For example:

```bash
cd k8s
```

Apply the manifests:

```bash
kubectl apply -f .
```

If your manifests are in the project root, run the command from the appropriate directory or supply the correct manifest path.

Check the Deployments:

```bash
kubectl get deployments
```

Check the Pods:

```bash
kubectl get pods
```

Check the Services:

```bash
kubectl get services
```

Wait until the application Pods reach the `Running` state and the Deployments report that their desired replicas are available.

## 10. Verify the Deployments

Describe a Deployment:

```bash
kubectl describe deployment <DEPLOYMENT_NAME>
```

View Pod details:

```bash
kubectl describe pod <POD_NAME>
```

View application logs:

```bash
kubectl logs <POD_NAME>
```

If a Pod is not starting, inspect its events and logs to identify problems such as image-pull failures, configuration errors, or application startup failures.

## 11. Access the Frontend

If the frontend Service is configured as a NodePort, obtain its URL with:

```bash
minikube service <FRONTEND_SERVICE_NAME> --url
```

Open the URL returned by Minikube in your browser.

Replace `<FRONTEND_SERVICE_NAME>` with the actual name from your frontend Service manifest.

If your Service uses a different type, follow the access method appropriate to that configuration.

## 12. Test Backend Connectivity

Confirm that the backend Service exists:

```bash
kubectl get services
```

Verify that the frontend configuration points to the backend's Kubernetes Service name and correct port.

If your Flask application provides a health endpoint, you can test it using a port-forward:

```bash
kubectl port-forward service/<BACKEND_SERVICE_NAME> 5000:5000
```

Then, in another terminal, run:

```bash
curl http://127.0.0.1:5000/health
```

This test requires the backend Service to expose port `5000` and the Flask application to implement `/health`.

## 13. Update and Delete Resources

Apply updated manifests:

```bash
kubectl apply -f .
```

Check deployment status:

```bash
kubectl rollout status deployment/<DEPLOYMENT_NAME>
```

Delete the resources defined in the manifests:

```bash
kubectl delete -f .
```

Stop Minikube when you have finished testing:

```bash
minikube stop
```

Delete the Minikube cluster only if you no longer need it:

```bash
minikube delete
```

## 14. Troubleshooting

### Pod is not running

Check Pod status and events:

```bash
kubectl get pods
kubectl describe pod <POD_NAME>
```

Check application logs:

```bash
kubectl logs <POD_NAME>
```

### Image pull errors

Verify the image name, tag, registry permissions, and image-pull configuration.

### Service is not accessible

Check that:
- The Service selector matches the Pod labels.
- The Service port and target port are correct.
- The frontend Service is exposed using the expected access method.
- The relevant Pods are running and ready.

### Frontend cannot communicate with backend

Verify that the frontend uses the backend Kubernetes Service name rather than `localhost` for in-cluster communication.

## 15. GitHub Repository

Repository URL: https://github.com/krishna-280792shinde/kubernetes

## 16. Conclusion

This project demonstrates the deployment of containerized Flask and Express applications on Kubernetes using Minikube.

It provides practical experience with Kubernetes Deployments, Services, Pods, YAML configuration, container images, application networking, and deployment verification.
