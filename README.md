# Nexvion – DevOps Project

## 1. Project Overview:

Nexvion is a frontend e-commerce demo website containerized using Docker and Nginx. This project demonstrates a DevOps workflow using Git, GitHub, Docker, Docker Compose, GitHub Actions, Trivy, Docker Hub, Kubernetes, and Minikube.

The project includes automated Docker image building, container image security scanning, Docker Hub image publishing, Kubernetes deployment, service exposure, and replica scaling.

**GitHub Repository:**  
https://github.com/iamamitha16/Nexvion-DevOps-Project

**Docker Hub Repository:**  
https://hub.docker.com/r/amithasri16/nexvion-website

## 2. Project Objectives:

- Containerize the Nexvion website using Docker.
- Serve the website using Nginx.
- Manage containers using Docker Compose.
- Automate Docker image building with GitHub Actions.
- Scan the Docker image using Trivy.
- Publish and pull the Docker image from Docker Hub.
- Deploy the application to a local Kubernetes cluster using Minikube.
- Expose the application through a Kubernetes Service.
- Scale the deployment from two replicas to three replicas.

## 3. Technologies Used:

| Technology | Purpose |
|---|---|
| HTML | Website structure |
| CSS | Website styling |
| JavaScript | Frontend functionality |
| Git | Version control |
| GitHub | Source code hosting |
| Docker | Containerization |
| Docker Compose | Container management |
| Nginx | Static website server |
| GitHub Actions | CI workflow automation |
| Trivy | Container image vulnerability scanning |
| Docker Hub | Docker image registry |
| Kubernetes | Container orchestration |
| Minikube | Local Kubernetes cluster |
| kubectl | Kubernetes command-line management |

## 4. Project Architecture:

```text
Developer
    |
    v
Git and GitHub
    |
    v
GitHub Actions CI Workflow
    |
    +--> Validate Website Files
    |
    +--> Build Docker Image
    |
    +--> Scan Image Using Trivy
    |
    v
Docker Hub
    |
    v
Kubernetes / Minikube
    |
    v
Nexvion Deployment
    |
    +--> Replica Pod 1
    +--> Replica Pod 2
    +--> Replica Pod 3 (after scaling)
    |
    v
Kubernetes NodePort Service
    |
    v
Nexvion Website
```

## 5. Project Structure:

```text
Nexvion/
├── .github/
│   └── workflows/
│       └── docker-build.yml
├── Dockerfile
├── .dockerignore
├── docker-compose.yml
├── deployment.yaml
├── README.md
├── README.txt
├── index.html
├── products.html
├── payment.html
├── style.css
├── products.css
├── payment.css
├── script.js
├── payment.js
└── logo.png
```

## 6. Docker Containerization:

The Nexvion website is packaged into a Docker image. Nginx serves the static website files inside the container.

### Build the Docker image

Run the following command from the project directory:

```bash
docker build -t nexvion-website:1.0 .
```

### Run the local Docker image

```bash
docker run -d --name nexvion-test -p 8090:80 nexvion-website:1.0
```

Open the website in your browser:

http://localhost:8090

**Port mapping:** Host port `8090` maps to container port `80`.

### Verify the container

```bash
docker ps
```

### Docker Compose

Start the website using Docker Compose:

```bash
docker compose up -d --build
```

Check the Compose service:

```bash
docker compose ps
```

Open the website:

http://localhost:8090

Stop the Compose application when needed:

```bash
docker compose down
```

**Note:** If the Nexvion container is already using port `8090`, use the existing container instead of starting another container with the same port.

## 7. Docker Hub Image Publishing:

The Docker image was published to Docker Hub.

**Image name:** `amithasri16/nexvion-website:1.0`

**Docker Hub URL:**  
https://hub.docker.com/r/amithasri16/nexvion-website

### Pull the published image

```bash
docker pull amithasri16/nexvion-website:1.0
```

### Run the published image on port 8090

If port `8090` is free, run:

```bash
docker run -d --name nexvion-hub-test -p 8090:80 amithasri16/nexvion-website:1.0
```

Open:

http://localhost:8090

Verify the running container:

```bash
docker ps
```

The host port `8090` forwards traffic to port `80` inside the container, where Nginx serves the website.

## 8. GitHub Actions CI Pipeline:

The GitHub Actions workflow is stored in:

`.github/workflows/docker-build.yml`

The workflow runs when changes are pushed to or a pull request targets the `main` branch. It can also be started manually.

### Pipeline stages

1. Check out the repository source code.
2. Verify that the required website files exist.
3. Build the Docker image.
4. Scan the image using Trivy.

### Required website file validation

The workflow checks for the presence of the main HTML, CSS, JavaScript, and image files before building the Docker image.

### Docker image build

GitHub Actions builds the Docker image to verify that the application can be packaged successfully.

### Trivy security scanning

Trivy scans the image for operating-system package and library vulnerabilities.

The workflow is configured to report CRITICAL, HIGH, and MEDIUM severity findings.

The current setting `exit-code: '0'` means detected vulnerabilities alone do not cause the workflow to fail. A successful workflow run does not prove that the image is free of vulnerabilities.

## 9. Kubernetes Deployment Using Minikube:

Nexvion is deployed locally to Kubernetes using Minikube and the Docker Hub image.

**Docker Hub image:**

`amithasri16/nexvion-website:1.0`

### 9.1 Start Minikube:

Ensure Docker Desktop is running, then execute:

```bash
minikube start --driver=docker
```

### 9.2 Verify the Kubernetes cluster:

Check the node:

```bash
kubectl get nodes
```

Check all pods:

```bash
kubectl get pods -A
```

The Minikube node should display `Ready`.

### 9.3 Kubernetes deployment configuration:

The Kubernetes configuration is stored in:

`deployment.yaml`

The deployment configuration defines:

- Deployment name: `nexvion-deployment`
- Container image: `amithasri16/nexvion-website:1.0`
- Container port: `80`
- Initial replicas: `2`

Apply the configuration:

```bash
kubectl apply -f deployment.yaml
```

### 9.4 Verify the deployment:

```bash
kubectl get deployments
```

Check the application pods:

```bash
kubectl get pods
```

The initial deployment was verified with two replicas in `Running` and ready status.

### 9.5 Scale the application:

Increase the number of replicas from two to three:

```bash
kubectl scale deployment nexvion-deployment --replicas=3
```

Verify the deployment:

```bash
kubectl get deployments
```

Verify the pods:

```bash
kubectl get pods
```

The deployment was verified with three ready replicas.

**Purpose of scaling:** Kubernetes maintains the requested number of application replicas, helping distribute traffic across available pods and improving application availability.

## 10. Kubernetes Service and Website Access:

A Kubernetes NodePort Service was created to expose the Nexvion deployment.

### Create the Service

```bash
kubectl expose deployment nexvion-deployment --type=NodePort --port=80
```

### Verify the Service

```bash
kubectl get services
```

The Service was created with a dynamically assigned NodePort.

### Open the Nexvion website

Run:

```bash
minikube service nexvion-deployment
```

Minikube displays a local URL and may open the default browser automatically.

Keep the terminal running while using the local service tunnel. The URL may use a temporary localhost port and can change when the tunnel is restarted.

The Nexvion website was opened successfully through the Minikube Service URL.

**Important:** Port `8090` is used for the local Docker website. The Kubernetes website is accessed through the URL provided by `minikube service nexvion-deployment`; these are separate access methods.

## 11. Useful Kubernetes Commands:

| Command | Purpose |
|---|---|
| `kubectl get nodes` | View cluster nodes |
| `kubectl get pods` | View application pods |
| `kubectl get pods -A` | View pods across all namespaces |
| `kubectl get deployments` | View deployments and replica status |
| `kubectl get services` | View Kubernetes Services |
| `kubectl describe deployment nexvion-deployment` | Inspect deployment details |
| `kubectl logs deployment/nexvion-deployment` | View application container logs |
| `kubectl scale deployment nexvion-deployment --replicas=3` | Scale the deployment to three replicas |

## 12. Testing and Verification:

The following checks were completed during the project:

- Built the Nexvion Docker image successfully.
- Ran the website using Docker.
- Verified local Docker access through port `8090`.
- Published the image to Docker Hub.
- Pulled the published Docker Hub image.
- Executed the GitHub Actions Docker build and Trivy scanning workflow successfully.
- Started the Minikube cluster.
- Verified that the Kubernetes node was `Ready`.
- Deployed Nexvion with two replicas.
- Verified that both application pods were `Running` and ready.
- Created a Kubernetes NodePort Service.
- Opened the website through the Minikube service URL.
- Scaled the deployment to three replicas.
- Verified that all three replicas were ready.

## 13. Project Limitations:

Nexvion is a frontend e-commerce demonstration website.

Login, registration, shopping cart, and checkout functionality are implemented on the client side. The project does not include a production authentication backend, database-backed order processing, or a real payment gateway.

Do not use this demo to collect real passwords or payment details.

The current Trivy workflow reports vulnerability findings but does not fail solely because vulnerabilities are detected.

The Kubernetes deployment is local to Minikube and is not a production cloud deployment.

## 14. Future Enhancements:

- Add automated application tests to the CI pipeline.
- Configure the pipeline to fail when selected critical vulnerabilities are found.
- Automate Docker image publishing through GitHub Actions.
- Add Kubernetes readiness and liveness probes.
- Configure CPU and memory requests and limits.
- Deploy the application to a managed Kubernetes service.
- Add monitoring, logging, and application metrics.
- Introduce a backend and database for a production-style application.

## 15. Conclusion:

This project demonstrates a DevOps workflow for the Nexvion e-commerce demo website using Git, GitHub, Docker, Nginx, Docker Compose, GitHub Actions, Trivy, Docker Hub, Kubernetes, and Minikube.

The project covers containerization, automated image building, security scanning, image publishing, Kubernetes deployment, Service exposure, and scaling an application from two replicas to three.

It provides practical experience with important DevOps tools and workflows.
