# pingpong-app

The provided Kubernetes manifests describe a solution for deploying, securing, and scaling a simple ping/pong service consisting of two microservices (pingpong-a and pingpong-b) running in separate namespaces.

Namespaces
- Purpose: Isolates resources for pingpong-a and pingpong-b.
- Defined Namespaces:
- pingpong-a: For resources related to the pingpong-a service.
-	pingpong-b: For resources related to the pingpong-b service.


# Deployments
-	Purpose: Manages the lifecycle of Pods for pingpong-a and pingpong-b.
-	Key Details:
-	Image: Uses ping-pong:test.
-	Environment Variables:
-	TARGET: Points each service to the other, enabling ping/pong communication.
-	TICK_MS: Defines the tick interval for sending pings.
-	METRICS__PATH and METRICS__PORT: For exposing metrics.
-	SERVICE__PORT: Specifies the service port.
	Probes:
-	Readiness Probe: Ensures the service is ready to handle traffic.
-	Liveness Probe: Monitors the service’s health.
-	Startup Probe: Ensures the service starts correctly.
-	Resource Limits:
-	Requests: 128Mi memory, 250m CPU.
-	Limits: 256Mi memory, 500m CPU.
-	Security Context:
-	Runs containers as non-root users (runAsNonRoot: true).
-	Drops unnecessary capabilities (e.g., SYS_ADMIN).
-	Prevents privilege escalation (allowPrivilegeEscalation: false).


# Services
-	Purpose: Exposes the pingpong-a and pingpong-b Deployments within their respective namespaces.
-	Key Details:
-	Ports:
-	Service Port: 8080 (maps to containerPort 8080).
-	Protocol: TCP.


# Horizontal Pod Autoscalers (HPA)
-	Purpose: Automatically scales the number of Pods based on CPU utilization.
-	Key Details:
-	Metrics: Targets 50% CPU utilization.
-	Scaling Range:
-	Minimum Pods: 2.
-	Maximum Pods: 10.


# Network Policies
-	Purpose: Controls network traffic to enhance cluster security.
-	Defined Policies:
1.	DNS Egress Policy:
-	Allows Pods to access DNS services (kube-dns) for name resolution.
2.	PingPong Communication Policy:
-	Enables communication between pingpong-a and pingpong-b:
-	Traffic from pingpong-a to pingpong-b.
-	Traffic from pingpong-b to pingpong-a.
-	Uses port 8080 over TCP.

# Open Policy Agent (OPA) Constraints
-	Purpose: Enforces security best practices.
-	Defined Constraint:
-	K8sRequiredNonRootUser:
-	Ensures all Pods run as non-root users by enforcing securityContext.runAsNonRoot: true.

# Service Mesh:
-	Added Istio for traffic management, observability, and security. 


