# Restauranty DevOps Deployment

This project contains a microservices-based application (Restauranty) deployed using modern DevOps practices.

## Project Structure
- `backend/`: Node.js microservices (auth, discounts, items).
- `client/`: React frontend.
- `k8s/`: Kubernetes manifests for orchestration.
- `.github/workflows/`: CI/CD pipeline configuration.
- `monitoring/`: Prometheus configuration.
- `haproxy.cfg`: Local development load balancer configuration.

## Local Development
1.  **Prerequisites**: Node.js, Docker, MongoDB.
2.  **Start Services**:
    - Run MongoDB on port 27017.
    - Run each service (`npm start` in each folder).
    - Run HAProxy: `haproxy -f haproxy.cfg`.
3.  **Access**: `http://localhost`.

## Containerization
Each service has a `Dockerfile`.
- `backend/*`: Node.js 18 Alpine.
- `client`: Multi-stage build (Node.js build -> Nginx serve).

## Kubernetes Deployment
1.  **Secrets**: Update `k8s/secrets.yaml` with your base64 encoded secrets.
2.  **Deploy**:
    ```bash
    kubectl apply -f k8s/secrets.yaml
    kubectl apply -f k8s/
    ```
    *Note: This includes the MongoDB deployment (`k8s/mongo-*.yaml`).*
3.  **Access**: via the Ingress hostname: `restauranty.hauke.az.diogohack.shop`.

## AWS EKS Deployment
The CI/CD pipeline (`.github/workflows/ci-cd.yaml`) is configured to deploy to AWS EKS.
- **Prerequisites**: Set the following GitHub Secrets: `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_REGION`, `EKS_CLUSTER_NAME`.
- **Workflow**: On push to `main`, images are built, pushed to Docker Hub, and manifests are applied to the EKS cluster.

## CI/CD
GitHub Actions workflow (`.github/workflows/ci-cd.yaml`) handles:
- Build and Test (CI).
- Docker Build and Push to Docker Hub (CD).
- Deployment to AWS EKS (CD).

## Monitoring
- Prometheus configuration provided in `monitoring/prometheus.yaml`.
- Configure Prometheus to scrape the services.

## Security
See [SECURITY.md](SECURITY.md) for details on secret management, network security, and database risks.
