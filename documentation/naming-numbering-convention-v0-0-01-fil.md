## Introduction

This document outlines the naming and numbering conventions used within the k8or Orbit application. Its primary purpose is to establish a consistent and structured approach for identifying and referencing various elements of the k8or Orbit application, including microservices, backend resources, process flows, and individual process steps. 

The scope of this document encompasses all aspects of the k8or Orbit application related to naming and numbering, specifically:

*  **Microservices:** Naming conventions for both frontend and backend microservices.
*  **Backend Resources:** Naming conventions for databases (DynamoDB, MySQL), storage buckets (S3), and other persistent resources.
*  **Flows:** Numbering conventions for distinct flows within the k8or Orbit application.
*  **Process Steps:** Identification and numbering of individual steps within each flow, including the use of Process IDs (PIDs).
*  **Components:** Logical groupings of resources and microservices and their associated numbering.
*  **Legs and Parts:** Structure for defining specific interactions and operations within a process.
*  **Transfer Functionality:** Detailed conventions and processes related to image transfer operations.

---

## Core Concepts and Definitions

This section defines the key terms and concepts used throughout this document to ensure a shared understanding of the naming and numbering conventions.

*  **Leg:** A "Leg" represents a distinct stage or interaction within a larger functionality, often involving communication between the Frontend User, Frontend/Backend Microservices, and Backend Resources. Each Leg focuses on a specific aspect of the overall functionality (e.g., static data retrieval, image validation, metadata storage). The image upload functionality, for example, is broken down into multiple Legs to represent the different phases of the upload.

*  **Part:** A "Part" is a subdivision of a Leg, detailing a specific operation or step within that stage. Each Part has a defined Start Resource and End Resource, along with a sequence of actions. Parts allow for a granular breakdown of complex Legs into manageable and trackable units.

*  **Flow:** A "Flow" represents a complete sequence of actions or operations, spanning multiple Legs. A Flow describes the end-to-end execution of a specific functionality, from initiation to completion. For example, the "Image Upload Flow" might encompass several Legs, each handling a different part of the overall upload.

*  **Component:** A "Component" is a logical grouping of related resources and microservices. Components serve to organize the k8or Orbit application into functional units.

*  **Microservice:** A "Microservice" is a small, independent service within a Component, designed to perform a specific function. Microservices communicate with each other and with backend resources to achieve the overall functionality of the Component. Microservices can be categorized as either Frontend Microservices (FMS), handling user interface interactions, or Backend Microservices (BMS), implementing the core logic and interacting with backend resources.

*  **Resource:** A "Resource" refers to a backend element that the microservices interact with. Resources are persistent elements like databases (e.g., DynamoDB, MySQL), storage buckets (e.g., S3), or message queues.

*  **Process ID (PID):** A "Process ID" is a unique identifier assigned to each step or operation (Part) within a flow. The PID follows a structured format (`<start-resource>-<end-resource>-e<execution_order>`) and is used for tracking and analyzing individual functionality steps.

*  **Microservice Identification (MID):** A "Microservice Identification" string is a unique identifier assigned to each microservice (e.g., `c8fms03`). The MID includes a component prefix and a functional identifier, making it easy to identify the microservice's role and location within the k8or Orbit application.

*  **Resource Identification (RID):** A "Resource Identification" string is a unique identifier assigned to each resource (e.g., `c20bre03`). The RID includes a component prefix and a resource type and identifier, providing a clear way to identify and reference each resource.

---

## Flow Numbering Convention

This section describes the convention used for numbering process flows within the k8or Orbit application. Consistent flow numbering is important for organizing and referencing the various functionalities within the k8or Orbit application.

* **Base and Increment:** Flow numbering begins with `flow-4` and increments by 4 for each subsequent flow. This means that the initial flow is designated as `flow-4`, the next flow is `flow-8`, then `flow-12`, `flow-16`, and so on.

* **Reserved Numbers:**  Numbers between assigned flow numbers are reserved for future expansion and the addition of new flows. This reservation strategy ensures that new flows can be easily integrated without disrupting the existing numbering sequence. For example, the numbers `flow-5`, `flow-6`, and `flow-7` are reserved for potential future flows between `flow-4` and `flow-8`. This pattern of reserving numbers between assigned flows is consistently applied across all flow ranges.

* **Example:**

Flow numbering within the k8or Orbit application begins with `flow-4` and increments by 4 for each subsequent flow. Numbers between assigned flows are reserved for future use, maintaining a structured and expandable numbering system. Each flow is associated with specific repositories (Infrastructure, Application, Data, Population, Testing, and State) on GitHub, enabling version control and collaboration.

Here are examples of assigned flows and their associated components and repositories:

*   **`flow-4`:** This flow is associated with the `Portal` component (C4 and C8). It covers image related processes such as uploading, searching, deploying, transferring and others. The repositories for this flow are:
    *   Infrastructure Repository: [flow-4-infrastructure-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-4-infrastructure-rep-k8d-aws/tree/k8or-dev)
    *   Application Repository: [flow-4-application-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-4-application-rep-k8d-aws/tree/k8or-dev)
    *   Data Repository: [flow-4-data-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-4-data-rep-k8d-aws/tree/k8or-dev)
    *   Population Repository: [flow-4-population-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-4-population-rep-k8d-aws/tree/k8or-dev)
    *   Testing Repository: [flow-4-testing-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-4-testing-rep-k8d-aws/tree/k8or-dev)
    *   State Repository: [flow-4-state-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-4-state-rep-k8d-aws/tree/k8or-dev)

*   **`flow-8`:** This flow pertains to the `Manifestor` component (C12). It handles processes related to manifest generation. The repositories for this flow are:
    *   Infrastructure Repository: [flow-8-infrastructure-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-8-infrastructure-rep-k8d-aws/tree/k8or-dev)
    *   Application Repository: [flow-8-application-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-8-application-rep-k8d-aws/tree/k8or-dev)
    *   Data Repository: [flow-8-data-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-8-data-rep-k8d-aws/tree/k8or-dev)
    *   Population Repository: [flow-8-population-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-8-population-rep-k8d-aws/tree/k8or-dev)
    *   Testing Repository: [flow-8-testing-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-8-testing-rep-k8d-aws/tree/k8or-dev)
    *   State Repository: [flow-8-state-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-8-state-rep-k8d-aws/tree/k8or-dev)

*   **`flow-12`:** This flow is dedicated to the `ImageHarbor` component (C16), managing interactions with the Harbor Image storage. The repositories for this flow are:
    *   Infrastructure Repository: [flow-12-infrastructure-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-12-infrastructure-rep-k8d-aws/tree/k8or-dev)
    *   Application Repository: [flow-12-application-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-12-application-rep-k8d-aws/tree/k8or-dev)
    *   Data Repository: [flow-12-data-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-12-data-rep-k8d-aws/tree/k8or-dev)
    *   Population Repository: [flow-12-population-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-12-population-rep-k8d-aws/tree/k8or-dev)
    *   Testing Repository: [flow-12-testing-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-12-testing-rep-k8d-aws/tree/k8or-dev)
    *   State Repository: [flow-12-state-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-12-state-rep-k8d-aws/tree/k8or-dev)

*   **`flow-16`:** This flow is associated with the `ChartStore` component (C20, C24, and C28), and handles operations related to the Helm charts storage. The repositories for this flow are:
    *   Infrastructure Repository: [flow-16-infrastructure-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-16-infrastructure-rep-k8d-aws/tree/k8or-dev)
    *   Application Repository: [flow-16-application-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-16-application-rep-k8d-aws/tree/k8or-dev)
    *   Data Repository: [flow-16-data-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-16-data-rep-k8d-aws/tree/k8or-dev)
    *   Population Repository: [flow-16-population-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-16-population-rep-k8d-aws/tree/k8or-dev)
    *   Testing Repository: [flow-16-testing-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-16-testing-rep-k8d-aws/tree/k8or-dev)
    *   State Repository: [flow-16-state-rep-k8d-aws](https://github.com/k8or-orbit-aws/flow-16-state-rep-k8d-aws/tree/k8or-dev)

Numbers such as `flow-5`, `flow-6`, `flow-7` (between `flow-4` and `flow-8`), `flow-9`, `flow-10`, `flow-11` (between `flow-8` and `flow-12`), and so on, are reserved for future flows within the k8or Orbit application. This structured approach to flow numbering ensures maintainability and scalability as the application evolves.

---

## Block Numbering Convention

This section details the convention used for numbering blocks within a flow. Blocks are used to manage and allocate resources or processing units within a component. The block numbering convention ensures that resources are assigned systematically and avoids conflicts.

* **Component-Specific Reservation:** Each component within a flow reserves four consecutive block numbers. These reserved blocks are specifically allocated to that component and are not available for other components within the same flow. This ensures that each component has dedicated resources and prevents resource contention.

* **Reserved Block Pattern:** The block numbers are reserved in a specific pattern. This pattern is consistent across all components and flows, making it easy to predict which block numbers are associated with a given component. The pattern for the first eight components is as follows:

    *  First Component: 3, 6, 9, 12
    *  Second Component: 15, 18, 21, 24
    *  Third Component: 27, 30, 33, 36
    *  Fourth Component: 39, 42, 45, 48
    *  Fifth Component: 51, 54, 57, 60
    *  Sixth Component: 63, 66, 69, 72
    *  Seventh Component: 75, 78, 81, 84
    *  Eighth Component: 87, 90, 93, 96

    This pattern shows that the first component reserves blocks 3, 6, 9, and 12. The second component then reserves blocks 15, 18, 21, and 24, and so on.

*   **Example:**

    Let's consider a `flow-8` that involves three components: Component C4, Component C8, and Component C12.

    *   Component C4 (within `flow-8`) would reserve blocks 3, 6, 9, and 12. These blocks are exclusively assigned to C4 within `flow-8`.
    *   Component C8 (within `flow-8`) would reserve blocks 15, 18, 21, and 24. These blocks are exclusively assigned to C8 within `flow-8`.
    *   Component C12 (within `flow-8`) would reserve blocks 27, 30, 33, and 36. These blocks are exclusively assigned to C12 within `flow-8`.

    If `flow-8` needed to add a fourth component, it would reserve blocks 39, 42, 45, and 48. This pattern continues for subsequent components.

    It's important to note that these block reservations are *specific to a given flow*. If the same component (e.g., C4) appears in a *different* flow (e.g., `flow-12`), it would reserve a *new* set of blocks according to the same pattern. For instance, Component C4 in `flow-12` might reserve blocks 3, 6, 9, and 12 *again*, because the block allocation is per flow instance of the component.

---

## Component Numbering Convention

This section describes the convention used for numbering components within the k8or Orbit application. Consistent component numbering is essential for organizing the k8or Orbit application's architecture and managing resources effectively.

*  **Base and Increment:** Component numbering begins with `C4` and increments by 4 for each subsequent component within the k8or Orbit application. This means the first component is designated as `C4`, the second as `C8`, the third as `C12`, the fourth as `C16`, and so on.

*  **Flow Component Limit:** A single flow can accommodate a maximum of eight components. This limitation is imposed by the available block numbering range and helps to maintain manageable and well-defined flows.

*  **Flow Division:** If a process requires more than eight components, the process *must* be divided into multiple smaller flows. Each flow will then have its own set of components, numbered according to the `C4` incrementing pattern. This ensures that no single flow becomes overly complex and that the eight-component limit is adhered to.

*  **Component Numbering Scheme (Regional):** While the `C4` incrementing pattern applies within the k8or Orbit application, the *initial* component number assigned to a component can also be influenced by the region where the associated infrastructure resources are located. This regional scheme helps to further organize components across different deployment environments:

    *  **N. Virginia Region:** Infrastructure resources launched in the N. Virginia region are assigned component numbers from 1 to 100. So, a component in this region might be `C4`, `C8`, `C12`, etc., up to `C100`.

    *  **Ohio Region - K3s Cluster Master Nodes:** K3s cluster master nodes and its components deployed in the Ohio region are assigned component numbers from 101 to 200. A component representing a K3s master node might be `C104`, `C108`, `C112`, etc.

    *  **Ohio Region - K3s Agent Nodes:** K3s agent nodes and its components deployed in the Ohio region are assigned component numbers from 201 to 300. A component representing a K3s agent node might be `C204`, `C208`, `C212`, etc.

---

## Microservice Identification (MID) Convention

This section details the convention used for identifying microservices within the k8or Orbit application. Consistent Microservice IDs (MIDs) are important for tracking, referencing, and managing the numerous microservices that comprise the k8or Orbit Application.

* **Structure:** A Microservice ID follows a specific structure: `c[Component Number][Microservice Type][Functional Identifier]`. For example, `c8fms03` is a typical Microservice ID.

* **Component Prefix:** The `c` prefix indicates that this is a Microservice ID. It is followed by the Component Number. For instance, in `c8fms03`, `c8` signifies that this microservice belongs to Component C8. This prefix links the microservice to its logical grouping within the k8or Orbit application.

* **Microservice Type:** Following the Component Number is a two or three-character code indicating the type of microservice:

    *  `fms`: Indicates a Frontend Microservice. These microservices handle user interface interactions and reside closer to the user. Example: `c8fms03` is a Frontend Microservice.
    *  `bms`: Indicates a Backend Microservice. These microservices implement the core business logic and interact with backend resources. Example: `c8bms06` is a Backend Microservice.

* **Functional Identifier:** The final part of the Microservice ID is a two-digit or three-digit number serving as a functional identifier. This number distinguishes individual microservices *within* the same component and of the same type. For example, `c8fms03` and `c8fms09` are both Frontend Microservices within Component C8, but the functional identifiers `03` and `09` distinguish them. The numbers are assigned sequentially as microservices are added to a component.

* **Example:**

    Let's break down a few example Microservice IDs:

    * `c8fms03`: This identifies a Frontend Microservice (`fms`) belonging to Component C8. The functional identifier `03` indicates that this is the first Frontend Microservice defined within Component C8.

    * `c8bms06`: This identifies a Backend Microservice (`bms`) belonging to Component C8. The functional identifier `06` indicates that this is the second Backend Microservice defined within Component C8.

    * `c12bms03`: This identifies a Backend Microservice (`bms`) belonging to Component C12. The functional identifier `03` indicates that this is the first Backend Microservice defined within Component C12.

The MID provides a clear and concise way to refer to each microservice and understand its relationship to other components and microservices in the k8or Orbit application.

---

## Resource Identification (RID) Convention

This section details the convention used for identifying backend resources within the k8or Orbit application. Consistent Resource IDs (RIDs) are essential for tracking, referencing, and managing the various resources used by the k8or Orbit application.

* **Structure:** A Resource ID follows a specific structure: `c[Component Number][Resource Type][Resource Identifier]`. For example, `c20bre03` is a typical Resource ID.

* **Component Prefix:** The `c` prefix indicates that this is a Resource ID. It is followed by the Component Number. For instance, in `c20bre03`, `c20` signifies that this resource belongs to Component C20. This prefix links the resource to its logical grouping within the k8or Orbit application.

* **Resource Type and Identifier:** Following the Component Number is a two or three-character code indicating the type of resource:

    * `bre`: Indicates a Backend Resource.

    Following the resource type is a two-digit or three-digit numerical identifier. This number distinguishes individual resources of the same type *within* the same component. For example, `c20bre03` and `c20bre06` are both Backend Resources belonging to Component C20, but the numerical identifiers `03` and `06` distinguish them. The numbers are assigned sequentially as resources are added to a component.

*   **Example:**

    Let's break down a few example Resource IDs:

    * `c20bre03`: This identifies a Backend Resource (`bre`) belonging to Component C20. The numerical identifier `03` indicates that this is the first Backend Resource defined within Component C20.

    * `c20bre06`: This identifies a Backend Resource (`bre`) belonging to Component C20. The numerical identifier `06` indicates that this is the second Backend Resource defined within Component C20.

    * `c16bre03`: This identifies a Backend Resource (`bre`) belonging to Component C16. The numerical identifier `03` indicates that this is the first Backend Resource defined within Component C16.

The RID provides a clear and concise way to refer to each resource and understand its relationship to other components and resources in the k8or Orbit application.

---

## Process ID (PID) Convention

This section describes the convention used for creating Process IDs (PIDs) within the k8or Orbit application. PIDs are critical for tracking and analyzing individual steps or operations (referred to as "Parts") within a process flow. They provide a granular level of detail for understanding the execution sequence and identifying the resources involved in each step.

* **Structure:** A Process ID follows the format: `<start-resource>-<end-resource>-e<execution_order>`.

* **Resource Identifiers:** The `<start-resource>` and `<end-resource>` portions of the PID represent the starting and ending resources for a given process step. Critically, these resource identifiers utilize the Microservice Identification (MID) convention for microservices and the Resource Identification (RID) convention for backend resources. This ensures consistency and allows for easy cross-referencing with the microservice and resource naming conventions.

* **Component ID Prefix:** Because MIDs and RIDs already include the component ID prefix, it is *implicitly* included in the PID. You do *not* need to repeat the component ID separately in the PID. The component context is already captured through the MIDs and RIDs used within the PID.

* **Execution Order:** The `-e<execution_order>` portion of the PID indicates the sequence of execution for the process step. The `e` prefix stands for "execution", and the following number represents the step number. The numbers increment by 5 (e.g., `e05`, `e10`, `e15`, `e20`, etc.). This consistent increment allows for future insertions of new steps without requiring renumbering of existing PIDs.

* **Example:**

    Let's consider an example PID: `c8fms03-c20bre06-e15`

    * `c8fms03`: This is the *start resource* and is a Microservice ID (MID). It indicates that the process step begins with the Frontend Microservice `fms03` belonging to Component C8.

    * `c20bre06`: This is the *end resource* and is a Resource ID (RID). It indicates that the process step ends with the Backend Resource `bre06` belonging to Component C20.

    * `e15`: This indicates that this process step is the *third* step in the execution sequence within the relevant Leg (because 15 / 5 = 3). The first step in the Leg would be `e05`, the second `e10`, and so on.

    Therefore, the complete PID `c8fms03-c20bre06-e15` signifies that this process step involves the Frontend Microservice `c8fms03` interacting with the Backend Resource `c20bre06`, and it is the third step in the execution sequence within the context of its Leg. This PID provides a concise and structured way to identify and track this specific process step.

---

## Naming Conventions for Specific Resources

This section details the naming conventions used for specific types of resources within the k8or Orbit application.  Consistent naming is essential for easy identification, management, and automation of these resources.

* **DynamoDB Tables:** DynamoDB table names follow the format: `[Application/Context]-[Descriptive Name]-[Suffix]-[Environment]-[Cloud Provider]`.

    * `Application/Context`: A short identifier indicating the application or context to which the table belongs (e.g., `user`, `image`, `k8or`).
    * `Descriptive Name`: A descriptive name for the table, using hyphens to separate words (e.g., `user-data`, `app-image-metadata`).
    * `Suffix`: A suffix indicating the resource type - `-tbl` for table.
    * `Environment`: The k8or environment, such as `-k8d` for development, `-k8t` for testing, `-k8s` for staging, `-k8p` for production.
    * `Cloud Provider`: The cloud provider where the table is created, `-aws` for AWS.

    *   **Examples:**
        * `user-data-1612-tbl-k8d-aws`: A table storing user data.
        * `app-image-metadata-1615-tbl-k8d-aws`: A table storing metadata about application images.
        * `user-event-log-1621-tbl-k8d-aws`: A table storing user event logs.
        * `portal-upload-static-data-1627-tbl-k8d-aws`: A table storing static data for image uploads.

* **S3 Buckets:** S3 bucket names follow the format: `[Application/Context]-[Descriptive Name]-[Suffix]-[Environment]-[Cloud Provider]`.

    * `Application/Context`: A short identifier indicating the application or context to which the bucket belongs (e.g., `user`, `image`, `k8or`).
    * `Descriptive Name`: A descriptive name for the bucket, using hyphens to separate words (e.g., `user-data`, `app-image-metadata`).
    * `Suffix`: A suffix indicating the resource type - `-bkt` for bucket.
    * `Environment`: The k8or environment - `-k8d` for development, `-k8t` for testing, `-k8s` for staging, `-k8p` for production.
    * `Cloud Provider`: The cloud provider where the table is created, `-aws` for AWS.

    *   **Examples:**
        *   `temporary-app-image-1203-bkt-k8d-aws`: A temporary bucket for storing images before validation.
        *   `app-image-1206-bkt-k8d-aws`: A permanent bucket for storing validated application images.
        *   `k8or-backup-bkt-prod-k8d-aws`: A backup bucket for the k8or portal in production.

* **MySQL Databases:** MySQL database names follow the format: `[Application/Context]_[Descriptive Name]_[Suffix]_[Environment]_[Cloud Provider]`.

    * `Application/Context`: A short identifier indicating the application or context to which the database belongs (e.g., `app`, `k8or`).
    * `Descriptive Name`: A descriptive name for the database, using underscores to separate words (e.g., `image_mql`).
    * `Suffix`: A suffix indicating the resource type - `-dtb` for database.
    * `Environment`: The k8or environment, such as `-k8d` for development, `-k8t` for testing, `-k8s` for staging, `-k8p` for production.
    * `Cloud Provider`: The cloud provider where the table is created, `-aws` for AWS.

    *   **Examples:**
        *   `app_image_mql_2003_dtb_k8d_aws`: A MySQL database for application image data in development.
        *   `app_image_mql_2003_dtb_k8p_aws`: A MySQL database for application image data in production.
---

## Specific Component and Resource Mapping

This section provides a detailed mapping of components, microservices, and resources within the k8or Orbit application, focusing on the `k8or Portal node` and its interactions. This information is critical for understanding the k8or Orbit application's architecture and the relationships between its various elements.

### `k8or Portal node` Microservices

The following microservices are currently hosted on the `k8or Portal node`:

*   **`c8fms03` - Portal Upload Board Frontend Microservice:** Handles user interface interactions for image uploads, presenting the upload form and managing user input.
*   **`c8bms06` - Portal Upload Logic Backend Microservice:** Implements the backend logic for image uploads, receiving user data, interacting with other microservices and resources, and managing the upload workflow.
*   **`c8fms09` - Portal Search Board Frontend Microservice:** Manages the user interface for searching uploaded images and related metadata.
*   **`c8bms18` - Portal Search Logic Backend Microservice:** Implements the backend search logic, querying databases and resources to retrieve search results.  Also handles the `Portal Transfer Logic`.
*   **`c8bms15` - Portal Deploy Logic Backend Microservice:** Manages the deployment of applications to Kubernetes clusters, using generated manifests and user configurations.
*   **`c8bms18` - Portal Transfer Logic Backend Microservice:** Handles the transfer of images between different storage locations or registries.  (Note: This microservice handles two distinct functions.)
*   **`c8bms21` - Portal Scan Logic Backend Microservice:** Implements image scanning functionalities, potentially for vulnerabilities or compliance checks.
*   **`c8bms24` - Portal Chart Logic Backend Microservice:** Manages the creation and manipulation of Helm charts, facilitating application deployments.
*   **`c8bms27` - Portal Delete Logic Backend Microservice:** Handles the deletion of images, manifests, or other resources within the portal.

### Backend Resource List

The following persistent resources are used by the k8or Orbit application:

*   **`c20bre03` - `base-kustomize-1603-tbl-k8d-aws` DynamoDB Table:** Stores base kustomize manifest templates.
*   **`c20bre06` - `user-input-manifest-data-1606-tbl-k8d-aws` DynamoDB Table:** Stores user-provided configuration data for manifest generation.
*   **`c20bre09` - `k8or-kustomize-1609-tbl-k8d-aws` DynamoDB Table:** Stores generated kustomize manifest files.
*   **`c20bre12` - `user-metadata-1612-tbl-k8d-aws` DynamoDB Table:** Stores user-related metadata.
*   **`c20bre15` - `app-image-metadata-1615-tbl-k8d-aws` DynamoDB Table:** Stores metadata about uploaded application images.
*   **`c20bre21` - `user-event-log-1621-tbl-k8d-aws` DynamoDB Table:** Stores user activity logs.
*   **`c20bre24` - `user-error-log-1624-tbl-k8d-aws` DynamoDB Table:** Stores error logs related to user actions.
*   **`c20bre27` - `portal-upload-static-data-1627-tbl-k8d-aws` DynamoDB Table:** Stores static data used in the image upload process.
*   **`c16bre03` - `temporary-app-image-1203-bkt-k8d-aws` S3 Bucket:** Serves as a temporary storage location for uploaded images before validation.
*   **`c16bre06` - `app-image-1206-bkt-k8d-aws` S3 Bucket:** Stores validated application images.
*   **`c32bre03` - `app_image_mql_2003_dtb_k8d_aws` MySQL Database:** Stores application and user data, including WordPress data if integrated.  Contains multiple tables as detailed in the original document.

---

## Transfer Image Functionality

This section details the image transfer functionality within the k8or Orbit application, outlining the steps involved, event logging, and error handling. The transfer functionality enables users to move application images between different environments, such as from development to production.

### Transfer Functionality Overview

The primary goal of the image transfer functionality is to efficiently and reliably copy application images from a source location (e.g., an S3 bucket) to a destination location (e.g., another S3 bucket). This functionality involves retrieving the image, and then storing it in the target location, along with updating relevant metadata.

### Transfer Process Event Logging

The following events are logged during the transfer process:

| Event Name | Description | Microservice (MID) | Resource (RID) | Example Log Entry |
|---|---|---|---|---|
| `transfer-initiated` | User initiates the transfer. | `c8bms18` | `c20bre21` | `{"event":"transfer-initiated", "timestamp":"2024-10-27T10:00:00Z", "user":"user123", "image":"my-image"}` |
| `image-retrieved` | Image successfully retrieved. | `c8bms18` | `c20bre21` | `{"event":"image-retrieved", "timestamp":"2024-10-27T10:00:15Z"}` |
| `image-uploaded` | Image successfully uploaded. | `c8bms18` | `c20bre21` | `{"event":"image-uploaded", "timestamp":"2024-10-27T10:00:25Z"}` |
| `helm-chart-retrieved` | Manifest successfully retrieved. | `c8bms18` | `c20bre21` | `{"event":"helm-chart-retrieved", "timestamp":"2024-10-27T10:00:35Z"}` |
| `helm-chart-saved` | Manifest successfully saved. | `c8bms18` | `c20bre21` | `{"event":"helm-chart-saved", "timestamp":"2024-10-27T10:00:45Z"}` |
| `viewport-updated` | Database successfully updated. | `c8bms18` | `c20bre21` | `{"event":"viewport-updated", "timestamp":"2024-10-27T10:00:55Z"}` |
| `transfer-completed` | Transfer process completed. | `c8bms18` | `c20bre21` | `{"event":"transfer-completed", "timestamp":"2024-10-27T10:01:00Z"}`|