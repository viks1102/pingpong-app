# This YAML manifest defines a ConstraintTemplate for Open Policy Agent (OPA) Gatekeeper. It uses a policy that enforces the runAsNonRoot field to be true in the Pod security context of all containers running in Kubernetes Pods. Here is a breakdown of the manifest, with sections labelled:

## How It Works:

1) ConstraintTemplate: 

    Describes the logic and schema for a particular policy.

2) Custom Resource Definition:

    A CRD (K8sRequiredNonRootUser) is instantiated using the template.

3) User Constraints:

    Admins create constraints through K8sRequiredNonRootUserresources that define where and how the policy should be applied.

4) Policy Enforcement:

    Gatekeeper evaluates the constraint when a Pod is created or updated.
    If a container lacks the runAsNonRoot: true field in its security context, the Pod is rejected.     


# The YAML manifest is a Constraint (run-as-non-root-constraint.yaml) enforces the ConstraintTemplate K8sRequiredNonRootUser. It applies the non-root user policy to specific Kubernetes resources, in this case, Pods. 

## How it works:

1) This constraint is associated with the label k8sRequiredNonRootUser from the ConstraintTemplate K8sRequiredNonRootUser.
2) Whenever a Pod is created or updated, Gatekeeper will:
    a. Know the containers in the Pod.
    b. Verify if the runAsNonRoot: true field is included in the container’s securityContext
    c. Reject the Pod if the field is not set or it is set to false.
