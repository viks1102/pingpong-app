# Ingress demonstration 
This document outlines the steps to configure the PingPong App using Kubernetes Ingress.

## Install nginx-ingress controller :

	•	Purpose: Deploy the NGINX Ingress controller in the Kubernetes cluster to handle HTTP and HTTPS traffic routing.
	•	Outcome: A Deployment of the NGINX Ingress controller is created in the cluster, enabling the use of Ingress resources.

To enable Ingress in your cluster, install the NGINX Ingress controller by running:

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

This will deploy the nginx-ingress controller as a Deployment in your cluster.

## Create Deployments

	•   Purpose: Deploy the PingPong application as two separate services (pingpong-a and pingpong-b) for demonstration purposes.
	•	Outcome: Two Deployments and their associated Services are created using manifest.yaml.

Deploy the PingPong services for demonstration. Use the following commands to create the Deployments:
```
kubectl apply -f manifest.yaml 

```

This commands create two Deployments (pingpong-a and pingpong-b) with their respective container images and services. 


## Create Ingress resource

	•	Purpose: Configure an Ingress resource to route traffic to the PingPong Services (pingpong-a and pingpong-b) with ssl.
	•	Outcome: The Ingress resource is created, allowing ssl based routing.

Now that you have created Deployments and Services, you can create an Ingress resource to route traffic with ssl. To create the Ingress resource, run the following command:

```
kubectl apply -f ingress-resource.yaml
```

This will create an Ingress resource. 

## Install certificate manager

	•	Purpose: Install cert-manager, a Kubernetes add-on for automating the management of TLS certificates, to enable HTTPS for the Ingress.
	•	Outcome: Cert-manager is deployed as a set of resources in the cluster.

To enable HTTPS for your Ingress, install the cert-manager: I am using a stable version v1.12.14.

```
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.12.14/cert-manager.yaml
```

This will install the cert-manager as a Deployment in your cluster.

## Create Clusterissuer

	•	Purpose: Define ClusterIssuers (staging and production) to request SSL certificates for securing the Ingress using HTTPS.
	•	Outcome: Two ClusterIssuers are created to manage TLS certificate issuance for both testing and production environments.

Set up ClusterIssuers to request SSL certificates for the Ingress. Run the following commands to create the staging and production Clusterissuers:

```
kubectl apply -f staging_issuer.yaml
kubectl apply -f prod_issuer.yaml
```

These commands will create two Clusterissuers, one for staging and one for production.

#### To view ingress
```
kubectl get ing
```
#### To describe ingress
```
kubectl describe ing <ing-name>
```
#### To view clusterissuer
```
kubectl get clusterissuer
```
#### To view certificate
```
kubectl get certificate
```
#### To describe certificate
```
kubectl describe certificate
```

