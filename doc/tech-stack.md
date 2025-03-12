# k8or Orbit Technology Stack

This document outlines the technology stack used for the **k8or Orbit** application.

## Diagram: [View](https://github.com/k8or-orbit/blob/k8or-dev/doc/diagram/k8or-orbit-component-high-level-architecture-diagram-v0-0-11-dia-k8d.pdf)

![Alt text](https://github.com/k8or-orbit/blob/k8or-dev/doc/diagram/k8or-orbit-component-high-level-architecture-diagram-v0-0-11-dia-k8d.jpg)

## Frontend

* **Languages:**
    * HTML
    * CSS
    * JavaScript
    * TypeScript
* **Framework/Library:** Angular
* **UI Library:** None

## Backend

* **Languages:** PHP
* **Frameworks:** Slim, WordPress,

## Database

* **Types:** Relational, NoSQL, Object Storage
* **Specific Databases:** MySQL, DynamoDB, Amazon S3

## Infrastructure

* **Cloud Providers:** AWS, DigitalOcean
* **Containerization:** Docker
* **Orchestration:** Kubernetes

## DevOps

* **Version Control:** Git
* **Repository Hosting:** GitLab
* **CI/CD:** GitLab CI/CD, Argo CD
* **Infrastructure as Code:** None

## Other Tools and Technologies (Based on MSVs):

* **Kubernetes Manifest Transformations:** Kustomize
* **Container Image Validation:** Trivy
* **Data Management/Routing:** NATS

## Rationale

* **Frontend (Angular, TypeScript):** We chose Angular and TypeScript for the frontend to build a robust and maintainable user interface. Angular's comprehensive framework and TypeScript's strong typing provide a solid foundation for developing complex features with a focus on scalability and code quality.

* **Backend (PHP, Slim, WordPress, and more):** PHP was selected for its versatility and widespread support, allowing for rapid development and integration with existing systems. Slim was chosen as a lightweight micro-framework to build APIs efficiently, while WordPress is utilized for content management and flexibility where needed. The specific backend languages and frameworks may vary depending on the microservice.

* **Database (MySQL, DynamoDB, S3):** We employ a combination of database solutions to address various data storage needs. MySQL is used for relational data, providing structured data management. DynamoDB is utilized for NoSQL workloads, enabling high-performance and scalable data storage. S3 buckets are employed for object storage, facilitating the management of files and large data assets.

* **Infrastructure (AWS, DigitalOcean, Docker, Kubernetes):** AWS and DigitalOcean are used for their reliable and scalable cloud infrastructure, providing flexibility in deploying and managing our application. Docker is utilized for containerization, ensuring consistent and portable deployments. Kubernetes is employed for orchestrating our containers, enabling efficient scaling and management of our application in a distributed environment.

* **DevOps (Git, GitLab, Argo CD):** Git is used for version control, facilitating collaboration and tracking changes. GitLab is employed for CI/CD pipelines, automating the build, test, and deployment process. Argo CD is used for GitOps-based continuous delivery, ensuring that our Kubernetes deployments are synchronized with our Git repositories.

* **Kustomize:** Used for Kubernetes manifest transformations, enabling flexible and tailored deployments within the k8or Orbit environment.

* **Container Image Validation (Trivy):** Trivy is used for comprehensive security scanning of container images. It identifies vulnerabilities in OS packages and application dependencies, ensuring that uploaded images are safe and secure for deployment.

* **Data Management/Routing (NATS):** NATS is a lightweight and high-performance messaging system used for data management and routing between microservices. It provides fast and reliable communication, supporting various messaging patterns and ensuring efficient data flow within the k8or Orbit platform.