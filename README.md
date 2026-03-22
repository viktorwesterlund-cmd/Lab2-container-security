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
