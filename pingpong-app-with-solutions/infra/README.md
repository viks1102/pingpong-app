# manifest.yaml
The manifest.yaml represents a production-grade Kubernetes deployment configuration for two services (pingpong-a and pingpong-b). Below is an explanation of each section of the manifest, focusing on the elements that are relevant for a production-grade setup:

## 1. Namespace Definitions :

	apiVersion: v1
    kind: Namespace
    metadata:
        name: pingpong-a

	•	Purpose: Separates resources for pingpong-a into its own namespace. This helps with isolation, access control, and organization.
	•	Production Consideration: Namespaces are critical for multi-tenant environments, ensuring that resources are logically separated.

## 2. Deployment for pingpong-a Application:

    apiVersion: apps/v1
    kind: Deployment
    metadata:
        name: pingpong-a
        namespace: pingpong-a

    •	Purpose: Defines the pingpong-a application deployment. Specifies 1 replica (for initial testing) and the container image to be used.
	•	Production Consideration:
	•	Replicas: In a production setup, you’d typically scale this up to at least 3 replicas for high availability.
	•	Health Probes: Readiness, liveness, and startup probes ensure that the application is functioning properly. These are crucial for maintaining service reliability and availability in production.
	•	Resource Requests and Limits: These ensure that the pod gets the right amount of CPU and memory resources while preventing it from over-consuming the cluster’s resources. In production, resource requests and limits are essential to avoid resource contention.   