# manifest.yaml
The manifest.yaml represents a production-grade Kubernetes deployment configuration for two services (pingpong-a and pingpong-b). Below is an explanation of each section of the manifest, focusing on the elements that are relevant for a production-grade setup:

## 1. Namespace Definitions :

	apiVersion: v1
    kind: Namespace
    metadata:
        name: pingpong-a

Separates resources for pingpong-a into its own namespace. This helps with isolation, access control, and organization.
Production Consideration: Namespaces are critical for multi-tenant environments, ensuring that resources are logically separated.

## 2. Deployment for pingpong-a Application:

    apiVersion: apps/v1
    kind: Deployment
    metadata:
        name: pingpong-a
        namespace: pingpong-a


Production Consideration:
    Replicas: In a production setup, you’d typically scale this up to at least 3 replicas for high availability.
    Health Probes: Readiness, liveness, and startup probes ensure that the application is functioning properly. These are crucial for maintaining service reliability and availability in production.
    Resource Requests and Limits: These ensure that the pod gets the right amount of CPU and memory resources while preventing it from over-consuming the cluster’s resources. In production, resource requests and limits are essential to avoid resource contention.  

    resources:  
        requests:
            memory: "128Mi"
            cpu: "250m"
        limits:
            memory: "256Mi"
            cpu: "500m"

It Specifies resource limits to prevent overconsumption and ensure fair resource distribution.
Production Consideration: 
    It’s critical to define resource requests and limits based on the actual application requirements to avoid issues like OOM kills or CPU starvation.

## 3. Security Context for pingpong-a

    securityContext:
        allowPrivilegeEscalation: false
        capabilities:
            drop:
                - SYS_ADMIN
        privileged: false
        runAsGroup: 3000
        runAsNonRoot: true
        runAsUser: 1000

Enforces security best practices by restricting container privileges.

Production Consideration: 
    Running containers as non-root users and avoiding privilege escalation are critical for reducing the risk of container breakout vulnerabilities.

## 4. Service to Expose pingpong-a

    apiVersion: v1
    kind: Service
    metadata:
        name: pingponger-a
        namespace: pingpong-a
    spec:
        selector:
            app.kubernetes.io/name: pingpong-a
        ports:
            - name: http
              protocol: TCP
              port: 8080
              targetPort: 8080

Exposes the pingpong-a deployment to the network so other services can access it.
Production Consideration: 
    Services are essential for internal and external communication within the Kubernetes cluster. In production, you may also want to configure additional service types (like LoadBalancer or ClusterIP), depending on the use case.              


## 5. Horizontal Pod Autoscaler for pingpong-a

    apiVersion: autoscaling/v2
    kind: HorizontalPodAutoscaler
    metadata:
        name: pingpong-a-hpa
        namespace: pingpong-a
    spec:
        scaleTargetRef:
            apiVersion: apps/v1
            kind: Deployment
            name: pingpong-a
        minReplicas: 2
        maxReplicas: 10
        metrics:
            - type: Resource
              resource:
                name: cpu
                target:
                    type: Utilization
                    averageUtilization: 50

Automatically scales the number of pingpong-a pods based on CPU utilization.
Production Consideration: 
    Autoscaling is crucial for handling variable traffic loads. The minimum and maximum replica settings ensure that the app remains responsive and cost-efficient.


## Considerations for Production-Grade:

Scalability: The replicas and Horizontal Pod Autoscalers allow the application to scale and manage variable load based on CPU usage.

Their Resilience: Liveness, Readiness, and Startup Probes allow the application to be resilient to failure, with Kubernetes taking the challenge of automatically restarting unhealthy pods.

Security: Security context enforces running containers as non-root, as well as dropping unused capabilities, shrinking the attack surface. Network policies allow only required communication between services.

Resource Requests and Limits Definition: To ensure that the application does not starve of or over-consume resources, you define resource requests and limits.

Isolation: Namespaces and network policies allow you to isolate resources and control access within the cluster, which helps in multi-tenant

Overall, this manifest is taking into account good production practice but might require some tuning for high traffic scenario or require stronger security requirements. Other considerations (like monitoring/logging, fault tolerance...) might be included for a full production-grade setup..