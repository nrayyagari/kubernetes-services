
# Theme of demo

- For simplicity sake, assume DevOps engineer name is JC
- JC adds a new microservice, say 'microservice3' within `microservices-src-dir` directory along with a dockerfile to build image
- Then, JC creates a kubernetes manifest file for that microservice under a folder called `microservice3` in `kubernetes-manifests` file
- Thereafter proceeds to create a CI workflow by creating `microservice3-build-push.yaml` in `.github` folder, customizes it with env variables like ECR repo for this new microservice


#### CI Part

- Once he updates src code of his new microservices, Github actions will build the docker image, pushes it to ECR repo by authenticating using secrets fed into Github Secrets
- Github now itself updates the latest docker image tag in the kubernetes manifest file, so that it has name of the recent docker image

#### CD Part

- JC needs to create ArgoCD application and deploy it by running `kubectl create -f microservice3-application.yaml` (only one time task)
- JC also needs to mention in above file, where to fetch kubernetes manifest file for this application. 
- ArgoCD will periodically poll this file every 3 minutes. If it detects any change in manifest file, it will deploy the application automatically. 
- JC now is in relax mode :)


# Understanding Repo structure

This repository is organized as follows:

- `.github/`: Contains workflow for CI part(building and pushing docker image)
- `argocd-applications`: Contains files for ArgoCD applications
- `bootstrapping`: helm charts to bootstrap ArgoCD(to be deployed only once)
- `kubernetes-manifests`: Contains Kubernetes manifest files for microservices and other shared applications across cluster(eg: external-dns)
- `microservices-src-dir`: Contains src code and dockerfile for each microservice 


# Stack used for building an end-to-end CI/CD pipeline

1. EKS
   - Using AWS EKS - Managed Kubernetes Cluster
2. Github Actions: Used for CI part
   - builds docker image based on given src files path
   - authenticates and pushes to AWS ECR
   - updates the latest docker image tag in kubernetes manifest file
3. ArgoCD: Used for CD part
   - each ArgoCD application fetches Kubernetes files from given URL/folder in repo
   - updates service every 3 minutes(auto sync enabled)
2. Nginx Ingress Controller: Configures HTTP load balancer using AWS ELB
   - Using single load balancer to deliver multiple microservices. 
   - Each microservice route configuration is mentioned through Ingress resource
3. External-DNS: 
   - Generates automatic DNS entries for services/ingress resources created in the cluster
   - Removes the hassle to create DNS entries for each service
   - Hosted zone name, frequency etc. are customizable.
4. Prometheus Grafana
   - For monitoring Kubernetes cluster resources


# Next improvements

1. Installing EFK for logging purpose
2. Updating Github actions workflow to use IAM role instead of Github secrets
3. Using AWS KMS for secret management
4. Automatic Infrastructure creation based on IaC using Terraform