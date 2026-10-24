# Nexvion - DevOps Internship Project

## 1. Project Overview:

Nexvion is a static e-commerce demonstration website built using HTML, CSS, and JavaScript.

This project demonstrates how to containerize a website using Docker and Nginx, run it using Docker Compose, and automate file validation and Docker image builds using GitHub Actions.

## 2. Technologies Used:

- Trivy (Docker image vulnerability scanning)
- HTML, CSS, and JavaScript
- Git and GitHub
- Docker
- Nginx
- Docker Compose
- GitHub Actions

## 3. Website Features:

- Product catalogue with 12 products
- Product search, sorting, and category filters
- Price and stock filters
- Shopping cart with quantity controls
- Demo registration and login
- Cart and account data stored using browser localStorage
- Checkout and order confirmation
- Payment form validation and cash-on-delivery option

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
   v
Validate Required Website Files
   |
   v
Build Docker Image
   |
   v
Run Container Using Docker Compose
   |
   v
Nginx Serves the Website
```

## 5. Docker Implementation:

The Dockerfile uses the lightweight Nginx Alpine image to serve the static website.

The website HTML, CSS, JavaScript, and logo files are copied into the Nginx web root.

### Build the Docker image

```bash
docker build -t nexvion-website:1.0 .
```

### Run with Docker Compose

```bash
docker compose up -d --build
```

### Check the running container

```bash
docker compose ps
```

### Access the website

Open the following URL in your browser:

http://localhost:8090

### Stop the application

```bash
docker compose down
```

### Published Docker Image:

The Nexvion Docker image is published on Docker Hub.

**Image:** `amithasri16/nexvion-website:1.0`

**Docker Hub Repository:** https://hub.docker.com/r/amithasri16/nexvion-website

Pull the image:

```bash
docker pull amithasri16/nexvion-website:1.0
```

Run the published image:

```bash
docker run -d --name nexvion-hub-test -p 127.0.0.1:8090:80 amithasri16/nexvion-website:1.0
```

Access the website at http://localhost:8090.

The published image was pulled from Docker Hub and started successfully in a separate container.


## 6. Continuous Integration with GitHub Actions:

Workflow file:

`.github/workflows/docker-build.yml`

The workflow runs when changes are pushed to the main branch, when a pull request targets main, or when the workflow is started manually.

The workflow performs these tasks:

1. Checks out the repository.
2. Verifies that the required website files exist.
3. Builds the Docker image in the GitHub Actions runner.

The workflow was executed successfully on the main branch.

### Docker Image Security Scanning

The GitHub Actions workflow uses Trivy to scan the built Docker image for known vulnerabilities.

The scan checks operating system packages and application libraries and reports CRITICAL, HIGH, and MEDIUM severity findings.

The workflow was tested successfully in GitHub Actions. The scan is configured to report findings without failing the pipeline solely because vulnerabilities are detected.

## 7. Testing and Verification:

The following checks were completed:

- Completed a Trivy Docker image security scan in GitHub Actions.
- Verified that the website opens in a browser.
- Tested the product catalogue and checkout pages.
- Built the Docker image successfully.
- Started the website using Docker Compose.
- Verified the running container and port mapping.
- Confirmed that the GitHub Actions CI workflow passed.

## 8. Project Limitations:

This is a frontend demonstration project. Login, registration, and payment interactions are demo functionality using browser-side storage and validation.

It does not include a production authentication backend, database-backed order processing, or a real payment gateway. It should not be used to collect real payment details.

## 9. Future Enhancements:

- Add automated application tests.
- Enforce security thresholds for critical vulnerabilities.
- Publish the Docker image to a container registry.
- Deploy the application to Kubernetes.
- Add monitoring and centralized logging.

## 10. Repository:

GitHub repository:

https://github.com/iamamitha16/Nexvion-DevOps-Project

## Conclusion:

This project provided hands-on practice with Docker containerization, Nginx, Docker Compose, Git, GitHub, and a GitHub Actions CI workflow for a static e-commerce website.
