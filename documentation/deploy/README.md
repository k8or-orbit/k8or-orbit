# k8or Portal Deploy Image Process

This document details the process flow for transferring an image from the development environment to the testing environment within the k8or portal.  The process involves interactions between the frontend user, frontend microservice, backend microservice, MySQL database, S3 buckets (development and testing), and DynamoDB tables.

## Content

1. **[Deployment Request and Preparation](#Deployment-Request-and-Preparation)**
    - Endpoint **`/search/`**
    - Endpoint **`/deploy/DeployImage.php`**
    - Endpoint **`/deploy/InitiateDeployImageUserEventLogChartStore.php`**
    - Endpoint **`/deploy/GetDeployHelmChartStore.php`**
    - Endpoint **`/deploy/InitiateGenerateHelmChartUserEventLogChartStore.php`**
    - Endpoint **`/deploy/GenerateHelmChartStore.php`**
    - Endpoint **`/deploy/GeneratedHelmChartUserEventLogChartStore.php`**
2. **[Image and Helm Chart Synchronization](#Image-and-Helm-Chart-Synchronization)**
    - Endpoint **`/syncmaster/SyncImage.php`**
    - Endpoint **`/syncmaster/InitiateSyncImageUserEventLogChartStore.php`**
    - Endpoint **`/syncmaster/RetrieveSyncImageHarbor.php`**
    - Endpoint **`/syncmaster/RetrievedSyncImageUserEventLogChartStore.php`**
    - Endpoint **`/syncmaster/SyncImageDockerHub.php`**
    - Endpoint **`/syncmaster/SyncImageDockerHubUserEventLogChartStore.php`**
    - Endpoint **`/syncmaster/RetrieveSyncHelmChartStore.php`**
    - Endpoint **`/syncmaster/RetrievedSyncHelmChartUserEventLogChartStore.php`**
    - Endpoint **`/syncmaster/SyncHelmChartGitLab.php`**
    - Endpoint **`/syncmaster/SyncHelmChartUserEventLogChartStore.php`**
    - Endpoint **`/syncmaster/UpdateDeployListGitLab.php`**
    - Endpoint **`/syncmaster/UpdatedDeployListGitLabUserEventLogChartStore.php`**
3. **[GitLab Integration and Argo CD Synchronization](#GitLab-Integration-and-Argo-CD-Synchronization)**
    - Endpoint **`eks-argocd-controller`**
    - Endpoint **`/accesspoint/ProvideK3sAccessArgoCD.php`**
    - Endpoint **`/accesspoint/RequestedK3sAccessArgoCDUserEventLogChartStore.php`**
    - Endpoint **`/accesspoint/ProvideK3sAccessEKSArgoCDtoK3sArgoCD.php`**
    - Endpoint **`k3s-api-server/apis/apps/v1/deployments`**
    - Endpoint **`/accesspoint/RetrievedK3sStateUserEventLogChartStore.php`**
4. **[Akuity Deployment Orchestration](#Akuity-Deployment-Orchestration)**
    - Endpoint **`eks-argocd-controller`**
    - Endpoint **`/accesspoint/ProvideK3sAccessAkuity.php`**
    - Endpoint **`/accesspoint/RequestedK3sAccessAkuityUserEventLogChartStore.php`**
    - Endpoint **`/accesspoint/ProvideK3sAccessEKSAkuitytoK3sAkuity.php`**
    - Endpoint **`k3s-api-aerver`**
    - Endpoint **`k3s-namespace`**
5. **[Argo CD Notification and Deployment Finalization](#Argo-CD-Notification-and-Deployment-Finalization)**
    - Endpoint **`/deploy/GetDeploymentNotificationArgoCD.php`**
    - Endpoint **`/deploy/ReceivedDeploymentNotificationArgoCDUserEventLogChartStore.php`**
    - Endpoint **`/deploy/UpdateDeployImageViewPort.php`**
    - Endpoint **`/deploy/UpdatedDeployImageViewPortUserEventLogChartStore.php`**
    - Endpoint **`/deploy/DeployedHelmChartUserEventLogChartStore.php`**
    - Endpoint **`/deploy/DeployedImageUserEventLogChartStore.php`**

**Component C8**, the **k8or Portal**, is deployed on an EC2 instance designated as the `k8or Portal node`. This node functions similarly to a node within an EKS or K3s cluster, hosting multiple microservices (MSVs). Each functional process (`upload`, `search`, `transfer`, `deploy`, `scan`, `chart`, `delete`, and future additions) is handled by dedicated microservices. Processes requiring a user interface have both frontend (FMS) and backend (BMS) microservices, while backend-only processes utilize only Backend Microservices.

---

## Configuration Resource list for the `Portal Image Upload` Process:
* **`GitLab` EC2 instance**
* **`AccessPoint` EC2 instance**
* **`ArgoCD` EKS Cluster Container**
* **`Akuity` EKS Cluster Container**
* **`ArgoCD` EKS Cluster Container Notification (to send a success/failure deployment response to the `Portal Deploy Logic` Backend Microservice `c8bms12`)** 
* **`ArgoCD` K3s Cluster Container**
* **`Akuity` K3s Cluster Container**

## Development Resource list for the `Portal Image Upload` Process:
* **`Portal Deploy Logic` Backend Microservice with MID (Microservice Identitication) `c8bms12`**
* **`SyncMaster` Backend Microservice with MID `c56bms03`**
* **`AccessPoint` Backend Microservice with MID `c52bms03`**

## Creation Resource List for the `Portal Image Upload` Process:
* **`k8or-orbit-aws` DockerHub Organization**
* **DynamoDB Tables:** 
    1. **`deployment-event-log-1618-tbl-k8d-aws` DynamoDB Table with RID (Resource Identification) `c20bre18`**

---

## The `k8or Portal node` currently hosts the following microservices:

1.  **`Portal Upload Board` Frontend Microservice** (`c8fms03`)
2.  **`Portal Upload Logic` Backend Microservice** (`c8bms06`)
3.  **`Portal Search Board` Frontend Microservice** (`c8fms09`)
4.  **`Portal Search Logic` Backend Microservice** (`c8bms12`)
5.  **`Portal Deploy Logic` Backend Microservice** (`c8bms15`)
6.  **`Portal Transfer Logic` Backend Microservice** (`c8bms18`)
7.  **`Portal Scan Logic` Backend Microservice** (`c8bms21`)
8.  **`Portal Chart Logic` Backend Microservice** (`c8bms24`)
9.  **`Portal Delete Logic` Backend Microservice** (`c8bms27`)

## The `k8or Portal node` microservices talks to the following external resources (outside the node):

* **Backend Resources:**

    * **DynamoDB Tables:** 

        1. **`k8or-kustomize-1609-tbl-k8d-aws` (`c20bre09`) DynamoDB Table**
            * **Purpose:** The purpose of the **`k8or-kustomize-1609-tbl-k8d-aws` (`c20bre09`) DynamoDB table** is to store the generated, customized kustomize manifest files. These are the final deployment configurations, ready to be applied to a Kubernetes cluster.

        2. **`user-event-log-1621-tbl-k8d-aws` (`c20bre21`) DynamoDB Table**
            * **Purpose:** The purpose of the **`user-event-log-1621-tbl-k8d-aws` (`c20bre21`) DynamoDB table** is to store logs of user activity within the k8or portal. This table records user actions, such as logins, image uploads, manifest generation, deployments, and other significant events, providing an audit trail of user interactions.

        3. **`user-error-log-1624-tbl-k8d-aws` (`c20bre24`) DynamoDB Table**
            * **Purpose:** The purpose of the **`user-error-log-1624-tbl-k8d-aws` (`c20bre24`) DynamoDB table** is to store error logs related to user actions. This table captures any errors encountered during user operations, providing valuable information for debugging and troubleshooting.

        4. **`deployment-event-log-1618-tbl-k8d-aws` (`c20bre18`) DynamoDB Table**
            * **Purpose:** The purpose of the **`deployment-event-log-1618-tbl-k8d-aws` (`c20bre24`) DynamoDB table** is to store detailed logs of deployment events. This table records information about each deployment, including timestamps, the deployed image (name and tag), the target environment, the deployment status (success or failure), the associated request ID (linking it to the initial user request), and any other relevant deployment-specific data. This provides a comprehensive history of deployments and facilitates tracking and analysis of deployment activity.

    * **S3 Buckets:** 

        1. **`app-image-1206-bkt-k8d-aws` (`c16bre06`) S3 Bucket**
            * **Purpose:** The purpose of the **`app-image-1206-bkt-k8d-aws` (`c16bre06`) S3 bucket** is to store validated application images.  Once an image has been successfully validated, it is moved to this bucket for permanent storage and later use in deployments.

    * **MySQL database:** 
        1. **`app_image_mql_2003_dtb_k8d_aws` (`c32bre03`) MySQL Database**
            * **Purpose:** The purpose of the **`app_image_mql_2003_dtb_k8d_aws` (`c32bre03`) MySQL database** is to store application and user data for the k8or portal, including data related to WordPress functionality if the portal integrates with it.

        * **MySQL Tables:**
            1. **`wp_options` (`c32bre06`) MySQL Table**
                * **Purpose:** The purpose of the **`wp_options` MySQL table** is to store website options and settings, used by WordPress.

            2. **`wp_postmeta` (`c32bre09`) MySQL Table**
                * **Purpose:** The purpose of the **`wp_postmeta` MySQL table** is to store metadata associated with posts (which can represent various content types in WordPress).

            3. **`wp_posts` (`c32bre12`) MySQL Table**
                * **Purpose:** The purpose of the **`wp_posts` MySQL table** is to store posts, pages, and other content items (WordPress).

            4. **`wp_term_relationships` (`c32bre15`) MySQL Table**
                * **Purpose:** The purpose of the **`wp_term_relationships` MySQL table** is to define the relationships between terms (categories, tags) and posts (WordPress).

            5. **`wp_term_taxonomy` (`c32bre18`) MySQL Table**
                * **Purpose:** The purpose of the **`wp_term_taxonomy` MySQL table** is to define the taxonomy (categories, tags) for terms (WordPress).

            6. **`wp_terms` (`c32bre21`) MySQL Table**
                * **Purpose:** The purpose of the **`wp_terms` MySQL table** is to store the actual terms (categories, tags) (WordPress).

            7. **`wp_usermeta` (`c32bre24`) MySQL Table**
                * **Purpose:** The purpose of the **`wp_usermeta` MySQL table** is to store metadata associated with users (WordPress).

            8. **`wp_users` (`c32bre27`) MySQL Table**
                * **Purpose:** The purpose of the **`wp_users` MySQL table** is to store user information (WordPress).

            9. **`wp_wpdatatables` (`c32bre30`) MySQL Table**
                * **Purpose:** The purpose of the **`wp_wpdatatables` MySQL table** is to store data for the wpDataTables plugin, likely used for displaying data in tables within the portal.

            10. **`wp_wpdatatables_columns` (`c32bre33`) MySQL Table**
                * **Purpose:** The purpose of the **`wp_wpdatatables_columns` MySQL table** is to store column definitions for the wpDataTables plugin, defining how data is displayed in the tables. This includes information such as column names, data types, formatting options, and whether a column is sortable or searchable.  It essentially provides the structure and presentation rules for the data tables displayed via the wpDataTables plugin.

The **microservice identification string** (e.g., `c8fms03`) facilitates tracking communication subprocesses between Frontend/Backend Microservices and backend resources.

**Component C2 designates frontend user accounts associated with specific actions performed within the k8or Orbit application. For example:**

1. The **`c2usr03` Frontend User** is dedicated to the **portal image upload process**.
2. The **`c2usr06` Frontend User** is dedicated to the **portal image search process**.
3. The **`c2usr09` Frontend User** is dedicated to the **portal image deployment process**.
4. The **`c2usr12` Frontend User** is dedicated to the **portal image transfer process**.
5. The **`c2usr15` Frontend User** is dedicated to the **portal image scanning process**.
6. The **`c2usr18` Frontend User** is dedicated to the **portal image charting process**.
7. The **`c2usr21` Frontend User** is dedicated to the **portal image deletion process**.

These **Frontend User** IDs (e.g, `c2usr03`) are used exclusively for their designated processes, and both the names and IDs are immutable variables. This convention is necessary because each process within the k8or Orbit application is initiated by a frontend user action. Spontaneous process initiation is not permitted. The frontend user IDs provide a mapping mechanism to identify the precise process triggered by a specific user action.

# Process Flows

### Diagram: [View](https://github.com/k8or-orbit-aws/k8or-orbit-doc-0103-rep-k8d-aws/tree/k8or-dev/deploy-dir/diagram-dir/k8or-portal-deploy-v0-0-06-dia-k8d.drawio.pdf)

![Alt text](https://github.com/k8or-orbit-aws/k8or-orbit-doc-0103-rep-k8d-aws/blob/k8or-dev/deploy-dir/diagram-dir/k8or-portal-deploy-v0-0-06-dia-k8d.jpg)

<h1 id="Deployment-Request-and-Preparation">Deployment Request and Preparation</h1>

### 1.1. `c2usr12-c8fms06-e05` User Initiates Deployment: The Frontend User clicks the `deploy` button, provides the required information, confirms the operation by clicking `Deploy`, and acknowledges the confirmation prompt.

* **Endpoint:** `https://orbit.k8or.com/search/`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** `none`

### 1.2. `c8fms06-c8bms15-e10` Frontend Microservice Relays Information: The **`Portal Search Board` Frontend Microservice** aggregates the user's deployment information and transmits it to the **`Portal Deploy Logic` Backend Microservice**.

* **Endpoint:** `https://orbit.k8or.com/deploy/DeployImage.php`
```
deploy/
├── src/
│   ├── routes/
│   │   └── DeployImage.php
│   ├── functions/
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c8fms06-c8bms15-e10",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development"
  },
  "deploymentDetail": {
    "containerPort": "80",
    "replicaNumber": "1",
    "cpuLimit": "100m",
    "memoryLimit": "256Mi",
    "clusterName": "K3s Development Cluster"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c8fms06-c8bms15-e10",
  "status": "success",
  "message": "The image has been deployed successfully."
}
```

### 1.3. `c8bms15-c20bre21-e15` Deployment Initiation Logged: The `Portal Deploy Logic` Backend Microservice logs a `deployment-initiated` event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **Endpoint:** `https://orbit.k8or.com/deploy/InitiateDeployImageUserEventLogChartStore.php`
```
deploy/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── InitiateDeployImageUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Event Type:** `deployment-initiated`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c8bms15-c20bre21-e15",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development",
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c8bms15-c20bre21-e15",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 1.4. `c8bms15-c20bre09-e20` Existing Helm Chart Retrieved: The `Portal Deploy Logic` Backend Microservice retrieves the existing image Helm chart from the `k8or-kustomize-1609-tbl-k8d-aws` DynamoDB table. The `Portal Deploy Logic` Backend Microservice compares the retrieved Helm chart with the user's submitted deployment data.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **Endpoint:** `https://orbit.k8or.com/deploy/GetDeployHelmChartStore.php`
```
deploy/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── GetDeployImageHelmChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c8bms15-c20bre09-e20",
  "imageMetadata": { 
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageVersion": "v0-0-01"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c8bms15-c20bre09-e20",
  "status": "success",
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageVersion": "v0-0-01",
    "helmChartName": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5-v0-0-01", // 'helmChartName' is constructed based on 'imageUID' and 'imageVersion'
    "helmChart": [
    {
      "apiVersion": "apps/v1",
      "kind": "Deployment",
      "metadata": {
        "name": "release-deployment",
        "labels": {
          "app": "app",
          "release": "release"
        }
      },
      "spec": {
        "replicas": 3,
        "selector": {
          "matchLabels": {
            "app": "app"
          }
        },
        "template": {
          "metadata": {
            "labels": {
              "app": "app"
            }
          },
          "spec": {
            "containers": [
              {
                "name": "container",
                "image": "nginx:latest",
                "ports": [
                  {
                    "containerPort": 80
                  }
                ],
                "envFrom": [
                  {
                    "configMapRef": {
                      "name": "release-configmap"
                    }
                  }
                ]
              }
            ]
          }
        }
      }
    },
    {
      "apiVersion": "v1",
      "kind": "Service",
      "metadata": {
        "name": "release-service",
        "labels": {
          "app": "app",
          "release": "release"
        }
      },
      "spec": {
        "type": "LoadBalancer",
        "ports": [
          {
            "port": 80,
            "targetPort": 80
          }
        ],
        "selector": {
          "app": "app"
        }
      }
    },
    {
      "apiVersion": "v1",
      "kind": "ConfigMap",
      "metadata": {
        "name": "release-configmap",
        "labels": {
          "app": "app",
          "release": "release"
        }
      },
      "data": {
        "MY_CONFIG_VALUE": "Hello from ConfigMap!"
      }
    }
  ]
}
```

### 1.5. `c8bms15-c20bre21-e25` Helm Chart Update Initiated Logged: If an update to the existing Helm chart is required, the `Portal Deploy Logic` Backend Microservice logs an `image-helm-chart-update-initiated` event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **Endpoint:** `https://orbit.k8or.com/deploy/InitiateGenerateHelmChartUserEventLogChartStore.php`
```
deploy/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── InitiateUpdateImageHelmChartUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Event Type:** `helm-chart-generate-initiated`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c8bms15-c20bre21-e15",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development",
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c8bms15-c20bre21-e15",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 1.6. `c8bms15-c20bre09-e30` New Helm Chart Generated and Saved: The `Portal Deploy Logic` Backend Microservice generates a new Helm chart, assigns it a new version number (`v0-0-02`), and persists it to the `k8or-kustomize-1609-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **Endpoint:** `https://orbit.k8or.com/deploy/GenerateHelmChartStore.php`
```
deploy/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── GenerateImageHelmChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c8bms15-c20bre09-e30",
  "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5",
  "helmChart": [
    {
      "apiVersion": "apps/v1",
      "kind": "Deployment",
      "metadata": {
        "labels": {
          "app": "app",
          "release": "release"
        },
        "name": "release-deployment"
      },
      "spec": {
        "replicas": 3,
        "selector": {
          "matchLabels": {
            "app": "app"
          }
        },
        "template": {
          "metadata": {
            "labels": {
              "app": "app"
            }
          },
          "spec": {
            "containers": [
              {
                "envFrom": [
                  {
                    "configMapRef": {
                      "name": "release-configmap"
                    }
                  }
                ],
                "image": "nginx:latest",
                "name": "container",
                "ports": [
                  {
                    "containerPort": 80
                  }
                ]
              }
            ]
          }
        }
      }
    },
    {
      "apiVersion": "v1",
      "kind": "Service",
      "metadata": {
        "labels": {
          "app": "app",
          "release": "release"
        },
        "name": "release-service"
      },
      "spec": {
        "ports": [
          {
            "port": 80,
            "targetPort": 80
          }
        ],
        "selector": {
          "app": "app"
        },
        "type": "LoadBalancer"
      }
    },
    {
      "apiVersion": "v1",
      "kind": "ConfigMap",
      "metadata": {
        "labels": {
          "app": "app",
          "release": "release"
        },
        "name": "release-configmap"
      },
      "data": {
        "MY_CONFIG_VALUE": "Hello from ConfigMap!"
      }
    }
  ]
}
```
* **Response payload:** 
```json
{
  "PID": "c8bms15-c20bre09-e30",
  "status": "success",
  "message": "The Helm Chart saved successfully.",
}
```

### 1.7. `c8bms15-c20bre21-e35` Helm Chart Generation Logged: The `Portal Deploy Logic` Backend Microservice logs a `helm-chart-generated` event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **Endpoint:** `https://orbit.k8or.com/deploy/GeneratedHelmChartUserEventLogChartStore.php`
```
deploy/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── GeneratedImageHelmChartUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Event Type:** `kustomization-saved`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c8bms15-c20bre21-e35",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5",
    "icao": "British",
    "tail": "G-AAAA",
    "Part": "21117-1",
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "port": "80",
    "envVars": [
      {"key": "VAR1", "value": "value1"},
      {"key": "VAR2", "value": "value2"}
    ],
    "volumes": [
      {"key": "/host/path", "value": "/container/path"}
    ],
    "imageLabels": [
      {"key": "label1", "value": "value1"}
    ],
    "commandOverride": "/app/run.sh",
    "entrypointOverride": "/app/entrypoint.sh",
    "workingDir": "/app",
    "image": "data:image/png;base64,iVBORw0KGgo"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c8bms15-c20bre21-e35",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

<h1 id="Image-and-Helm-Chart-Synchronization">Image and Helm Chart Synchronization</h1>

### 2.1. `c8bms15-c56bms03-e40` Sync Request Sent: The `Portal Deploy Logic` Backend Microservice transmits a synchronization request to the `SyncMaster` Backend Microservice.

* **Endpoint:** `https://orbit.k8or.com/syncmaster/SyncImage.php`
```
syncmaster/
├── src/
│   ├── routes/
│   │   └── SyncImage.php
│   ├── functions/
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c56bms03-c20bre21-e45",
  "imageMetadata": { 
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageVersion": "v0-0-01", // 'imageFileName' will be constructed based on 'imageUID' and 'imageVersion': Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5-v0-0-01.tar
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c56bms03-c20bre21-e45",
  "status": "success",
  "imageData": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxglj2+t/AA..."
}
```

### 2.2. `c56bms03-c20bre21-e45` Sync Initiation Logged: The `SyncMaster` Backend Microservice logs a `sync-initiated` event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

* **Endpoint:** `https://orbit.k8or.com/syncmaster/InitiateSyncImageUserEventLogChartStore.php`
```
syncmaster/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── InitiateSyncImageUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Event Type:** `sync-initiated`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c56bms03-c20bre21-e45",
  "imageMetadata": { 
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageVersion": "v0-0-01", // 'imageFileName' will be constructed based on 'imageUID' and 'imageVersion': Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5-v0-0-01.tar
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c56bms03-c20bre21-e45",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 2.3. `c56bms03-c16bre06-e50` S3 Image Retrieved: The `SyncMaster` Backend Microservice retrieves the image from the `app-image-1206-bkt-k8d-aws` S3 bucket.

* **Endpoint:** `https://orbit.k8or.com/syncmaster/RetrieveSyncImageHarbor.php`
```
syncmaster/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── RetrieveSyncImageHarbor.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c56bms03-c16bre06-e50",
  "imageMetadata": { 
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageVersion": "v0-0-01", // 'imageFileName' will be constructed based on 'imageUID' and 'imageVersion': Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5-v0-0-01.tar
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c56bms03-c16bre06-e50",
  "status": "success",
  "imageData": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxglj2+t/AA..."
}
```

### 2.4. `c56bms03-c20bre21-e55` Image Retrieval Logged: The `SyncMaster` Backend Microservice logs an `image-retrieved` event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **Endpoint:** `https://orbit.k8or.com/syncmaster/RetrievedSyncImageUserEventLogChartStore.php`
```
syncmaster/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── RetrievedSyncImageUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Event Type:** `image-retrieved`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c56bms03-c20bre21-e55",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c56bms03-c20bre21-e55",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 2.5. `c56bms03-c96bre03-e60` Image Pushed to Docker Hub: The `SyncMaster` Backend Microservice pushes the retrieved image to the designated Docker Hub organization/repository.

* **Endpoint:** `https://orbit.k8or.com/syncmaster/SyncImageDockerHub.php`
```
syncmaster/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── SyncImageDockerHub.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c56bms03-c96bre03-e60",
  "imageMetadata": { 
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageVersion": "v0-0-01", // 'imageFileName' will be constructed based on 'imageUID' and 'imageVersion': Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5-v0-0-01.tar
    "imageData": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxglj2+t/AA..."
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c56bms03-c96bre03-e60",
  "status": "success",
  "message": "The image has been transferred successfully."
}
```

### 2.6. `c56bms03-c20bre21-e65` Image Push Logged: The `SyncMaster` Backend Microservice logs an `image-pushed` event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **Endpoint:** `https://orbit.k8or.com/syncmaster/SyncImageDockerHubUserEventLogChartStore.php`
```
syncmaster/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── SyncImageDockerHubUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Event Type:** `image-pushed`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c56bms03-c20bre21-e65",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c56bms03-c20bre21-e65",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 2.7. `c56bms03-c20bre09-e70` Image Helm Chart Retrieved: The `SyncMaster` Backend Microservice retrieves the image Helm chart from the `k8or-kustomize-1609-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **Endpoint:** `https://orbit.k8or.com/syncmaster/RetrieveSyncHelmChartStore.php`
```
syncmaster/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── RetrieveSyncHelmChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c56bms03-c20bre09-e70",
  "imageMetadata": { 
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageVersion": "v0-0-01"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c56bms03-c20bre09-e70",
  "status": "success",
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageVersion": "v0-0-01",
    "helmChartName": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5-v0-0-01", // 'helmChartName' is constructed based on 'imageUID' and 'imageVersion'
    "helmChart": [
    {
      "apiVersion": "apps/v1",
      "kind": "Deployment",
      "metadata": {
        "name": "release-deployment",
        "labels": {
          "app": "app",
          "release": "release"
        }
      },
      "spec": {
        "replicas": 3,
        "selector": {
          "matchLabels": {
            "app": "app"
          }
        },
        "template": {
          "metadata": {
            "labels": {
              "app": "app"
            }
          },
          "spec": {
            "containers": [
              {
                "name": "container",
                "image": "nginx:latest",
                "ports": [
                  {
                    "containerPort": 80
                  }
                ],
                "envFrom": [
                  {
                    "configMapRef": {
                      "name": "release-configmap"
                    }
                  }
                ]
              }
            ]
          }
        }
      }
    },
    {
      "apiVersion": "v1",
      "kind": "Service",
      "metadata": {
        "name": "release-service",
        "labels": {
          "app": "app",
          "release": "release"
        }
      },
      "spec": {
        "type": "LoadBalancer",
        "ports": [
          {
            "port": 80,
            "targetPort": 80
          }
        ],
        "selector": {
          "app": "app"
        }
      }
    },
    {
      "apiVersion": "v1",
      "kind": "ConfigMap",
      "metadata": {
        "name": "release-configmap",
        "labels": {
          "app": "app",
          "release": "release"
        }
      },
      "data": {
        "MY_CONFIG_VALUE": "Hello from ConfigMap!"
      }
    }
  ]
}
```

### 2.8. `c56bms03-c20bre21-e75` Image Helm Chart Retrieval Logged: The `SyncMaster` Backend Microservice logs an `image-helm-chart-retrieved` event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **Endpoint:** `https://orbit.k8or.com/syncmaster/RetrievedSyncHelmChartUserEventLogChartStore.php`
```
syncmaster/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── RetrievedSyncHelmChartUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Event Type:** `helm-chart-retrieved`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c56bms03-c20bre21-e75",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c56bms03-c20bre21-e75",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 2.9. `c56bms03-c100bms03-e80` Image Helm Chart Pushed to GitLab: The `SyncMaster` Backend Microservice pushes the image Helm chart to the specified GitLab project/repository.

* **Endpoint:** `https://orbit.k8or.com/syncmaster/SyncHelmChartGitLab.php`
```
syncmaster/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── SyncHelmChartGitLab.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c56bms03-c100bms03-e80",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5",
    "imageVersion": "v0-0-01"
  },
  "chartContent": "base64_encoded_chart_data"
}
```
* **Response payload:** 
```json
{
  "PID": "c56bms03-c100bms03-e80",
  "file_path": "helm-chart/trolls-world-tour-2020/v0-0-01/trolls-world-tour-2020-v0-0-01.tgz",
  "branch": "syncmaster",
  "commit_id": "e7195a92a51f9a8b072c7c897f1396a925438c64", // Commit SHA
  "content": "base64 encoded content...",
  "encoding": "base64",
  "content_sha256": "some_sha256_hash" // SHA256 hash of the content
}
```

### 2.10. `c56bms03-c20bre21-e85` Image Helm Chart Push Logged: The `SyncMaster` Backend Microservice logs an `image-helm-chart-pushed` event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **Endpoint:** `https://orbit.k8or.com/syncmaster/SyncHelmChartUserEventLogChartStore.php`
```
syncmaster/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── SyncHelmChartUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Event Type:** `helm-chart-uploaded`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c56bms03-c20bre21-e85",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c56bms03-c20bre21-e85",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 2.11. `c56bms03-c100bms03-e90` `deploy-list.yaml` Updated: The `SyncMaster` Backend Microservice updates the `deploy-list.yaml` file within the GitLab project/repository with the new Kubernetes image-related resources.

* **Endpoint:** `https://orbit.k8or.com/syncmaster/UpdateDeployListGitLab.php`
```
syncmaster/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── UpdateDeployListGitLab.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c56bms03-c100bms03-e90",
  "gitlabProjectId": 12345,
  "filePath": "deploy-list.yaml",
  "branch": "syncmaster",
  "action": "add",
  "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5",
  "imageVersion": "v0-0-01",
  "imageName": "Trolls World Tour (2020)",
  "environment": "development"
}
```
* **Response payload:** 
```json
{
  "PID": "c56bms03-c100bms03-e90",
  "status": "success", 
  "message": "deploy-list.yaml updated successfully. Commit ID: e7195a92a51f9a8b...", 
  "commitId": "e7195a92a51f9a8b..."
}
```

### 2.12. `c56bms03-c20bre21-e95` `deploy-list.yaml` Update Logged: The `SyncMaster` Backend Microservice logs a `deploy-list-updated` event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

* **Endpoint:** `https://orbit.k8or.com/syncmaster/UpdatedDeployListGitLabUserEventLogChartStore.php`
```
syncmaster/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── UpdatedDeployListGitLabUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Event Type:** `deploy-list-updated`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c56bms03-c20bre21-e95",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response payload:**
```json
{
  "PID": "c56bms03-c20bre21-e95",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

<h1 id="GitLab-Integration-and-Argo-CD-Synchronization">GitLab Integration and Argo CD Synchronization</h1>

### 3.1. `c100bms03-c108bms03-e100` Argo CD Fetches Changes: The `ArgoCD Controller` Backend Microservice deployed on the AWS EKS cluster retrieves the changes from the GitLab project/repository.

* **Endpoint:** `eks-argocd-controller`
* **Protocol type:** `https`
* **Request type:** `GET`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:**
```json
{
  "PID": "c100bms03-c108bms03-e100",
  "status": "triggered",
  "message": "Argo CD synchronization initiated."
}
```

### 3.2. `c108bms03-c52bms03-e105` Argo CD Requests K3s State: The `ArgoCD Controller` Backend Microservice deployed on the AWS EKS cluster requests the current state of the K3s cluster from the `AccessPoint` Backend Microservice.

* **Endpoint:** `https://orbit.k8or.com/accesspoint/ProvideK3sAccessArgoCD.php`
```
accesspoint/
├── src/
│   ├── routes/
│   │   └── ProvideK3sAccessArgoCD.php
│   ├── functions/
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c108bms03-c52bms03-e105",
  "clusterIdentifier": "k3s-cluster-123",
  "requestType": "state"
}
```
* **Response payload:** 
```json
{
  "PID": "c108bms03-c52bms03-e105",
  "clusterIdentifier": "k3s-cluster-123",
  "status": "online",
  "kubernetesVersion": "v1.25.4+k3s1",
  "nodes": [
    {
      "name": "k3s-node-1",
      "status": "Ready",
      "cpuUsage": "20%",
      "memoryUsage": "40%"
    },
    {
      "name": "k3s-node-2",
      "status": "Ready",
      "cpuUsage": "15%",
      "memoryUsage": "35%"
    }
  ],
  "deployments": [
    {
      "name": "nginx-deployment",
      "replicas": 3,
      "status": "Available"
    }
  ]
}
```

### 3.3. `c52bms03-c20bre21-e110` K3s State Request Logged: The `AccessPoint` Backend Microservice logs a `k3s-state-requested` event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **Endpoint:** `https://orbit.k8or.com/accesspoint/RequestedK3sAccessArgoCDUserEventLogChartStore.php`
```
accesspoint/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── RequestedK3sAccessArgoCDUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Event Type:** `argocd-requested-k3s-access`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c52bms03-c20bre21-e110",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c52bms03-c20bre21-e110",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 3.4. `c52bms03-c216bms03-e115` Request Information Sent to Argo CD (K3s): The `AccessPoint` Backend Microservice transmits the request information to the `ArgoCD` Backend Microservice deployed on the K3s cluster.

* **Endpoint:** `https://orbit.k8or.com/accesspoint/ProvideK3sAccessEKSArgoCDtoK3sArgoCD.php`
```
accesspoint/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── ProvideK3sAccessEKSArgoCDtoK3sArgoCD.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c52bms03-c216bms03-e115",
  "requestType": "clusterState"
}
```
* **Response payload:** 
```json
{
  "PID": "c52bms03-c216bms03-e115",
  "clusterState": {
    "nodes": [
      {
        "name": "node1",
        "status": "Ready",
        "cpuUsage": "20%",
        "memoryUsage": "50%"
      },
      {
        "name": "node2",
        "status": "Ready",
        "cpuUsage": "15%",
        "memoryUsage": "40%"
      }
    ],
    "deployments": [
      {
        "name": "nginx-deployment",
        "replicas": 3,
        "availableReplicas": 3
      },
      {
        "name": "redis-deployment",
        "replicas": 1,
        "availableReplicas": 1
      }
    ],
    "kubernetesVersion": "v1.26.3"
  }
}
```

### 3.5. `c52bms03-c228bre03-e120` K3s Argo CD Requests Cluster State: The `ArgoCD` Backend Microservice deployed on the K3s cluster queries the K3s cluster's `API Server` for its current state and resources.

* **Endpoint:** `k3s-api-server/apis/apps/v1/deployments`
* **Protocol type:** `https`
* **Request type:** `GET`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** 
```json
{
  "PID": "c52bms03-c228bre03-e120",
  "kind": "DeploymentList",
  "apiVersion": "apps/v1",
  "items": [
    {
      "metadata": {
        "name": "nginx-deployment"
      },
      "status": {
        "availableReplicas": 3
      }
    }
  ]
}
```

### 3.6. `c52bms03-c20bre21-e125` K3s State Retrieved and Logged: The `AccessPoint` Backend Microservice receives the response from the `ArgoCD` Backend Microservice deployed on the K3s cluster and logs a `k3s-state-retrieved` event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **Endpoint:** `https://orbit.k8or.com/accesspoint/RetrievedK3sStateUserEventLogChartStore.php`
```
accesspoint/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── RetrievedK3sStateUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Event Type:** `k3s-state-retrieved`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c52bms03-c20bre21-e125",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c52bms03-c20bre21-e125",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

<h1 id="Akuity-Deployment-Orchestration">Akuity Deployment Orchestration</h1>

### 4.1. `c108bms03-c104bms03-e130` Akuity Fetches Changes: The `Akuity Controller` Backend Microservice deployed on the AWS EKS cluster retrieves the changes from the `ArgoCD Controller` Backend Microservice.

* **Endpoint:** `eks-argocd-controller`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** `none`

### 4.2. `c104bms03-c52bms03-e135` Akuity Initiates Deployment: The `Akuity Controller` Backend Microservice deployed on the AWS EKS cluster sends a request to the `AccessPoint` Backend Microservice to initiate the deployment on the K3s cluster.

* **Endpoint:** `https://orbit.k8or.com/accesspoint/ProvideK3sAccessAkuity.php`
```
accesspoint/
├── src/
│   ├── routes/
│   │   └── ProvideK3sAccessAkuity.php
│   ├── functions/
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** `none`

### 4.3. `c52bms03-c20bre21-e140` Deployment Request Logged: The `AccessPoint` Backend Microservice logs a `k3s-deployment-requested` event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **Endpoint:** `https://orbit.k8or.com/accesspoint/RequestedK3sAccessAkuityUserEventLogChartStore.php`
```
accesspoint/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── RequestedK3sAccessAkuityUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Event Type:** `akuity-requested-k3s-access`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c52bms03-c20bre21-e140",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c52bms03-c20bre21-e140",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 4.4. `c52bms03-c212bms03-e145` Deployment Request Sent to Akuity (K3s): The `AccessPoint` Backend Microservice sends a deployment request to the `Akuity` Backend Microservice deployed on the K3s cluster.

* **Endpoint:** `https://orbit.k8or.com/accesspoint/ProvideK3sAccessEKSAkuitytoK3sAkuity.php`
```
accesspoint/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── ProvideK3sAccessEKSAkuitytoK3sAkuity.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** `none`

### 4.5. `c212bms03-c228bre03-e150` Akuity Applies Helm Chart: The `Akuity` Backend Microservice deployed on the K3s cluster applies the image Helm chart to the K3s cluster's `API Server`.

* **Endpoint:** `k3s-api-aerver`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** `none`

### 4.6. `c228bre03-c340bre03-e155` Resources Created in K3s: The K3s cluster's `API Server` creates the image-related resources in the specified namespace.

* **Endpoint:** `k3s-namespace`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** `none`

### 4.7. `c96bre03-c340bre03-e160` The Deployment Kubernetes resource pulls the image from Docker Hub to create pods.

* **Endpoint:** `dockerhub`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** `none`

<h1 id="Argo-CD-Notification-and-Deployment-Finalization">Argo CD Notification and Deployment Finalization</h1>

### 5.1. `c8bms15-c108bms03-e165` Argo CD Deployment Notification Sent: The `ArgoCD Controller` Backend Microservice deployed on the AWS EKS cluster notifies the `Portal Deploy Logic` Backend Microservice that the deployment is complete.

* **Endpoint:** `https://orbit.k8or.com/deploy/GetDeploymentNotificationArgoCD.php`
```
deploy/
├── src/
│   ├── routes/
│   │   └── GetDeploymentNotificationArgoCD.php
│   ├── functions/
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c8bms15-c108bms03-e165",
  "deploymentId": "deployment-12345",
  "application": "Trolls World Tour (2020)",
  "namespace": "development",
  "status": "success",
  "commitId": "e7195a92a51f9a8b072c7c897f1396a925438c64",
  "timestamp": "2023-10-27T10:00:00Z"
}
```
* **Response payload:** 
```json
{
  "PID": "c8bms15-c108bms03-e165",
  "status": "received",
  "message": "Deployment notification received successfully."
}
```

### 5.2. `c8bms15-c20bre21-e170` Argo CD Notification Logged: The `Portal Deploy Logic` Backend Microservice logs an `argocd-notification` event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **Endpoint:** `https://orbit.k8or.com/deploy/ReceivedDeploymentNotificationArgoCDUserEventLogChartStore.php`
```
deploy/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── ReceivedDeploymentNotificationArgoCDUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Event Type:** `argocd-deployment-notification-received`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c8bms15-c20bre21-e170",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c8bms15-c20bre21-e170",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 5.3. `c8bms15-c32bre03-e175` Deployed Image State Updated: The `Portal Deploy Logic` Backend Microservice updates the deployed image state in the Viewport MySQL database from `Deployed: none` to `Deployed: Jan 17, 2025` (deployment date). The MySQL field stores the full timestamp, but only the date (`Jan 17, 2025`) is displayed on the frontend search UI.

* **Endpoint:** `https://orbit.k8or.com/deploy/UpdateDeployImageViewPort.php`
```
deploy/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── UpdateDeployImageViewPort.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c8bms15-c32bre03-e175",
  "userMetadata": {
    "username": "jamesbond"
  },
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5",
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "icao": "British",
    "tail": "G-AAAA",
    "part": "21117-1", 
    "port": 80,
    "envVars": [
      { "key": "VAR1", "value": "value1" },
      { "key": "VAR2", "value": "value2" }
    ],
    "volumes": [
      { "key": "/host/path", "value": "/container/path" }
    ],
    "imageLabels": [
      { "key": "label1", "value": "value1" }
    ],
    "commandOverride": "/app/run.sh",
    "entrypointOverride": "/app/entrypoint.sh",
    "workingDir": "/app",
    "image": "data:image/png;base64,iVBORw0KGgo" 
  },
  "postMetadata": {
    "postData": {
      "post_id": 32,
    },
    "post_meta": [
      {
        "meta_key": "Deployed",
        "meta_value": "2025-02-27T10:00:00Z" // The deployment timestamp will be converted to "Feb 27, 2025" to be displayed on the Search FE UI
      }
    ]
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c8bms15-c32bre03-e175",
  "success": true,
  "message": "The image updated successfully."
}
```

### 5.4. `c8bms15-c20bre21-e180` Viewport Update Logged: The `Portal Deploy Logic` Backend Microservice logs a `viewport-updated` event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **Endpoint:** `https://orbit.k8or.com/deploy/UpdatedDeployImageViewPortUserEventLogChartStore.php`
```
deploy/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── UpdatedDeployImageViewPortUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Event Type:** `viewport-updated`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c8bms15-c20bre21-e180",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c8bms15-c20bre21-e180",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 5.5. `c8bms15-c20bre18-e185` Image Helm Chart Deployment Log: The `Portal Deploy Logic` Backend Microservice logs an `image-helm-chart-deployed` deployment event to the `deployment-event-log-1618-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **Endpoint:** `https://orbit.k8or.com/deploy/DeployedHelmChartUserEventLogChartStore.php`
```
deploy/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── DeployedHelmChartUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Event Type:** `helm-chart-deployed`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c8bms15-c20bre18-e185",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c8bms15-c20bre18-e185",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 5.6. `c8bms15-c20bre21-e185` Final Log: The `Portal Deploy Logic` Backend Microservice logs a final `image-deployed` event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **Endpoint:** `https://orbit.k8or.com/deploy/DeployedImageUserEventLogChartStore.php`
```
deploy/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── DeployedImageUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Event Type:** `image-deployment-completed`
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** 
```json
{
  "PID": "c8bms15-c20bre21-e190",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c8bms15-c20bre21-e190",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```