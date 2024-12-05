# Kafka Cluster with TLS Encryption on Kubernetes

## Overview
This setup deploys a minimal Kafka cluster with the following:
- TLS encryption using `cert-manager`
- Producer and consumer securely communicating over SSL
- Persistent storage for Kafka and Zookeeper

## Prerequisites
- Kubernetes cluster (e.g., Minikube)
- Helm installed for `cert-manager`
- kubectl CLI configured for your cluster

## Deployment Steps
1. **Start Minikube**:
   ```bash
   minikube start
    ```
### Install Cert-Manager:

```bash
kubectl apply -f cert-manager/
```
### Deploy Kafka:

```bash
kubectl apply -f kafka/
```
Deploy Producer & Consumer:

```bash
kubectl apply -f producer-consumer/
```
### Verify TLS Setup:
- Verify Kafka logs for successful TLS setup.
- Use the producer and consumer pods to send and receive messages on test-topic.

## Customization
- Update secrets in secrets/kafka-tls.yaml with your Base64-encoded certificates.
- Adjust resource limits in deployment files for production use.


### Additional steps (pre-requisites)
Installs all necessary resources, including the CustomResourceDefinitions and the cert-manager, cainjector, and webhook components, without requiring Helm

Apply CRDs
```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.16.1/cert-manager.crds.yaml
```
Install all cert-manager components using the release manifest:
```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.16.1/cert-manager.yaml
```
List all installed components
```bash
kubectl get all -n cert-manager
```