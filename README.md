Lab 2 – Container Security
In this lab, I worked with container security by analyzing a vulnerable Docker image, hardening it, and enforcing security policies. The goal was to understand how to reduce vulnerabilities and improve the security of containerized applications.


Tools Used
Docker – for building and running containers
Trivy – for vulnerability scanning and SBOM generation (via Docker)
GitHub – for version control and submission
OPA Gatekeeper – for Kubernetes policy enforcement

ulnerable Container

The initial container was intentionally insecure:

Used an outdated base image (python:3.8)
Ran as root user
Used an old dependency (flask==1.0.0)
No health checks
Larger attack surface

I scanned the image using Trivy:

trivy image my-app:vulnerable > scan-before.txt

The results showed multiple HIGH and CRITICAL vulnerabilities.

Hardened Container

I improved the Dockerfile to follow best practices:

Switched to a smaller and updated base image (python:3.12-slim-bookworm)
Created and used a non-root user
Updated dependencies (flask==3.0.0)
Added a HEALTHCHECK
Reduced image size with --no-cache-dir

Built and scanned:

docker build -f Dockerfile.hardened -t my-app:hardened .
trivy image my-app:hardened > scan-after.txt

The hardened version had significantly fewer vulnerabilities.

 SBOM (Software Bill of Materials)

I generated an SBOM using Trivy:

trivy image --format cyclonedx --output sbom.json my-app:hardened

The SBOM provides a full list of all components and dependencies in the container.

This is important for:

Identifying vulnerable components quickly
Meeting compliance requirements (e.g. EU Cyber Resilience Act)
Improving supply chain security
 Policy Enforcement with Gatekeeper

I used Gatekeeper to enforce security rules in Kubernetes.

Examples of enforced policies:

Containers must not run as root
Containers must not use latest tag
Required labels must be set
Containers must drop capabilities

I tested:

A non-compliant (bad) pod → denied
A compliant (hardened) pod → allowed
📸 Screenshots

The following screenshots are included in the repository:
<img width="1769" height="212" alt="scan output" src="https://github.com/user-attachments/assets/3aa4488e-1a81-4974-a6ca-4a1631f68821" />
<img width="1764" height="194" alt="trivy output" src="https://github.com/user-attachments/assets/b991d298-8465-4a97-9520-b59a895fe5b8" />

<img width="1255" height="712" alt="bad pod" src="https://github.com/user-attachments/assets/956c5d6c-83b3-451b-9469-38fafbc83a28" />
<img width="1038" height="281" alt="hardenpod" src="https://github.com/user-attachments/assets/639ca511-0453-4639-b708-17a5e4109f44" />
Reflection

I learned that container security is largely about reducing the attack surface, for example by using smaller and updated base images and avoiding running applications as root. I also saw how outdated dependencies quickly introduce vulnerabilities, and how tools like Trivy make it easy to detect them.

SBOM is important because it provides a clear overview of all components and libraries included in a container. This makes it easier to identify vulnerabilities and understand what needs to be updated, especially in larger projects.

Policy enforcement with Gatekeeper changes how you work with Kubernetes because security is no longer something you think about afterwards. Instead, rules are enforced directly in the cluster, meaning insecure resources are automatically blocked. This makes the workflow more structured and reduces the risk of human error.

Conclusion

By hardening the Dockerfile, scanning with Trivy, generating an SBOM, and enforcing policies with Gatekeeper, I significantly improved the security of the containerized application and gained practical experience with modern DevSecOps practices.
