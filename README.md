# k8or Orbit

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Welcome to the k8or Orbit open-source project! This repository serves as the central hub for the k8or Orbit platform, providing a comprehensive overview of the project, its architecture, and how to contribute.

## Diagram: [View](https://github.com/k8or-orbit/k8or-orbit/blob/k8or-dev/documentation/diagram/k8or-orbit-component-high-level-architecture-diagram-v0-0-13-dia-k8d.pdf)

![Alt text](https://github.com/k8or-orbit/k8or-orbit/blob/k8or-dev/documentation/diagram/k8or-orbit-component-high-level-architecture-diagram-v0-0-13-dia-k8d.jpg)

## What is k8or Orbit?

k8or Orbit is a Kubernetes-native platform designed to streamline the deployment, management, and scaling of containerized applications. It provides a user-friendly interface and a set of tools to simplify the complexities of Kubernetes, making it accessible to developers and operators of all levels.

**Key Features:**

* **Simplified Deployments:** Easily deploy and manage applications on Kubernetes using intuitive workflows and pre-built templates.
* **Streamlined Workflows:** Automate common tasks and integrate with existing tools to accelerate your development process.
* **Enhanced Visibility:** Gain insights into your Kubernetes deployments with real-time monitoring, logging, and visualization tools.
* **Scalability and Resilience:** Design and deploy applications that can scale seamlessly and withstand failures.
* **Open Source and Extensible:** Built on open-source technologies and designed to be extensible, allowing you to customize and adapt it to your specific needs.

## Project Structure

k8or Orbit is built as a collection of microservices, each responsible for a specific functionality. The source code for these microservices is hosted in separate repositories within the `k8or-orbit` GitHub organization. This repository provides an overview of the project and links to the individual microservice repositories.

**Microservices:**

* **Static Page Functionality:**
    * **[Orbit Static Portal](https://github.com/k8or-orbit/orbit-static-portal/tree/k8or-dev)**: Serves as the initial entry point for users, hosting the static pages that introduce k8or Orbit.
* **Deploy Functionality:**
    * **[Orbit Deploy Orchestrator](https://github.com/k8or-orbit/orbit-deploy-orchestrator/tree/k8or-dev)**: Acts as the control center for deploying applications and services within the k8or Orbit environment.
    * **[Orbit SyncMaster](https://github.com/k8or-orbit/orbit-syncmaster/tree/k8or-dev)**: Responsible for maintaining synchronization between different components and data sources within k8or Orbit.
* **Transfer Functionality:**
    * **[Orbit Data Shuttle](https://github.com/k8or-orbit/orbit-data-shuttle/tree/k8or-dev)**: Functions as a data transport mechanism, facilitating the secure and efficient movement of data between various microservices.
* **Search Functionality:**
    * **[Orbit Board Navigator](https://github.com/k8or-orbit/orbit-board-navigator/tree/k8or-dev)**: Empowers users to effectively navigate and explore the various boards and visualizations available within k8or Orbit.
    * **[ViewPort](https://github.com/k8or-orbit/viewport/tree/k8or-dev)**: Serves as a bridge between the frontend and the WordPress database, handling WordPress functionality.
* **Upload Functionality:**
    * **[Orbit Upload Orchestrator](https://github.com/k8or-orbit/orbit-upload-orchestrator/tree/k8or-dev)**: Manages the upload process, handling various data types from the upload form and intelligently routing them.
    * **[Orbit Kustomize Transformer](https://github.com/k8or-orbit/orbit-kustomize-transformer/tree/k8or-dev)**: Leverages Kustomize to apply transformations and customizations to Kubernetes configurations.
    * **[Orbit Data Manager](https://github.com/k8or-orbit/orbit-data-manager/tree/k8or-dev)**: Provides comprehensive data management capabilities, handling storage, retrieval, and manipulation of various data types.
    * **[Orbit Artifact Uploader](https://github.com/k8or-orbit/orbit-artifact-uploader/tree/k8or-dev)**: Specializes in uploading artifacts, such as container images and deployment packages.
    * **[Orbit Image Validator](https://github.com/k8or-orbit/orbit-image-validator/tree/k8or-dev)**: Plays a role in ensuring the quality and security of uploaded application images.

## Contributing

We welcome contributions from the community! If you're interested in contributing to k8or Orbit, please see our [Contribution Guidelines](CONTRIBUTING.md) for detailed instructions.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contact

If you have any questions or feedback, please feel free to reach out to us:

* **GitHub Issues:** Use the issue tracker in this repository to report bugs or suggest features.
* **Discussions:** Join the [k8or Orbit Discussions](link-to-discussions) to engage with the community and ask questions.

We appreciate your interest in k8or Orbit!