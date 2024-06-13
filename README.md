## Prerequisites

Before you begin, ensure you have met the following requirements:
- Docker
- Kubernetes
- kubectl

## Commands
- docker compose up --build

- kubectl apply -f client-deployment.yaml
- kubectl apply -f client-service.yaml
- kubectl apply -f server-deployment.yaml
- kubectl apply -f server-service.yaml

- kubectl get deployments
- kubectl get services

- kubectl get pods


- kubectl delete -f client-deployment.yaml
- kubectl delete -f server-deployment.yaml
- kubectl apply -f client-deployment.yaml
- kubectl apply -f server-deployment.yaml
- http://localhost:31535




