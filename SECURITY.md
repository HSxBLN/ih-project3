# Security & Compliance

## Network Security
- **Ingress**: Only the Ingress controller is exposed to the public internet (Port 80/443).
- **Internal Traffic**: Microservices communicate internally within the Kubernetes cluster.
- **Network Policies**: (Recommended) Implement NetworkPolicies to restrict traffic between pods.

## Secret Management
- **Environment Variables**: Sensitive data (DB URIs, API Keys) are injected via Kubernetes Secrets.
- **Storage**: Secrets are stored in `k8s/restauranty-secrets.yaml` (base64 encoded). **DO NOT COMMIT REAL SECRETS TO GIT.**
- **CI/CD**: Secrets for the pipeline (Docker Hub credentials) are stored in GitHub Secrets.

## Authentication & Authorization
- **JWT**: The `auth` service issues JWTs.
- **Validation**: `discounts` and `items` services validate JWTs from the `Authorization` header.

## Compliance
- **Data Encryption**: Ensure MongoDB uses encryption at rest (provider managed).
- **HTTPS**: Enable TLS on the Ingress controller (e.g., using cert-manager and Let's Encrypt).

> [!WARNING]
> **Database Security**: The current MongoDB deployment (`k8s/mongo-deployment.yaml`) runs without authentication for demonstration purposes. **Do not use this configuration in production.** Enable MongoDB authentication and update connection strings/secrets accordingly.
