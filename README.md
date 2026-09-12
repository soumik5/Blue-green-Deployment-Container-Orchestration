# Blue-Green Deployment Project

## Prerequisites
- Docker Desktop
- Minikube
- kubectl
- Helm
- Node.js
- Git

## Project Setup

### 1. Clone the Repository
```bash
git clone https://github.com/soumik5/Blue-green-Deployment-Container-Orchestration.git

cd Blue-green-Deployment-Container-Orchestration/
```
<img width="875" height="175" alt="image" src="https://github.com/user-attachments/assets/eb8803fc-e442-469e-8974-aff140b5131b" />
<img width="920" height="272" alt="image" src="https://github.com/user-attachments/assets/ebabed2b-2c1a-499d-b362-a64b4309f1bb" />

### 2. Local Development

#### Backend Setup
1. Navigate to backend directory
2. Install dependencies
```bash
cd backend
npm install
```
<img width="987" height="437" alt="image" src="https://github.com/user-attachments/assets/cb52a441-a888-4855-aa52-b25f1c0a6045" />

3. Create `.env` file with:
```
PORT=5000
MONGO_URI=mongodb+srv://handyguy495_db_user:<pwd>@clusternew5.o35be6r.mongodb.net/blue-green
```
<img width="997" height="570" alt="image" src="https://github.com/user-attachments/assets/4ce34ef1-7aba-432f-a4e6-99fb2ec22379" />

4. Start backend server
```bash
npm start
```
<img width="976" height="151" alt="image" src="https://github.com/user-attachments/assets/9d0a09ec-a4e8-442c-aecf-6b13ffffad43" />

#### Frontend Setup
1. Setup Blue Frontend
```bash
cd frontend-blue
npm install
```
<img width="1017" height="481" alt="image" src="https://github.com/user-attachments/assets/264e3e83-f196-4d70-a34a-4a2bdacb24ba" />

2. Create `.env` file:
```
PORT=3100
```
<img width="995" height="182" alt="image" src="https://github.com/user-attachments/assets/4bb01355-6011-4585-9292-e3af30f6d5bf" />

3. Start blue frontend
```bash
npm start
```
<img width="1002" height="192" alt="image" src="https://github.com/user-attachments/assets/2cfc17c3-b65b-467f-8f3d-8351d3edaf8e" />
<img width="1427" height="937" alt="image" src="https://github.com/user-attachments/assets/d2ab8592-ebb3-4a52-b9bc-ba3985d546c9" />
<img width="1470" height="627" alt="image" src="https://github.com/user-attachments/assets/4d6c1fcd-cfd2-484a-b2ed-3e58df0b15b1" />


4. Setup Green Frontend
```bash
cd frontend-green
npm install
```
<img width="1036" height="442" alt="image" src="https://github.com/user-attachments/assets/623790a0-ce29-448a-a38c-2f3daf2bc710" />

5. Create `.env` file:
```
PORT=3200
```
<img width="1002" height="182" alt="image" src="https://github.com/user-attachments/assets/d5716e17-55f2-4559-bc2a-21599161a878" />


6.  Start Green frontend
```bash
npm start
```
<img width="1037" height="147" alt="image" src="https://github.com/user-attachments/assets/61c20ea0-54f5-43d6-a424-971b755adefa" />
<img width="1851" height="877" alt="image" src="https://github.com/user-attachments/assets/d4cd05ca-64dd-4e01-9dd4-2c5d32fcf486" />
<img width="1680" height="872" alt="image" src="https://github.com/user-attachments/assets/e1ea4c54-5b1c-40b9-8c03-0f5367029180" />



### 3. Dockerization

#### Build Docker Images

Create a Dockerfile for the backend service
<img width="770" height="285" alt="image" src="https://github.com/user-attachments/assets/9b848bad-1cef-4c1e-ae0b-dd524b9b941f" />

Create Dockerfiles for both frontend services
<img width="892" height="242" alt="image" src="https://github.com/user-attachments/assets/91a453ad-e617-434c-841d-d33b991d66e8" />
<img width="821" height="222" alt="image" src="https://github.com/user-attachments/assets/f8d276eb-7c7f-45c9-86fa-5e7e3b12701b" />

Create a docker-compose.yml file that runs all services together 
<img width="1131" height="857" alt="image" src="https://github.com/user-attachments/assets/ef4e05a7-cc03-4d11-8dbd-d7e89dc69e2c" />

Build and run the containers locally to verify functionality 
```bash
docker compose up -d
```
<img width="1491" height="566" alt="image" src="https://github.com/user-attachments/assets/a48371be-4e59-463f-ba7f-2ced320450ac" />
<img width="906" height="222" alt="image" src="https://github.com/user-attachments/assets/a33fe193-5811-4b00-8161-bb69b3ef6d88" />


### 4. Kubernetes Deployment

#### Minikube Setup
1. Start Minikube
```bash
minikube start
```
<img width="950" height="540" alt="image" src="https://github.com/user-attachments/assets/05311061-2c83-44e9-960b-2cfaa00b928b" />


2. Enable Required Addons
```bash
minikube addons enable metrics-server
minikube addons enable ingress
```
<img width="1106" height="402" alt="image" src="https://github.com/user-attachments/assets/684127ef-bf23-4733-95fc-f72b12b29c98" />


### 5. Create Kubernetes Manifest Files

#### Required Manifest Files
Created k8s directory
pushed locally built images to dockerhub
<img width="1005" height="971" alt="image" src="https://github.com/user-attachments/assets/39fc827f-28b6-43ef-b4ce-29b2da449def" />

Create following files in `k8s/` directory:
- `backend-deployment.yaml`
- `frontend-blue-deployment.yaml`
- `frontend-green-deployment.yaml`
- `frontend-service.yaml`
- `ingress.yaml`

Above files have been created under k8s repository. Please refer to k8s folder of my Github repo for detaiils.

#### Service File Key Concepts
Your `frontend-service.yaml` should:
- Use selector to route traffic
- Define version (blue/green)
- Map ports correctly

### 6. Deploy to Minikube
```bash
# Apply all manifests
kubectl apply -f k8s/

# Verify deployments
kubectl get deployments
kubectl get services
kubectl get pods
```

### 7. Blue-Green Switching

#### Switch Traffic Methods

1. Basic Patch Command
```bash
# Switch to Green
kubectl patch service frontend-service -p '{"spec":{"selector":{"version":"green"}}}'

# Switch back to Blue
kubectl patch service frontend-service -p '{"spec":{"selector":{"version":"blue"}}}'
```

2. Detailed Patch Command
```bash
kubectl patch service frontend-service --type='merge' -p '{
  "spec":{
    "selector":{
      "app":"frontend",
      "version":"green"
    }
  }
}'
```

### 8. Verification
- Check service endpoints
- Verify traffic routing
- Monitor application logs

### Troubleshooting
- `kubectl get pods` - Check pod status
- `kubectl logs <pod-name>` - View logs
- `kubectl describe service frontend-service` - Service details

### Cleanup
```bash
# Remove deployments
kubectl delete -f k8s/

# Stop Minikube
minikube stop
```

## Blue-Green Deployment Flow Chart

```mermaid
graph TD
    A[Blue Environment Running] -->|Deploy Green| B[Green Environment Prepared]
    B -->|Validate Green| C{Green Ready?}
    C -->|Yes| D[Update Service Selector]
    C -->|No| B
    D -->|Redirect Traffic| E[Green Now Active]
    E -->|Rollback Option| A
```

### Flow Explanation
1. Blue environment is initial production
2. Green environment deployed alongside
3. Validate green environment 
4. Update service selector
5. Redirect traffic to green
6. Blue remains as rollback option

## Best Practices
- Implement health checks
- Use resource limits
- Configure monitoring
- Validate before switching
- Maintain rollback strategy


## License
This project is licensed under the MIT License
