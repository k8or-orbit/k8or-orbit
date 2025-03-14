# k8or Portal Transfer Image Process

This document details the process flow for transferring an image from the development environment to the testing environment within the k8or portal.  The process involves interactions between the frontend user, frontend microservice, backend microservice, MySQL database, S3 buckets (development and testing), and DynamoDB tables.

**Component C8**, the **k8or Portal**, is deployed on an EC2 instance designated as the `k8or Portal node`. This node functions similarly to a node within an EKS or K3s cluster, hosting multiple microservices (MSVs). Each functional process (`upload`, `search`, `transfer`, `deploy`, `scan`, `chart`, `delete`, and future additions) is handled by dedicated microservices. Processes requiring a user interface have both frontend (FMS) and backend (BMS) microservices, while backend-only processes utilize only Backend Microservices.

## The `k8or Portal node` currently hosts the following microservices:

1.  **`Portal Upload Board` Frontend Microservice** (`c8fms03`)
2.  **`Portal Upload Logic` Backend Microservice** (`c8bms06`)
3.  **`Portal Search Board` Frontend Microservice** (`c8fms06`)
4.  **`Portal Search Logic` Backend Microservice** (`c8bms18`)
5.  **`Portal Deploy Logic` Backend Microservice** (`c8bms15`)
6.  **`Portal Transfer Logic` Backend Microservice** (`c8bms18`)
7.  **`Portal Scan Logic` Backend Microservice** (`c8bms21`)
8.  **`Portal Chart Logic` Backend Microservice** (`c8bms24`)
9.  **`Portal Delete Logic` Backend Microservice** (`c8bms27`)

The **microservice identification string** (e.g., `c8fms03`) facilitates tracking communication subprocesses between Frontend/Backend Microservices and backend resources.

### Process ID (PID) Structure

Each step in the process flows is assigned a unique Process ID (PID) to facilitate tracking and analysis. The PID follows a structured format:

```
PID: <start-resource>-<end-resource>-e<execution_order>
```

1.  **Resource Identifiers:** The PID identifies the starting and ending resources involved in a specific process step. Resources can be either microservices (MSVs, e.g., `fms09`, `bms12`) backend resources (e.g., `bre03`, `bre06`), or a Frontend User (e.g., `usr03`, `usr06`).
2.  **Component ID Prefix:** Both the start and end resource identifiers are prefixed with a component ID (e.g., `c2`, `c4`, `c8`) to specify the component to which the resource belongs. For example, `c8fms09` indicates that `fms09` is part of component `c8`.
3.  **Execution Order:** The `e<execution_order>` component of the PID indicates the sequence of execution. The `e` prefix denotes `execution`, and the subsequent number represents the step number in increments of 5 (e.g., `e05`, `e10`, `e15`, `e20`). This allows for easy identification of the order in which the processes occur.  For instance, `e05` represents the first step, `e10` the second, and so on.

# Process Flows

### Diagram: [View](https://github.com/k8or-orbit-aws/k8or-orbit-doc-0103-rep-k8d-aws/tree/k8or-dev/transfer-dir/diagram-dir/k8or-portal-transfer-v0-0-06-dia-k8d.drawio.pdf)

![Alt text](https://github.com/k8or-orbit-aws/k8or-orbit-doc-0103-rep-k8d-aws/blob/k8or-dev/transfer-dir/diagram-dir/k8or-portal-transfer-v0-0-06-dia-k8d.jpg)

### 1. `c2usr09-c8fms06-e05`: The process begins when the Frontend User `c2usr09` clicks the **"Transfer"** button on the **Portal Search Board** Frontend Microservice. The **Portal Search Board** Frontend Microservice `c8fms06` presents a new window or form prompting the user to provide any required transfer details (e.g., target environment, specific configurations, transfer notes). The user fills in the required transfer details and submits the form. The **Portal Search Board** Frontend Microservice `c8fms06` displays a confirmation window asking the user to confirm the image transfer. The user clicks **"Yes"** to confirm the transfer. 

### 2. `c8fms06-c8bms18-e10`: The **Portal Search Board** Frontend Microservice `c8fms06` sends a request, containing the transfer details, to the **Transfer Image Logic** Backend Microservice `c8bms18` to initiate the image transfer.

* **Endpoint:** `https://orbit.k8or.com/transfer/TransferImage.php`
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**

```json
{
  "PID": "c8fms06-c8bms18-e10",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development",
    "transferEnvironment": "Testing"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c8fms06-c8bms18-e10",
  "status": "success",
  "message": "The image has been transferred successfully."
}
```

### 3. `c8bms18-c20bre21-e15`: Event Logging: The **Portal Transfer Logic** Backend Microservice `c8bms18` logs the successful image transfer event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table `c20bre21`.

* **Endpoint:** `https://orbit.k8or.com/transfer/InitiateTransferImageUserEventLogChartStore.php`
* **Event Type:** `transfer-initiated`
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms18-c20bre21-e15",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development",
    "transferEnvironment": "Testing"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms18-c20bre21-e15",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 4. `c8bms18-c8bms30-e20`: The `Portal Transfer Logic` Backend Microservice requests a unique Image UID from the `Portal ViewPort` Backend Microservice.
* **Endpoint:** `https://orbit.k8or.com/viewport/AssignImageUID.php`
```
upload/
├── src/
│   ├── routes/
│   │   └── AssignImageUID.php
│   ├── functions/
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request payload:** 
```json
{
  "PID": "c8bms18-c8bms30-e20",
  "imageMetadata": {
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c8bms18-c8bms30-e20",
  "status": "success",
  "message": "The image UID has been successfully assigned.",
  "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
}
```

### 5. `c8bms30-c20bre21-e25`: The `Portal ViewPort` Backend Microservice logs an `image-uid-requested` event in the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `eventType` (String)
* **Endpoint:** `https://orbit.k8or.com/viewport/RequestImageUIDUserEventLogChartStore.php`
```
upload/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── RequestImageUIDUserEventLogChartStore.php 
│   └── config.php 
├── composer.json
└── vendor/
```
* **Event Type:** `image-uid-requested`
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms30-c20bre21-e25",
  "imageMetadata": {
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
* **Response Payload:**
```json
{
  "PID": "c8bms30-c20bre21-e25",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 6. `c8bms30-c32bre03-e30`: The `Portal ViewPort` Backend Microservice fetches and assigns a unique Image UID.

* **MySQL table `wp_image_uid` Schema**:
```sql
CREATE TABLE `wp_image_uid` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `image_uid` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `image_metadata` text COLLATE utf8mb4_unicode_ci,
  `created_at` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `image_uid_index` (`image_uid`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```
* **MySQL Table `wp_image_uid` CSV Format:**
```csv
"id","image_uid","image_metadata","created_at","updated_at"
1,"8jBycF79sF2MBn5zVGqvBhLytDsHtNRSLrwXAP2yvSr1TX66sUzwnfBi7LeV","Trolls World Tour (2020), v0-0-01, Movie","2025-02-13 14:05:49","2025-02-13 14:05:49"
2,yytoP26bcePnsh8pGa3gNQAsLUXMYcpc88Zyzps2GsBaPdi8TT3BQzVsWXXr,,"2025-02-13 14:05:49",
3,Xp6BLyzknZ9PBtpXkyj2BT8v5AB6f1DiijDCGqxkH761cGQTvUbgUKqnRQaM,,"2025-02-13 14:05:49",
4,NH8AbAyNvJrFKkQCSqQkmtpioE8Vm4AKGYfL6gFEr2XWVqa3tbc22c5wZnie,,"2025-02-13 14:05:49",
5,KdXPvJvU2STtohjQfrRYM9AdZ338VhPfqTWxhCMNoviFCFqK58YFFvWxKPjt,,"2025-02-13 14:05:49",
```

* **Endpoint:** `https://orbit.k8or.com/viewport/AssignImageUIDViewPort.php`
```
upload/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── AssignImageUIDViewPort.php 
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
  "PID": "c8bms30-c32bre03-e30",
  "imageMetadata": {
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c8bms30-c32bre03-e30",
  "status": "success",
  "message": "The image UID has been successfully assigned.",
  "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
}
```

### 7. `c8bms30-c20bre21-e35`: The `Portal ViewPort` Backend Microservice logs an `image-uid-assigned` event in the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `eventType` (String)
* **Endpoint:** `https://orbit.k8or.com/upload/AssignedImageUIDUserEventLogChartStore.php`
```
upload/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── AssignedImageUIDUserEventLogChartStore.php 
│   └── config.php 
├── composer.json
└── vendor/
```
* **Event Type:** `image-uid-assigned`
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms30-c20bre21-e35",
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
* **Response Payload:**
```json
{
  "PID": "c8bms30-c20bre21-e35",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 8. `c8bms18-c16bre06-e40`: Image Retrieval from Development S3:

The **Portal Transfer Logic** Backend Microservice `c8bms18` retrieves the image file from the `app-image-1206-bkt-k8d-aws` S3 bucket `c16bre06`.

* **Endpoint:** `https://orbit.k8or.com/transfer/GetTransferImageHarbor.php`
* **Protocol:** `https`
* **Request Type:** `GET`
* **Request Payload:**
```json
{
  "PID": "c8bms18-c16bre06-e40",
  "imageMetadata": { 
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageVersion": "v0-0-01", // 'imageFileName' will be constructed based on 'imageUID' and 'imageVersion': Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5-v0-0-01.tar
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms18-c16bre06-e40",
  "status": "success",
  "imageData": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxglj2+t/AA..."
}
```

### 9. `c8bms18-c20bre21-e45`: Event Logging: The **Portal Transfer Logic** Backend Microservice `c8bms18` logs the successful image transfer event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table `c20bre21`.

* **Endpoint:** `https://orbit.k8or.com/transfer/RetrieveTransferImageUserEventLogChartStore.php`
* **Event Type:** `image-retrieved`
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms18-c20bre21-e45",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development",
    "transferEnvironment": "Testing"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms18-c20bre21-e45",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 10. `c8bms18-c16bre09-e50`: Image Upload to Testing S3: The **Portal Transfer Logic** Backend Microservice `c8bms18` uploads the retrieved image file to the `app-image-1209-bkt-k8t-aws` S3 bucket `c16bre09`. The image is *copied*, not moved or deleted from the development bucket.

* **Endpoint:** `https://orbit.k8or.com/transfer/TransferImageHarbor.php`
* **Protocol:** `https`
* **Request Type:** `GET`
* **Request Payload:**
```json
{
  "PID": "c8bms18-c16bre09-e50",
  "imageMetadata": { 
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageVersion": "v0-0-01", // 'imageFileName' will be constructed based on 'imageUID' and 'imageVersion': Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5-v0-0-01.tar
    "imageData": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxglj2+t/AA..."
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms18-c16bre09-e50",
  "status": "success",
  "message": "The image has been transferred successfully."
}
```

### 11. `c8bms18-c20bre21-e55`: Event Logging: The **Portal Transfer Logic** Backend Microservice `c8bms18` logs the successful image transfer event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table `c20bre21`.

* **Endpoint:** `https://orbit.k8or.com/transfer/UploadTransferImageUserEventLogChartStore.php`
* **Event Type:** `image-uploaded`
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms18-c20bre21-e55",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development",
    "transferEnvironment": "Testing"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms18-c20bre21-e55",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 12. `c8bms18-c20bre09-e60`: Manifest Retrieval: The **Portal Transfer Logic** Backend Microservice `c8bms18` queries the `k8or-kustomize-1609-tbl-k8d-aws` DynamoDB table `c20bre09` to retrieve the necessary kustomize manifest files associated with the transferred image.

* **Endpoint:** `https://orbit.k8or.com/transfer/GetTransferImageHelmChartStore.php`
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms18-c20bre09-e60",
  "imageMetadata": { 
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageVersion": "v0-0-01"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms18-c20bre09-e60",
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

### 13. `c8bms18-c20bre21-e65`: Event Logging: The **Portal Transfer Logic** Backend Microservice `c8bms18` logs the successful image transfer event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table `c20bre21`.

* **Endpoint:** `https://orbit.k8or.com/transfer/RetrieveTransferHelmChartUserEventLogChartStore.php`
* **Event Type:** `helm-chart-retrieved`
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms18-c20bre21-e65",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development",
    "transferEnvironment": "Testing"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms18-c20bre21-e65",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 14. `c8bms18-c20bre30-e70`: Manifest Saving to Testing: The **Portal Transfer Logic** Backend Microservice `c8bms18` saves the retrieved kustomize manifest files to the `k8or-kustomize-1630-tbl-k8t-aws` DynamoDB table `c20bre30`.

* **Endpoint:** `https://orbit.k8or.com/transfer/TransferImageHelmChartStore.php`
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms18-c20bre30-e70",
  "imageMetadata": {
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
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms18-c20bre30-e70",
  "status": "success",
  "message": "The image Helm Chart has been transferred successfully.",

}
```

### 15. `c8bms18-c20bre21-e75`: Event Logging: The **Portal Transfer Logic** Backend Microservice `c8bms18` logs the successful image transfer event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table `c20bre21`.

* **Endpoint:** `https://orbit.k8or.com/transfer/UploadTransferHelmChartUserEventLogChartStore.php`
* **Event Type:** `helm-chart-uploaded`
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms18-c20bre21-e75",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development",
    "transferEnvironment": "Testing"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms18-c20bre21-e75",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 16. `c8bms18-c8bms30-e80`: Image Labeling in Database: The **Portal Transfer Logic** Backend Microservice `c8bms18` communicates with the `Portal ViewPort` Backend Microservice to transfer the image in ViewPort.

* **Endpoint:** `https://orbit.k8or.com/transfer/TransferImageViewPort.php`
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**

```json
{
  "PID": "c8bms18-c8bms30-e80",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageVersion": "v0-0-01",
    "currentEnvironment": "Development",
    "transferEnvironment": "Testing"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms18-c8bms30-e80",
  "status": "success",
  "message": "The image environment has been updated successfully."
}
```

### 17. `c8bms30-c32bre03-e85`: Image Labeling in Database: The **Portal Transfer Logic** Backend Microservice `c8bms18` communicates with the MySQL database `c32bre03` to update the image's metadata, marking it with a **"Testing"** label.

* **Endpoint:** `https://orbit.k8or.com/transfer/TransferImageViewPort.php`
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**

```json
{
  "PID": "c8bms30-c32bre03-e85",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageVersion": "v0-0-01",
    "currentEnvironment": "Development",
    "transferEnvironment": "Testing"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms30-c32bre03-e85",
  "status": "success",
  "message": "The image environment has been updated successfully."
}
```

### 18. `c8bms30-c20bre21-e90`: Event Logging: The **Portal Transfer Logic** Backend Microservice `c8bms18` logs the successful image transfer event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table `c20bre21`.

* **Endpoint:** `https://orbit.k8or.com/transfer/UpdateTransferImageUserEventLogChartStore.php`
* **Event Type:** `viewport-updated`
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms30-c20bre21-e90",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development",
    "transferEnvironment": "Testing"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms30-c20bre21-e90",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 19. `c8bms18-c20bre21-e95`: Event Logging: The **Portal Transfer Logic** Backend Microservice `c8bms18` logs the successful image transfer event to the `user-event-log-1621-tbl-k8d-aws` DynamoDB table `c20bre21`.

* **Endpoint:** `https://orbit.k8or.com/transfer/TransferImageUserEventLogChartStore.php`
* **Event Type:** `image-transferred`
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms18-c20bre21-e95",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5", // 60 digits, alphanumeric string
    "imageName": "Trolls World Tour (2020)",
    "imageVersion": "v0-0-01",
    "imageCategory": "Movie",
    "Description": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "Part": "1234",
    "currentEnvironment": "Development",
    "transferEnvironment": "Testing"
  },
  "userMetadata": {
    "username": "jamesbond",
    "firstName": "James",
    "lastName": "Bond",
    "email": "jamesbond@k8or.com"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms18-c20bre21-e95",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 20. `c8bms18-c20bre24-911`: Error Logging: If any errors occurred during the transfer, the **Portal Transfer Logic** Backend Microservice `c8bms18` writes the error details to the `user-error-log-1624-tbl-k8d-aws` DynamoDB table `c20bre24`.

* **Endpoint:** `https://orbit.k8or.com/transfer/TransferImageUserErrorLogChartStore.php`
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms18-c20bre24-911",
  "error": {}
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms18-c20bre24-911",
  "status": "success",
  "message": "The user error was recorded successfully.",
}
```