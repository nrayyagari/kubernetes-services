

# Tech stack used to building an end-to-end CI/CD pipeline

1. EKS: Using AWS EKS - Managed Kubernetes Cluster
2. Nginx Ingress Controller: Configures HTTP load balancer according to Ingress resource. Using single load balancer to deliver multiple microservices. This is done through Ingress resource for each microservice
3. External-DNS: Generates automatic DNS entries for services/ingress resources created in the cluster
4. Prometheus Grafana: For monitoring Kubernetes cluster resources


# Next improvements

1. Installing EFK for logging purpose
2. Updating Github actions workflow to use IAM role instead of Github secrets
3. Using AWS KMS for secret management
4. Automatic Infrastructure creation based on IaC using Terraform