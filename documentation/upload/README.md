# k8or Portal Upload Image Process

**Component C8**, the **k8or Portal**, is deployed on an EC2 instance designated as the `k8or Portal node`. This node functions similarly to a node within an EKS or K3s cluster, hosting multiple microservices (MSVs). Each functional process (`upload`, `search`, `transfer`, `deploy`, `scan`, `chart`, `delete`, and future additions) is handled by dedicated microservices. Processes requiring a user interface have both frontend (FMS) and backend (BMS) microservices, while backend-only processes utilize only Backend Microservices.

---

## Configuration Resource list for the `Portal Image Upload` Process:
* **EC2 instances**

## Development Resource list for the `Portal Image Upload` Process:

* **`Portal Upload Board` Frontend Microservice with MID (Microservice Identitication) `c8fms03`**
* **`Portal Upload Logic` Backend Microservice with MID `c8bms06`**
* **`Manifestor Image Validator` Backend Microservice with MID `c12bms03`**
* **`Manifestor Save Metadata` Backend Microservice with MID `c12bms06`**
* **`Manifestor Chart Generator` Backend Microservice with MID `c12bms09`**

## Creation Resource List for the `Portal Image Upload` Process:
* **DynamoDB Tables:** 
    1. **`base-kustomize-1603-tbl-k8d-aws` DynamoDB Table with RID (Resource Identification) `c20bre03`**
    2. **`user-input-manifest-data-1606-tbl-k8d-aws` DynamoDB Table with RID `c20bre06`**
    3. **`k8or-kustomize-1609-tbl-k8d-aws` DynamoDB Table with RID `c20bre09`**
    4. **`user-metadata-1612-tbl-k8d-aws` DynamoDB Table with RID `c20bre12`**
    5. **`app-image-metadata-1615-tbl-k8d-aws` DynamoDB Table with RID `c20bre15`**
    6. **`user-event-log-1621-tbl-k8d-aws` DynamoDB Table with RID `c20bre21`**
    7. **`user-error-log-1624-tbl-k8d-aws` DynamoDB Table with RID `c20bre24`**
    8. **`portal-upload-static-data-1627-tbl-k8d-aws` DynamoDB Table with RID `c20bre27`**
* **S3 Buckets:** 
    1. **`temporary-app-image-1203-bkt-k8d-aws` S3 Bucket with RID `c16bre03`**
    2. **`app-image-1206-bkt-k8d-aws` S3 Bucket with RID `c16bre06`**

* **MySQL database:** 
    1. **`app_image_mql_2003_dtb_k8d_aws` MySQL Database with RID `c32bre03`**
    * **MySQL Tables:**
        1. **`wp_options` MySQL Table with RID `c32bre06`**
        2. **`wp_postmeta` MySQL Table with RID `c32bre09`**
        3. **`wp_posts` MySQL Table with RID `c32bre12`**
        4. **`wp_term_relationships` MySQL Table with RID `c32bre15`**
        5. **`wp_term_taxonomy` MySQL Table with RID `c32bre18`**
        6. **`wp_terms` MySQL Table with RID `c32bre21`**
        7. **`wp_usermeta` MySQL Table with RID `c32bre24`**
        8. **`wp_users` MySQL Table with RID `c32bre27`**
        9. **`wp_wpdatatables` MySQL Table with RID `c32bre30`**
        10. **`wp_wpdatatables_columns` MySQL Table with RID `c32bre33`**

---

## The `k8or Portal node` currently hosts the following microservices:

1.  **`Portal Upload Board` Frontend Microservice** (`c8fms03`)
2.  **`Portal Upload Logic` Backend Microservice** (`c8bms06`)
3.  **`Portal Search Board` Frontend Microservice** (`c8fms09`)
4.  **`Portal Search Logic` Backend Microservice** (`c8bms12`)
5.  **`Portal Deploy Logic` Backend Microservice** (`c8bms15`)
6.  **`Portal Transfer Logic` Backend Microservice** (`c8bms06`)
7.  **`Portal Scan Logic` Backend Microservice** (`c8bms21`)
8.  **`Portal Chart Logic` Backend Microservice** (`c8bms24`)
9.  **`Portal Delete Logic` Backend Microservice** (`c8bms27`)

## The `k8or Portal node` microservices talks to the following external resources (outside the node):

* **Microservices:**

    1. **`Manifestor Image Validator` Backend Microservice (`c12bms03`)**
        * **Purpose:** The purpose of the **`Manifestor Image Validator` Backend Microservice (`c12bms03`)** is to validate uploaded images. It checks image format, size, dimensions, file integrity, and potentially other criteria to ensure the image meets the system's requirements and is safe for processing.  This helps prevent corrupted images, malicious uploads, and ensures compatibility within the system.

    2. **`Manifestor Save Metadata` Backend Microservice (`c12bms06`)**
        * **Purpose:** The purpose of the **`Manifestor Save Metadata` Backend Microservice (`c12bms06`)** is to persist image and user metadata. It stores information like image name, size, upload date, user ID, and any other relevant details in various DynamoDB tables. This metadata is used for searching, managing, and tracking uploaded images, as well as associating them with the uploading user.

    3. **`Manifestor Chart Generator` Backend Microservice (`c12bms09`)**
        * **Purpose:** The purpose of the **`Manifestor Chart Generator` Backend Microservice (`c12bms09`)** is to generate kustomize manifest files for deploying applications to Kubernetes clusters. It retrieves base manifest templates and combines them with user-provided configuration data to create customized deployment configurations. This allows users to define their application's specific requirements (e.g., number of replicas, resource limits, service ports) and generate the necessary Kubernetes YAML files for deployment.

* **Backend Resources:**

    * **DynamoDB Tables:** 
        1. **`base-kustomize-1603-tbl-k8d-aws` (`c20bre03`) DynamoDB Table**
            * **Purpose:** The purpose of the **`base-kustomize-1603-tbl-k8d-aws` (`c20bre03`) DynamoDB table** is to store base kustomize manifest templates. These templates provide a foundation for generating application deployments and are customized with user-provided input.  They contain the standard, unchanging parts of the manifest.

        2. **`user-input-manifest-data-1606-tbl-k8d-aws` (`c20bre06`) DynamoDB Table**
            * **Purpose:** The purpose of the **`user-input-manifest-data-1606-tbl-k8d-aws` (`c20bre06`) DynamoDB table** is to store user-provided configuration data for kustomize manifest generation. This data allows users to customize their application deployments, specifying parameters like resource allocation, environment variables, and other deployment-specific settings.

        3. **`k8or-kustomize-1609-tbl-k8d-aws` (`c20bre09`) DynamoDB Table**
            * **Purpose:** The purpose of the **`k8or-kustomize-1609-tbl-k8d-aws` (`c20bre09`) DynamoDB table** is to store the generated, customized kustomize manifest files. These are the final deployment configurations, ready to be applied to a Kubernetes cluster.

        4. **`user-metadata-1612-tbl-k8d-aws` (`c20bre12`) DynamoDB Table**
            * **Purpose:** The purpose of the **`user-metadata-1612-tbl-k8d-aws` (`c20bre12`) DynamoDB table** is to store metadata related to users of the k8or portal. This might include user profiles, contact information, roles, permissions, and other user-specific attributes.

        5. **`app-image-metadata-1615-tbl-k8d-aws` (`c20bre15`) DynamoDB Table**
            * **Purpose:** The purpose of the **`app-image-metadata-1615-tbl-k8d-aws` (`c20bre15`) DynamoDB table** is to store metadata about uploaded application images. This includes details like image name, size, format, resolution, upload timestamp, associated user, and any other relevant information about the image itself.

        6. **`user-event-log-1621-tbl-k8d-aws` (`c20bre21`) DynamoDB Table**
            * **Purpose:** The purpose of the **`user-event-log-1621-tbl-k8d-aws` (`c20bre21`) DynamoDB table** is to store logs of user activity within the k8or portal. This table records user actions, such as logins, image uploads, manifest generation, deployments, and other significant events, providing an audit trail of user interactions.

        7. **`user-error-log-1624-tbl-k8d-aws` (`c20bre24`) DynamoDB Table**
            * **Purpose:** The purpose of the **`user-error-log-1624-tbl-k8d-aws` (`c20bre24`) DynamoDB table** is to store error logs related to user actions. This table captures any errors encountered during user operations, providing valuable information for debugging and troubleshooting.

        8. **`portal-upload-static-data-1627-tbl-k8d-aws` (`c20bre27`) DynamoDB Table**
            * **Purpose:** The purpose of the **`portal-upload-static-data-1627-tbl-k8d-aws` (`c20bre27`) DynamoDB table** is to store static data required for the image upload process.

    * **S3 Buckets:** 
        1. **`temporary-app-image-1203-bkt-k8d-aws` (`c16bre03`) S3 Bucket**
            * **Purpose:** The purpose of the **`temporary-app-image-1203-bkt-k8d-aws` (`c16bre03`) S3 bucket** is to serve as a temporary staging area for uploaded images *before* validation. Images are initially stored here for processing by the Manifestor Image Validator.  This separation allows for processing and validation without affecting the main image storage.

        2. **`app-image-1206-bkt-k8d-aws` (`c16bre06`) S3 Bucket**
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

### Process ID (PID) Structure

Each step in the process flows is assigned a unique Process ID (PID) to facilitate tracking and analysis. The PID follows a structured format:

```
PID: <start-resource>-<end-resource>-e<execution_order>
```

1.  **Resource Identifiers:** The PID identifies the starting and ending resources involved in a specific process step. Resources can be either microservices (MSVs, e.g., `fms09`, `bms12`) backend resources (e.g., `bre03`, `bre06`), or a Frontend User (e.g., `usr03`, `usr06`).
2.  **Component ID Prefix:** Both the start and end resource identifiers are prefixed with a component ID (e.g., `c2`, `c4`, `c8`) to specify the component to which the resource belongs. For example, `c8fms09` indicates that `fms09` is part of component `c8`.
3.  **Execution Order:** The `e<execution_order>` component of the PID indicates the sequence of execution. The `e` prefix denotes `execution`, and the subsequent number represents the step number in increments of 5 (e.g., `e05`, `e10`, `e15`, `e20`). This allows for easy identification of the order in which the processes occur.  For instance, `e05` represents the first step, `e10` the second, and so on.

# Process Flows

### Diagram: [View](https://github.com/k8or-orbit-aws/k8or-orbit-doc-0103-rep-k8d-aws/tree/k8or-dev/upload-dir/k8or-portal-upload-v0-0-09-dia-k8d.drawio.pdf)

![Alt text](https://github.com/k8or-orbit-aws/k8or-orbit-doc-0103-rep-k8d-aws/blob/k8or-dev/upload-dir/k8or-portal-upload-v0-0-09-dia-k8d.jpg)

## 1. Static Data Retrieval (Before Form Submission):

The frontend *first* requests static data (e.g., dropdown options, default values) from the **Portal Upload Logic** Backend Microservice. It sends a request to the `/upload/GetUploadImageStaticData.php` endpoint. This request *must complete* before the user can interact with the upload form.

### 1.1. **`c2usr03-c8fms03-e05`:** User navigates to the `Image Upload Page`.
* **Endpoint:** `https://orbit.k8or.com/`
* **Protocol type:** `https`
* **Request type:** `GET`
* **Synchronicity:** `async` 
* **Request payload:** none
* **Response payload:** none

### 1.2. **`c8fms03-c8bms06-e10`:** The `Portal Upload Board` Frontend Microservice requests static data (e.g., dropdown options) from the `Portal Upload Logic` Backend Microservice.
* **Endpoint:** `https://orbit.k8or.com/upload/GetUploadImageStaticData.php`
```
upload/
├── src/
│   ├── routes/
│   │   ├── GetUploadImageStaticData.php
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
  "PID": "c8fms03-c8bms06-e10",
  "ICAOCode": "1947",
  "username": "jamesbond"
}
```
* **Response payload:**
```json
{
  "PID": "c8fms03-c8bms06-e10",
  "filter_criteria": {
    "ICAOCode": "1947",
    "aircraftTailNumber": [
      "41-38116",
      "42-13400"
    ],
    "imageCategory": [
      "Games",
      "Movies",
      "Books",
      "Shopping",
      "Weather",
      "Video"
    ],
    "partNumber": [
      "21117-1",
      "211-171"
    ]
  }
}
```

### 1.3. **`c8bms06-c20bre27-e15`:** The `Portal Upload Logic` Backend Microservice retrieves the required static data from the `portal-upload-static-data-1627-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `ICAOCode` (String)
    * **Sort key:** none
```json
{
  "ICAOCode": {
    "S": "1947"
  },
  "aircraftTailNumber": {
    "L": [
      {
        "S": "41-38116"
      },
      {
        "S": "42-13400"
      }
    ]
  },
  "imageCategory": {
    "L": [
      {
        "S": "Games"
      },
      {
        "S": "Movies"
      },
      {
        "S": "Books"
      },
      {
        "S": "Shopping"
      },
      {
        "S": "Weather"
      },
      {
        "S": "Video"
      }
    ]
  },
  "partNumber": {
    "L": [
      {
        "S": "21117-1"
      },
      {
        "S": "211-171"
      }
    ]
  }, 
  "createdTimestampUTC": {
    "S": "2025-02-18 08:49:02"
  },
  "createdTimestampUnix": {
    "S": "1739632730"
  }
}
```
* **Endpoint:** `https://orbit.k8or.com/upload/GetUploadImageStaticDataChartStore.php`
```
upload/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── GetUploadImageStaticDataChartStore.php
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
  "PID": "c8bms06-c20bre27-e15",
  "ICAOCode": "1947",
  "username": "jamesbond"
}
```
* **Response payload:** 
```json
{
  "PID": "c8bms06-c20bre27-e15",
  "ICAOCode": "1947",
  "aircraftTailNumber": [
    "41-38116",
    "42-13400"
  ],
  "imageCategory": [
    "Games",
    "Movies",
    "Books",
    "Shopping",
    "Weather",
    "Video"
  ],
  "partNumber": [
    "21117-1",
    "211-171"
  ]
}
```

### 1.4. **`c8bms06-c32bre03-e20`:** The `Portal Upload Logic` Backend Microservice retrieves all `display_name` values from the `wp_users` ViewPort MySQL database table.
* **MySQL Table Schema:**
```sql
CREATE TABLE `wp_users` (
  `ID` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `user_login` varchar(60) COLLATE utf8mb4_unicode_520_ci NOT NULL DEFAULT '',
  `user_pass` varchar(255) COLLATE utf8mb4_unicode_520_ci NOT NULL DEFAULT '',
  `user_nicename` varchar(50) COLLATE utf8mb4_unicode_520_ci NOT NULL DEFAULT '',
  `user_email` varchar(100) COLLATE utf8mb4_unicode_520_ci NOT NULL DEFAULT '',
  `user_url` varchar(100) COLLATE utf8mb4_unicode_520_ci NOT NULL DEFAULT '',
  `user_registered` datetime NOT NULL DEFAULT '0000-00-00 00:00:00',
  `user_activation_key` varchar(255) COLLATE utf8mb4_unicode_520_ci NOT NULL DEFAULT '',
  `user_status` int(11) NOT NULL DEFAULT '0',
  `display_name` varchar(250) COLLATE utf8mb4_unicode_520_ci NOT NULL DEFAULT '',
  PRIMARY KEY (`ID`),
  KEY `user_login_key` (`user_login`),
  KEY `user_nicename` (`user_nicename`),
  KEY `user_email` (`user_email`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_520_ci;
```
* **CSV Structure:**
```csv
"ID","user_login","user_pass","user_nicename","user_email","user_url","user_registered","user_activation_key","user_status","display_name"
2,jamesbond,,jamesbond,jamesbond@k8or.com,http://www.k8or.com,"2025-02-07 11:45:49","",0,James Bond
3,madamecoco,,madamecoco,madamecoco@k8or.com,http://www.k8or.com,"2025-02-07 12:07:10","",0,Madame Coco
4,k8or_admin,,k8or_admin,admin@k8or.com,http://www.k8or.com,"2025-02-18 08:49:02","",0,k8or Admin

```
* **Endpoint:** `https://orbit.k8or.com/upload/GetUserDataViewPort.php`
```
upload/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── GetUserDataViewPort.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `GET`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** 
```json
{
  "PID": "c8bms06-c32bre03-e20",
  "user_data": [
  {
    "ID": 2,
    "display_name": "James Bond"
  },
  {
    "ID": 3,
    "display_name": "Madame Coco"
  },
  {
    "ID": 4,
    "display_name": "k8or Admin"
  }
]
}
```

### 1.5. **`c8bms06-c20bre24-EEE`:** The `Portal Upload Logic` Backend Microservice writes error logs to the `user-error-log-1624-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `errorType` (String)
* **Endpoint:** `https://orbit.k8or.com/upload/GetUserDataViewPort.php`
```
upload/
├── src/
│   ├── routes/
│   │   └── ErrorLog.php
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

---

## 2. Image UID Assignment (After Form Submission):

*After* the user submits the image upload form, the frontend requests a unique image ID (UID) from the **Portal Upload Logic** Backend Microservice by sending a request to the `/upload/AssignImageUID.php` endpoint. This request *must complete* before proceeding.

### 2.1. **`c8fms03-c8bms06-e25`** The `Portal Upload Board` Frontend Microservice requests a unique Image UID from the `Portal Upload Logic` Backend Microservice.
* **Endpoint:** `https://orbit.k8or.com/upload/AssignImageUID.php`
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
  "PID": "c8fms03-c8bms06-e25",
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
  "PID": "c8fms03-c8bms06-e25",
  "status": "success",
  "message": "The image UID has been successfully assigned.",
  "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
}
```

### 2.2. **`c8bms06-c20bre21-e30`:** The `Portal Upload Logic` Backend Microservice logs an `image-uid-requested` event in the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `eventType` (String)
```json
{
  "username": {
    "S": "jamesbond"
  },
  "eventType": {
    "S": "image-uid-requested"
  },
  "imageMetadata": {
    "M": {
      "icao": {
        "S": "British"
      },
      "tail": {
        "S": "G-AAAA"
      },
      "Part": {
        "S": "21117-1"
      },
      "imageName": {
        "S": "Trolls World Tour (2020)"
      },
      "imageVersion": {
        "S": "v0-0-01"
      },
      "imageCategory": {
        "S": "Movie"
      },
      "Description": {
        "S": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair."
      },
      "port": {
        "S": "80"
      },
      "envVars": {
        "L": [
          {
            "M": {
              "key": {
                "S": "VAR1"
              },
              "value": {
                "S": "value1"
              }
            }
          },
          {
            "M": {
              "key": {
                "S": "VAR2"
              },
              "value": {
                "S": "value2"
              }
            }
          }
        ]
      },
      "volumes": {
        "L": [
          {
            "M": {
              "key": {
                "S": "/host/path"
              },
              "value": {
                "S": "/container/path"
              }
            }
          }
        ]
      },
      "imageLabels": {
        "L": [
          {
            "M": {
              "key": {
                "S": "label1"
              },
              "value": {
                "S": "value1"
              }
            }
          }
        ]
      },
      "commandOverride": {
        "S": "/app/run.sh"
      },
      "entrypointOverride": {
        "S": "/app/entrypoint.sh"
      },
      "workingDir": {
        "S": "/app"
      },
      "image": {
        "S": "data:image/png;base64,iVBORw0KGgo"
      }
    }
  },
  "userMetadata": {
    "M": {
      "firstName": {
        "S": "James"
      },
      "lastName": {
        "S": "Bond"
      },
      "email": {
        "S": "jamesbond@k8or.com"
      }
    }
  },
  "eventMetadata": {
    "M": {
      "timestamp": {
        "S": "timestamp"
      }
    }
  }, 
  "createdTimestampUTC": {
    "S": "2025-02-18 08:49:02"
  },
  "createdTimestampUnix": {
    "S": "1739632730"
  }
}
```
* **Endpoint:** `https://orbit.k8or.com/upload/RequestImageUIDUserEventLogChartStore.php`
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
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms06-c20bre21-e30",
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
  },
  "eventMetadata": {
    "eventType": "image-uid-requested",
    "timestamp": "timestamp"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms06-c20bre21-e30",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 2.3. **`c8bms06-c32bre03-e35`:** The `Portal Upload Logic` Backend Microservice fetches and assigns a unique Image UID.

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

* **Endpoint:** `https://orbit.k8or.com/upload/AssignImageUIDViewPort.php`
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
  "PID": "c8bms06-c32bre03-e35",
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
  "PID": "c8bms06-c32bre03-e35",
  "status": "success",
  "message": "The image UID has been successfully assigned.",
  "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
}
```

### 2.4. **`c8bms06-c20bre21-e40`:** The `Portal Upload Logic` Backend Microservice logs an `image-uid-assigned` event in the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `eventType` (String)
```json
{
  "username": {
    "S": "jamesbond"
  },
  "eventType": {
    "S": "image-uid-assigned"
  },
  "imageMetadata": {
    "M": {
      "imageUID": {
        "S": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
      },
      "icao": {
        "S": "British"
      },
      "tail": {
        "S": "G-AAAA"
      },
      "Part": {
        "S": "21117-1"
      },
      "imageName": {
        "S": "Trolls World Tour (2020)"
      },
      "imageVersion": {
        "S": "v0-0-01"
      },
      "imageCategory": {
        "S": "Movie"
      },
      "Description": {
        "S": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair."
      },
      "port": {
        "S": "80"
      },
      "envVars": {
        "L": [
          {
            "M": {
              "key": {
                "S": "VAR1"
              },
              "value": {
                "S": "value1"
              }
            }
          },
          {
            "M": {
              "key": {
                "S": "VAR2"
              },
              "value": {
                "S": "value2"
              }
            }
          }
        ]
      },
      "volumes": {
        "L": [
          {
            "M": {
              "key": {
                "S": "/host/path"
              },
              "value": {
                "S": "/container/path"
              }
            }
          }
        ]
      },
      "imageLabels": {
        "L": [
          {
            "M": {
              "key": {
                "S": "label1"
              },
              "value": {
                "S": "value1"
              }
            }
          }
        ]
      },
      "commandOverride": {
        "S": "/app/run.sh"
      },
      "entrypointOverride": {
        "S": "/app/entrypoint.sh"
      },
      "workingDir": {
        "S": "/app"
      },
      "image": {
        "S": "data:image/png;base64,iVBORw0KGgo"
      }
    }
  },
  "userMetadata": {
    "M": {
      "firstName": {
        "S": "James"
      },
      "lastName": {
        "S": "Bond"
      },
      "email": {
        "S": "jamesbond@k8or.com"
      }
    }
  },
  "eventMetadata": {
    "M": {
      "timestamp": {
        "S": "timestamp"
      }
    }
  }, 
  "createdTimestampUTC": {
    "S": "2025-02-18 08:49:02"
  },
  "createdTimestampUnix": {
    "S": "1739632730"
  }
}
```
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
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms06-c20bre21-e40",
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
  },
  "eventMetadata": {
    "eventType": "image-uid-assigned",
    "timestamp": "timestamp"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms06-c20bre21-e40",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 2.5. **`c8bms06-c20bre24-EEE`:** The `Portal Upload Logic` Backend Microservice writes error logs to the `user-error-log-1624-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `errorType` (String)
* **Endpoint:** `https://orbit.k8or.com/upload/GetUserDataViewPort.php`
```
upload/
├── src/
│   ├── routes/
│   │   └── ErrorLog.php
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

---

## 3. Image Validation (After Form Submission):

*After* receiving the UID, the frontend sends the image file to the **Manifestor Image Validator** Backend Microservice for validation. This request goes to the `/validate/ValidateUploadedImage.php` endpoint. This request *must complete* before proceeding.

### 3.1. **`c8fms03-c12bms03-e45`:** The `Portal Upload Board` Frontend Microservice sends the image to the `Manifestor Image Validator` Backend Microservice.
* **Endpoint:** `https://orbit.k8or.com/validate/ValidateUploadedImage.php`
```
validate/
├── src/
│   ├── routes/
│   │   └── ValidateUploadedImage.php
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
  "PID": "c8fms03-c12bms03-e45",
  "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5",
  "imageVersion": "v0-0-01",
  "imageData": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxglj2+t/AA..."
}
```
* **Response payload:** 
```json
{
  "PID": "c8fms03-c12bms03-e45",
  "status": "success",
  "message": "Image validated successfully."
}
```

### 3.2. **`c12bms03-c16bre03-e50`:** The `Manifestor Image Validator` Backend Microservice immediately saves the image to the `temporary-app-image-1203-bkt-k8d-aws` S3 bucket.
* **Endpoint:** `https://orbit.k8or.com/validate/TemporaryUploadImageHarbor.php`
```
validate/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── TemporaryUploadImageHarbor.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol:** `https`
* **Request Type:** `GET`
* **Request Payload:**
```json
{
  "PID": "c12bms03-c16bre03-e50",
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
  "PID": "c12bms03-c16bre03-e50",
  "status": "success",
  "message": "The image has been transferred successfully."
}
```

### 3.3. **`c12bms03-c20bre21-e55`:** The `Manifestor Image Validator` Backend Microservice logs a `temporary-image-saved` event in the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `eventType` (String)
```json
{
  "username": {
    "S": "jamesbond"
  },
  "eventType": {
    "S": "image-temporary-saved"
  },
  "imageMetadata": {
    "M": {
      "imageUID": {
        "S": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
      },
      "icao": {
        "S": "British"
      },
      "tail": {
        "S": "G-AAAA"
      },
      "Part": {
        "S": "21117-1"
      },
      "imageName": {
        "S": "Trolls World Tour (2020)"
      },
      "imageVersion": {
        "S": "v0-0-01"
      },
      "imageCategory": {
        "S": "Movie"
      },
      "Description": {
        "S": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair."
      },
      "port": {
        "S": "80"
      },
      "envVars": {
        "L": [
          {
            "M": {
              "key": {
                "S": "VAR1"
              },
              "value": {
                "S": "value1"
              }
            }
          },
          {
            "M": {
              "key": {
                "S": "VAR2"
              },
              "value": {
                "S": "value2"
              }
            }
          }
        ]
      },
      "volumes": {
        "L": [
          {
            "M": {
              "key": {
                "S": "/host/path"
              },
              "value": {
                "S": "/container/path"
              }
            }
          }
        ]
      },
      "imageLabels": {
        "L": [
          {
            "M": {
              "key": {
                "S": "label1"
              },
              "value": {
                "S": "value1"
              }
            }
          }
        ]
      },
      "commandOverride": {
        "S": "/app/run.sh"
      },
      "entrypointOverride": {
        "S": "/app/entrypoint.sh"
      },
      "workingDir": {
        "S": "/app"
      },
      "image": {
        "S": "data:image/png;base64,iVBORw0KGgo"
      }
    }
  },
  "userMetadata": {
    "M": {
      "firstName": {
        "S": "James"
      },
      "lastName": {
        "S": "Bond"
      },
      "email": {
        "S": "jamesbond@k8or.com"
      }
    }
  },
  "eventMetadata": {
    "M": {
      "timestamp": {
        "S": "timestamp"
      }
    }
  }, 
  "createdTimestampUTC": {
    "S": "2025-02-18 08:49:02"
  },
  "createdTimestampUnix": {
    "S": "1739632730"
  }
}
```
* **Endpoint:** `https://orbit.k8or.com/validate/TemporaryUploadImageUserEventLogChartStore.php`
```
validate/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── TemporaryUploadImageUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c12bms03-c20bre21-e55",
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
  },
  "eventMetadata": {
    "eventType": "image-temporary-saved",
    "timestamp": "timestamp"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c12bms03-c20bre21-e55",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 3.4. **`c12bms03-c16bre03-e60`:** The `Manifestor Image Validator` Backend Microservice performs image validation checks.
* **Endpoint:** `https://orbit.k8or.com/validate/ValidateUploadImageHarbor.php`
```
validate/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── ValidateUploadImageHarbor.php
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
  "PID": "c12bms03-c16bre03-e60",
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
  "PID": "c12bms03-c16bre03-e60",
  "status": "success",
  "message": "The image has been validated successfully."
}
```

### 3.5. **`c12bms03-c16bre06-e65`:** If validation is successful, the `Manifestor Image Validator` Backend Microservice moves the validated image to the `app-image-1206-bkt-k8d-aws` S3 bucket.
* **Endpoint:** `https://orbit.k8or.com/validate/UploadImageHarbor.php`
```
validate/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── UploadImageHarbor.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol:** `https`
* **Request Type:** `GET`
* **Request Payload:**
```json
{
  "PID": "c12bms03-c16bre06-e65",
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
  "PID": "c12bms03-c16bre06-e65",
  "status": "success",
  "message": "The image has been transferred successfully."
}
```

### 3.6. **`c12bms03-c20bre21-e70`:** The `Manifestor Image Validator` Backend Microservice logs an `image-validated` event in the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `eventType` (String)
```json
{
  "username": {
    "S": "jamesbond"
  },
  "eventType": {
    "S": "image-validated"
  },
  "imageMetadata": {
    "M": {
      "imageUID": {
        "S": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
      },
      "icao": {
        "S": "British"
      },
      "tail": {
        "S": "G-AAAA"
      },
      "Part": {
        "S": "21117-1"
      },
      "imageName": {
        "S": "Trolls World Tour (2020)"
      },
      "imageVersion": {
        "S": "v0-0-01"
      },
      "imageCategory": {
        "S": "Movie"
      },
      "Description": {
        "S": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair."
      },
      "port": {
        "S": "80"
      },
      "envVars": {
        "L": [
          {
            "M": {
              "key": {
                "S": "VAR1"
              },
              "value": {
                "S": "value1"
              }
            }
          },
          {
            "M": {
              "key": {
                "S": "VAR2"
              },
              "value": {
                "S": "value2"
              }
            }
          }
        ]
      },
      "volumes": {
        "L": [
          {
            "M": {
              "key": {
                "S": "/host/path"
              },
              "value": {
                "S": "/container/path"
              }
            }
          }
        ]
      },
      "imageLabels": {
        "L": [
          {
            "M": {
              "key": {
                "S": "label1"
              },
              "value": {
                "S": "value1"
              }
            }
          }
        ]
      },
      "commandOverride": {
        "S": "/app/run.sh"
      },
      "entrypointOverride": {
        "S": "/app/entrypoint.sh"
      },
      "workingDir": {
        "S": "/app"
      },
      "image": {
        "S": "data:image/png;base64,iVBORw0KGgo"
      }
    }
  },
  "userMetadata": {
    "M": {
      "firstName": {
        "S": "James"
      },
      "lastName": {
        "S": "Bond"
      },
      "email": {
        "S": "jamesbond@k8or.com"
      }
    }
  },
  "eventMetadata": {
    "M": {
      "timestamp": {
        "S": "timestamp"
      }
    }
  }, 
  "createdTimestampUTC": {
    "S": "2025-02-18 08:49:02"
  },
  "createdTimestampUnix": {
    "S": "1739632730"
  }
}
```
* **Endpoint:** `https://orbit.k8or.com/validate/ValidatedUploadImageUserEventLogChartStore.php`
```
validate/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── ValidatedUploadImageUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c12bms03-c20bre21-e70",
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
  },
  "eventMetadata": {
    "eventType": "image-validated",
    "timestamp": "timestamp"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c12bms03-c20bre21-e70",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 3.7. **`c12bms03-c20bre24-EEE`:** The `Manifestor Image Validator` Backend Microservice writes error logs to the `user-error-log-1624-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `errorType` (String)
* **Endpoint:** `https://orbit.k8or.com/validate/GetUserDataViewPort.php`
```
validate/
├── src/
│   ├── routes/
│   │   └── ErrorLog.php
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

---

## 4. Metadata Saving (After Form Submission):

*After* successful validation, the frontend sends the user-provided data and metadata (image name, version, category, etc.) along with the assigned UID to the Manifestor Save Metadata Backend Microservice. This data/metadata is sent to the `/metadata/SaveUploadedImageMetadata.php` endpoint. This request *must complete* before proceeding.

### 4.1. **`c8fms03-c12bms06-e75`:** The `Portal Upload Board` Frontend Microservice sends the image metadata and user input to the `Manifestor Save Metadata` Backend Microservice.
* **Endpoint:** `https://orbit.k8or.com/metadata/SaveUploadedImageMetadata.php`
```
metadata/
├── src/
│   ├── routes/
│   │   └── SaveUploadedImageMetadata.php
│   ├── functions/
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request Payload:**
```json
{
  "PID": "c8fms03-c12bms06-e75",
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
  "PID": "c8fms03-c12bms06-e75",
  "status": "success",
  "message": "The user data was recorded successfully.",
}
```

### 4.2. **`c12bms06-c20bre12-e80`:** The `Manifestor Save Metadata` Backend Microservice saves the user data to the `user-data-1612-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** none
```json
{
  "username": {
    "S": "jamesbond"
  },
  "imageMetadata": {
    "M": {
      "imageUID": {
        "S": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
      },
      "icao": {
        "S": "British"
      },
      "tail": {
        "S": "G-AAAA"
      },
      "Part": {
        "S": "21117-1"
      },
      "imageName": {
        "S": "Trolls World Tour (2020)"
      },
      "imageVersion": {
        "S": "v0-0-01"
      },
      "imageCategory": {
        "S": "Movie"
      },
      "Description": {
        "S": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair."
      },
      "port": {
        "S": "80"
      },
      "envVars": {
        "L": [
          {
            "M": {
              "key": {
                "S": "VAR1"
              },
              "value": {
                "S": "value1"
              }
            }
          },
          {
            "M": {
              "key": {
                "S": "VAR2"
              },
              "value": {
                "S": "value2"
              }
            }
          }
        ]
      },
      "volumes": {
        "L": [
          {
            "M": {
              "key": {
                "S": "/host/path"
              },
              "value": {
                "S": "/container/path"
              }
            }
          }
        ]
      },
      "imageLabels": {
        "L": [
          {
            "M": {
              "key": {
                "S": "label1"
              },
              "value": {
                "S": "value1"
              }
            }
          }
        ]
      },
      "commandOverride": {
        "S": "/app/run.sh"
      },
      "entrypointOverride": {
        "S": "/app/entrypoint.sh"
      },
      "workingDir": {
        "S": "/app"
      },
      "image": {
        "S": "data:image/png;base64,iVBORw0KGgo"
      }
    }
  },
  "userMetadata": {
    "M": {
      "firstName": {
        "S": "James"
      },
      "lastName": {
        "S": "Bond"
      },
      "email": {
        "S": "jamesbond@k8or.com"
      }
    }
  }, 
  "createdTimestampUTC": {
    "S": "2025-02-18 08:49:02"
  },
  "createdTimestampUnix": {
    "S": "1739632730"
  }
}
```
* **Endpoint:** `https://orbit.k8or.com/metadata/SaveUserDataChartStore.php`
```
metadata/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── SaveUserDataChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request Payload:**
```json
{
  "PID": "c12bms06-c20bre12-e80",
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
  "PID": "c12bms06-c20bre12-e80",
  "status": "success",
  "message": "The user data was recorded successfully.",
}
```

### 4.3. **`c12bms06-c20bre21-e85`:** The `Manifestor Save Metadata` Backend Microservice logs a `user-data-saved` event in the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `eventType` (String)
```json
{
  "username": {
    "S": "jamesbond"
  },
  "eventType": {
    "S": "user-data-saved"
  },
  "imageMetadata": {
    "M": {
      "imageUID": {
        "S": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
      },
      "icao": {
        "S": "British"
      },
      "tail": {
        "S": "G-AAAA"
      },
      "Part": {
        "S": "21117-1"
      },
      "imageName": {
        "S": "Trolls World Tour (2020)"
      },
      "imageVersion": {
        "S": "v0-0-01"
      },
      "imageCategory": {
        "S": "Movie"
      },
      "Description": {
        "S": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair."
      },
      "port": {
        "S": "80"
      },
      "envVars": {
        "L": [
          {
            "M": {
              "key": {
                "S": "VAR1"
              },
              "value": {
                "S": "value1"
              }
            }
          },
          {
            "M": {
              "key": {
                "S": "VAR2"
              },
              "value": {
                "S": "value2"
              }
            }
          }
        ]
      },
      "volumes": {
        "L": [
          {
            "M": {
              "key": {
                "S": "/host/path"
              },
              "value": {
                "S": "/container/path"
              }
            }
          }
        ]
      },
      "imageLabels": {
        "L": [
          {
            "M": {
              "key": {
                "S": "label1"
              },
              "value": {
                "S": "value1"
              }
            }
          }
        ]
      },
      "commandOverride": {
        "S": "/app/run.sh"
      },
      "entrypointOverride": {
        "S": "/app/entrypoint.sh"
      },
      "workingDir": {
        "S": "/app"
      },
      "image": {
        "S": "data:image/png;base64,iVBORw0KGgo"
      }
    }
  },
  "userMetadata": {
    "M": {
      "firstName": {
        "S": "James"
      },
      "lastName": {
        "S": "Bond"
      },
      "email": {
        "S": "jamesbond@k8or.com"
      }
    }
  }, 
  "createdTimestampUTC": {
    "S": "2025-02-18 08:49:02"
  },
  "createdTimestampUnix": {
    "S": "1739632730"
  }
}
```
* **Endpoint:** `https://orbit.k8or.com/metadata/SavedUserDataUserEventLogChartStore.php`
```
metadata/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── SavedUserDataUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c12bms06-c20bre21-e85",
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
  },
  "eventMetadata": {
    "eventType": "user-data-saved",
    "timestamp": "timestamp"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c12bms06-c20bre21-e85",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 4.4. **`c12bms06-c20bre15-e90`:** The `Manifestor Save Metadata` Backend Microservice saves the image metadata to the `app-image-metadata-1615-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `part-number` (String)
    * **Sort key:** `tail-name` (String)
```json
{
  "part-number": {
    "S": "21117-1"
  },
  "tail-name": {
    "S": "G-AAAA"
  },
  "imageMetadata": {
    "M": {
      "imageUID": {
        "S": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
      },
      "icao": {
        "S": "British"
      },
      "imageName": {
        "S": "Trolls World Tour (2020)"
      },
      "imageVersion": {
        "S": "v0-0-01"
      },
      "imageCategory": {
        "S": "Movie"
      },
      "Description": {
        "S": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair."
      },
      "port": {
        "S": "80"
      },
      "envVars": {
        "L": [
          {
            "M": {
              "key": {
                "S": "VAR1"
              },
              "value": {
                "S": "value1"
              }
            }
          },
          {
            "M": {
              "key": {
                "S": "VAR2"

              },
              "value": {
                "S": "value2"
              }
            }
          }
        ]
      },
      "volumes": {
        "L": [
          {
            "M": {
              "key": {
                "S": "/host/path"
              },
              "value": {
                "S": "/container/path"
              }
            }
          }
        ]
      },
      "imageLabels": {
        "L": [
          {
            "M": {
              "key": {
                "S": "label1"
              },
              "value": {
                "S": "value1"
              }
            }
          }
        ]
      },
      "commandOverride": {
        "S": "/app/run.sh"
      },
      "entrypointOverride": {
        "S": "/app/entrypoint.sh"
      },
      "workingDir": {
        "S": "/app"
      },
      "image": {
        "S": "data:image/png;base64,iVBORw0KGgo"
      }
    }
  },
  "userMetadata": {
    "M": {
      "username": {
        "S": "jamesbond"
      },
      "firstName": {
        "S": "James"
      },
      "lastName": {
        "S": "Bond"
      },
      "email": {
        "S": "jamesbond@k8or.com"
      }
    }
  }, 
  "createdTimestampUTC": {
    "S": "2025-02-18 08:49:02"
  },
  "createdTimestampUnix": {
    "S": "1739632730"
  }
}
```
* **Endpoint:** `https://orbit.k8or.com/metadata/SaveAppImageMetadataChartStore.php`
```
metadata/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── SaveAppImageMetadataChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request Payload:**
```json
{
  "PID": "c12bms06-c20bre15-e90",
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
  "PID": "c12bms06-c20bre15-e90",
  "status": "success",
  "message": "The user data was recorded successfully.",
}
```

### 4.5. **`c12bms06-c20bre21-e95`:** The `Manifestor Save Metadata` Backend Microservice logs an `image-metadata-saved` event in the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `eventType` (String)
```json
{
  "username": {
    "S": "jamesbond"
  },
  "eventType": {
    "S": "image-metadata-saved"
  },
  "imageMetadata": {
    "M": {
      "imageUID": {
        "S": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
      },
      "icao": {
        "S": "British"
      },
      "tail": {
        "S": "G-AAAA"
      },
      "Part": {
        "S": "21117-1"
      },
      "imageName": {
        "S": "Trolls World Tour (2020)"
      },
      "imageVersion": {
        "S": "v0-0-01"
      },
      "imageCategory": {
        "S": "Movie"
      },
      "Description": {
        "S": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair."
      },
      "port": {
        "S": "80"
      },
      "envVars": {
        "L": [
          {
            "M": {
              "key": {
                "S": "VAR1"
              },
              "value": {
                "S": "value1"
              }
            }
          },
          {
            "M": {
              "key": {
                "S": "VAR2"
              },
              "value": {
                "S": "value2"
              }
            }
          }
        ]
      },
      "volumes": {
        "L": [
          {
            "M": {
              "key": {
                "S": "/host/path"
              },
              "value": {
                "S": "/container/path"
              }
            }
          }
        ]
      },
      "imageLabels": {
        "L": [
          {
            "M": {
              "key": {
                "S": "label1"
              },
              "value": {
                "S": "value1"
              }
            }
          }
        ]
      },
      "commandOverride": {
        "S": "/app/run.sh"
      },
      "entrypointOverride": {
        "S": "/app/entrypoint.sh"
      },
      "workingDir": {
        "S": "/app"
      },
      "image": {
        "S": "data:image/png;base64,iVBORw0KGgo"
      }
    }
  },
  "userMetadata": {
    "M": {
      "firstName": {
        "S": "James"
      },
      "lastName": {
        "S": "Bond"
      },
      "email": {
        "S": "jamesbond@k8or.com"
      }
    }
  }, 
  "createdTimestampUTC": {
    "S": "2025-02-18 08:49:02"
  },
  "createdTimestampUnix": {
    "S": "1739632730"
  }
}
```
* **Endpoint:** `https://orbit.k8or.com/metadata/SavedImageMetadataUserEventLogChartStore.php`
```
metadata/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── SavedImageMetadataUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c12bms06-c20bre21-e95",
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
  },
  "eventMetadata": {
    "eventType": "image-metadata-saved",
    "timestamp": "timestamp"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c12bms06-c20bre21-e95",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 4.6. **`c12bms06-c20bre06-e100`:** The `Manifestor Save Metadata` Backend Microservice saves the user input (manifest) to the `user-input-manifest-1606-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `imageUID` (String)
    * **Sort key:** `timestamp` (String)
```json
{
  "imageUID": {
    "S": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
  },
  "timestamp": {
    "S": "timestamp"
  },
  "imageMetadata": {
    "M": {
      "icao": {
        "S": "British"
      },
      "tail": {
        "S": "G-AAAA"
      },
      "Part": {
        "S": "21117-1"
      },
      "imageName": {
        "S": "Trolls World Tour (2020)"
      },
      "imageVersion": {
        "S": "v0-0-01"
      },
      "imageCategory": {
        "S": "Movie"
      },
      "Description": {
        "S": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair."
      },
      "port": {
        "S": "80"
      },
      "envVars": {
        "L": [
          {
            "M": {
              "key": {
                "S": "VAR1"
              },
              "value": {
                "S": "value1"
              }
            }
          },
          {
            "M": {
              "key": {
                "S": "VAR2"
              },
              "value": {
                "S": "value2"
              }
            }
          }
        ]
      },
      "volumes": {
        "L": [
          {
            "M": {
              "key": {
                "S": "/host/path"
              },
              "value": {
                "S": "/container/path"
              }
            }
          }
        ]
      },
      "imageLabels": {
        "L": [
          {
            "M": {
              "key": {
                "S": "label1"
              },
              "value": {
                "S": "value1"
              }
            }
          }
        ]
      },
      "commandOverride": {
        "S": "/app/run.sh"
      },
      "entrypointOverride": {
        "S": "/app/entrypoint.sh"
      },
      "workingDir": {
        "S": "/app"
      },
      "image": {
        "S": "data:image/png;base64,iVBORw0KGgo"
      }
    }
  },
  "userMetadata": {
    "M": {
      "username": {
        "S": "jamesbond"
      },
      "firstName": {
        "S": "James"
      },
      "lastName": {
        "S": "Bond"
      },
      "email": {
        "S": "jamesbond@k8or.com"
      }
    }
  }, 
  "createdTimestampUTC": {
    "S": "2025-02-18 08:49:02"
  },
  "createdTimestampUnix": {
    "S": "1739632730"
  }
}
```
* **Endpoint:** `https://orbit.k8or.com/metadata/SaveUserInputHelmChartStore.php`
```
metadata/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── SaveUserInputHelmChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request Payload:**
```json
{
  "PID": "c12bms06-c20bre06-e100",
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
  "PID": "c12bms06-c20bre06-e100",
  "status": "success",
  "message": "The user data was recorded successfully.",
}
```

### 4.7. **`c12bms06-c20bre21-e105`:** The `Manifestor Save Metadata` Backend Microservice logs a `user-input-saved` event in the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `eventType` (String)
```json
{
  "username": {
    "S": "jamesbond"
  },
  "eventType": {
    "S": "user-input-saved"
  },
  "imageMetadata": {
    "M": {
      "imageUID": {
        "S": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
      },
      "icao": {
        "S": "British"
      },
      "tail": {
        "S": "G-AAAA"
      },
      "Part": {
        "S": "21117-1"
      },
      "imageName": {
        "S": "Trolls World Tour (2020)"
      },
      "imageVersion": {
        "S": "v0-0-01"
      },
      "imageCategory": {
        "S": "Movie"
      },
      "Description": {
        "S": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair."
      },
      "port": {
        "S": "80"
      },
      "envVars": {
        "L": [
          {
            "M": {
              "key": {
                "S": "VAR1"
              },
              "value": {
                "S": "value1"
              }
            }
          },
          {
            "M": {
              "key": {
                "S": "VAR2"
              },
              "value": {
                "S": "value2"
              }
            }
          }
        ]
      },
      "volumes": {
        "L": [
          {
            "M": {
              "key": {
                "S": "/host/path"
              },
              "value": {
                "S": "/container/path"
              }
            }
          }
        ]
      },
      "imageLabels": {
        "L": [
          {
            "M": {
              "key": {
                "S": "label1"
              },
              "value": {
                "S": "value1"
              }
            }
          }
        ]
      },
      "commandOverride": {
        "S": "/app/run.sh"
      },
      "entrypointOverride": {
        "S": "/app/entrypoint.sh"
      },
      "workingDir": {
        "S": "/app"
      },
      "image": {
        "S": "data:image/png;base64,iVBORw0KGgo"
      }
    }
  },
  "userMetadata": {
    "M": {
      "firstName": {
        "S": "James"
      },
      "lastName": {
        "S": "Bond"
      },
      "email": {
        "S": "jamesbond@k8or.com"
      }
    }
  },
  "eventMetadata": {
    "M": {
      "timestamp": {
        "S": "timestamp"
      }
    }
  }, 
  "createdTimestampUTC": {
    "S": "2025-02-18 08:49:02"
  },
  "createdTimestampUnix": {
    "S": "1739632730"
  }
}
```
* **Endpoint:** `https://orbit.k8or.com/metadata/SavedUserInputHelmChartUserEventLogChartStore.php`
```
metadata/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── SavedUserInputHelmChartUserEventLogChartStore.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c12bms06-c20bre21-e105",
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
  },
  "eventMetadata": {
    "eventType": "user-input-saved",
    "timestamp": "timestamp"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c12bms06-c20bre21-e105",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 4.8. **`c12bms06-c20bre24-EEE`:** The `Manifestor Save Metadata` Backend Microservice writes error logs to the `user-error-log-1624-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `errorType` (String)
* **Endpoint:** `https://orbit.k8or.com/metadata/GetUserDataViewPort.php`
```
metadata/
├── src/
│   ├── routes/
│   │   └── ErrorLog.php
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

---

## 5. Kustomize Helm Chart Generation (After Form Submission):

*After* saving the metadata, the frontend then triggers the generation of a Kustomize Helm chart by sending a request to the Manifestor Chart Generator Backend Microservice at the `/kustomize/GenerateKustomizeUploadedImage.php` endpoint. This request *must complete* before proceeding.

### 5.1. **`c8fms03-c12bms09-e110`:** The `Portal Upload Board` Frontend Microservice requests the `Manifestor Chart Generator` Backend Microservice to generate Kustomize manifest files.
* **Endpoint:** `https://orbit.k8or.com/kustomize/GenerateKustomizeUploadedImage.php`
```
kustomize/
├── src/
│   ├── routes/
│   │   └── GenerateKustomizeUploadedImage.php
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
  "PID": "c8fms03-c12bms09-e110",
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
  "PID": "c8fms03-c12bms09-e110",
  "status": "success",
  "message": "The Helm Chart was generated successfully.",
}
```

### 5.2. **`c12bms09-c20bre21-e115`:** The `Manifestor Chart Generator` Backend Microservice logs a `kustomization-initiated` event in the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `eventType` (String)
```json
{
  "helmChart": {
    "S": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
  },
  "helmChartData": {
    "L": [
      {
        "M": {
          "apiVersion": {
            "S": "apps/v1"
          },
          "kind": {
            "S": "Deployment"
          },
          "metadata": {
            "M": {
              "labels": {
                "M": {
                  "app": {
                    "S": "app"
                  },
                  "release": {
                    "S": "release"
                  }
                }
              },
              "name": {
                "S": "release-deployment"
              }
            }
          },
          "spec": {
            "M": {
              "replicas": {
                "N": "3"
              },
              "selector": {
                "M": {
                  "matchLabels": {
                    "M": {
                      "app": {
                        "S": "app"
                      }
                    }
                  }
                }
              },
              "template": {
                "M": {
                  "metadata": {
                    "M": {
                      "labels": {
                        "M": {
                          "app": {
                            "S": "app"
                          }
                        }
                      }
                    }
                  },
                  "spec": {
                    "M": {
                      "containers": {
                        "L": [
                          {
                            "M": {
                              "envFrom": {
                                "L": [
                                  {
                                    "M": {
                                      "configMapRef": {
                                        "M": {
                                          "name": {
                                            "S": "release-configmap"
                                          }
                                        }
                                      }
                                    }
                                  }
                                ]
                              },
                              "image": {
                                "S": "nginx:latest"
                              },
                              "name": {
                                "S": "container"
                              },
                              "ports": {
                                "L": [
                                  {
                                    "M": {
                                      "containerPort": {
                                        "N": "80"
                                      }
                                    }
                                  }
                                ]
                              }
                            }
                          }
                        ]
                      }
                    }
                  }
                }
              }
            }
          }
        }
      },
      {
        "M": {
          "apiVersion": {
            "S": "v1"
          },
          "kind": {
            "S": "Service"
          },
          "metadata": {
            "M": {
              "labels": {
                "M": {
                  "app": {
                    "S": "app"
                  },
                  "release": {
                    "S": "release"
                  }
                }
              },
              "name": {
                "S": "release-service"
              }
            }
          },
          "spec": {
            "M": {
              "ports": {
                "L": [
                  {
                    "M": {
                      "port": {
                        "N": "80"
                      },
                      "targetPort": {
                        "N": "80"
                      }
                    }
                  }
                ]
              },
              "selector": {
                "M": {
                  "app": {
                    "S": "app"
                  }
                }
              },
              "type": {
                "S": "LoadBalancer"
              }
            }
          }
        }
      },
      {
        "M": {
          "apiVersion": {
            "S": "v1"
          },
          "data": {
            "M": {
              "MY_CONFIG_VALUE": {
                "S": "Hello from ConfigMap!"
              }
            }
          },
          "kind": {
            "S": "ConfigMap"
          },
          "metadata": {
            "M": {
              "labels": {
                "M": {
                  "app": {
                    "S": "app"
                  },
                  "release": {
                    "S": "release"
                  }
                }
              },
              "name": {
                "S": "release-configmap"
              }
            }
          }
        }
      }
    ]
  }, 
  "createdTimestampUTC": {
    "S": "2025-02-18 08:49:02"
  },
  "createdTimestampUnix": {
    "S": "1739632730"
  }
}
```
* **Endpoint:** `https://orbit.k8or.com/kustomize/InitiateKustomizationUserEventLogChartStore.php`
```
kustomize/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── InitiateKustomizationUserEventLogChartStore.php 
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c12bms09-c20bre21-e115",
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
  },
  "eventMetadata": {
    "eventType": "kustomization-initiated",
    "timestamp": "timestamp"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c12bms09-c20bre21-e115",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 5.3. **`c12bms09-c20bre03-e120`:** The `Manifestor Chart Generator` Backend Microservice retrieves the base Kustomize manifest files from the `base-kustomize-1603-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `helmChart` (String)
    * **Sort key:** none
```json
{
  "helmChart": {
    "S": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
  },
  "helmChartData": {
    "L": [
      {
        "M": {
          "apiVersion": {
            "S": "apps/v1"
          },
          "kind": {
            "S": "Deployment"
          },
          "metadata": {
            "M": {
              "labels": {
                "M": {
                  "app": {
                    "S": "app"
                  },
                  "release": {
                    "S": "release"
                  }
                }
              },
              "name": {
                "S": "release-deployment"
              }
            }
          },
          "spec": {
            "M": {
              "replicas": {
                "N": "3"
              },
              "selector": {
                "M": {
                  "matchLabels": {
                    "M": {
                      "app": {
                        "S": "app"
                      }
                    }
                  }
                }
              },
              "template": {
                "M": {
                  "metadata": {
                    "M": {
                      "labels": {
                        "M": {
                          "app": {
                            "S": "app"
                          }
                        }
                      }
                    }
                  },
                  "spec": {
                    "M": {
                      "containers": {
                        "L": [
                          {
                            "M": {
                              "envFrom": {
                                "L": [
                                  {
                                    "M": {
                                      "configMapRef": {
                                        "M": {
                                          "name": {
                                            "S": "release-configmap"
                                          }
                                        }
                                      }
                                    }
                                  }
                                ]
                              },
                              "image": {
                                "S": "nginx:latest"
                              },
                              "name": {
                                "S": "container"
                              },
                              "ports": {
                                "L": [
                                  {
                                    "M": {
                                      "containerPort": {
                                        "N": "80"
                                      }
                                    }
                                  }
                                ]
                              }
                            }
                          }
                        ]
                      }
                    }
                  }
                }
              }
            }
          }
        }
      },
      {
        "M": {
          "apiVersion": {
            "S": "v1"
          },
          "kind": {
            "S": "Service"
          },
          "metadata": {
            "M": {
              "labels": {
                "M": {
                  "app": {
                    "S": "app"
                  },
                  "release": {
                    "S": "release"
                  }
                }
              },
              "name": {
                "S": "release-service"
              }
            }
          },
          "spec": {
            "M": {
              "ports": {
                "L": [
                  {
                    "M": {
                      "port": {
                        "N": "80"
                      },
                      "targetPort": {
                        "N": "80"
                      }
                    }
                  }
                ]
              },
              "selector": {
                "M": {
                  "app": {
                    "S": "app"
                  }
                }
              },
              "type": {
                "S": "LoadBalancer"
              }
            }
          }
        }
      },
      {
        "M": {
          "apiVersion": {
            "S": "v1"
          },
          "data": {
            "M": {
              "MY_CONFIG_VALUE": {
                "S": "Hello from ConfigMap!"
              }
            }
          },
          "kind": {
            "S": "ConfigMap"
          },
          "metadata": {
            "M": {
              "labels": {
                "M": {
                  "app": {
                    "S": "app"
                  },
                  "release": {
                    "S": "release"
                  }
                }
              },
              "name": {
                "S": "release-configmap"
              }
            }
          }
        }
      }
    ]
  }, 
  "createdTimestampUTC": {
    "S": "2025-02-18 08:49:02"
  },
  "createdTimestampUnix": {
    "S": "1739632730"
  }
}
```
* **Endpoint:** `https://orbit.k8or.com/kustomize/GetBaseKustomizeChartStore.php`
```
kustomize/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── GetBaseKustomizeChartStore.php 
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `GET`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** 
```json
{
  "PID": "c12bms09-c20bre03-e120",
  "helmChart": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5",
  "helmChartData": [
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

### 5.4. **`c12bms09-c20bre06-e125`:** The `Manifestor Chart Generator` Backend Microservice retrieves the user input (manifest) from the `user-input-manifest-1606-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `imageUID` (String)
    * **Sort key:** `timestamp` (String)
```json
{
  "imageUID": {
    "S": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
  },
  "timestamp": {
    "S": "timestamp"
  },
  "imageMetadata": {
    "M": {
      "icao": {
        "S": "British"
      },
      "tail": {
        "S": "G-AAAA"
      },
      "Part": {
        "S": "21117-1"
      },
      "imageName": {
        "S": "Trolls World Tour (2020)"
      },
      "imageVersion": {
        "S": "v0-0-01"
      },
      "imageCategory": {
        "S": "Movie"
      },
      "Description": {
        "S": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair."
      },
      "port": {
        "S": "80"
      },
      "envVars": {
        "L": [
          {
            "M": {
              "key": {
                "S": "VAR1"
              },
              "value": {
                "S": "value1"
              }
            }
          },
          {
            "M": {
              "key": {
                "S": "VAR2"
              },
              "value": {
                "S": "value2"
              }
            }
          }
        ]
      },
      "volumes": {
        "L": [
          {
            "M": {
              "key": {
                "S": "/host/path"
              },
              "value": {
                "S": "/container/path"
              }
            }
          }
        ]
      },
      "imageLabels": {
        "L": [
          {
            "M": {
              "key": {
                "S": "label1"
              },
              "value": {
                "S": "value1"
              }
            }
          }
        ]
      },
      "commandOverride": {
        "S": "/app/run.sh"
      },
      "entrypointOverride": {
        "S": "/app/entrypoint.sh"
      },
      "workingDir": {
        "S": "/app"
      },
      "image": {
        "S": "data:image/png;base64,iVBORw0KGgo"
      }
    }
  },
  "userMetadata": {
    "M": {
      "username": {
        "S": "jamesbond"
      },
      "firstName": {
        "S": "James"
      },
      "lastName": {
        "S": "Bond"
      },
      "email": {
        "S": "jamesbond@k8or.com"
      }
    }
  }, 
  "createdTimestampUTC": {
    "S": "2025-02-18 08:49:02"
  },
  "createdTimestampUnix": {
    "S": "1739632730"
  }
}
```
* **Endpoint:** `https://orbit.k8or.com/kustomize/GetUserInputDataChartStore.php`
```
kustomize/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── GetUserInputDataChartStore.php 
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
  "PID": "c12bms09-c20bre06-e125",
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c12bms09-c20bre06-e125",
  "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5",
  "configmap": {
    "apiVersion": "v1",
    "data": {
      "key1": "value1",
      "key2": "value2"
    },
    "kind": "ConfigMap",
    "metadata": {
      "name": "my-configmap",
      "namespace": "my-namespace"
    }
  },
  "deployment": {
    "apiVersion": "apps/v1",
    "kind": "Deployment",
    "metadata": {
      "name": "my-app",
      "namespace": "my-namespace"
    },
    "spec": {
      "replicas": 3,
      "selector": {
        "matchLabels": {
          "app": "my-app"
        }
      },
      "template": {
        "metadata": {
          "labels": {
            "app": "my-app"
          }
        },
        "spec": {
          "containers": [
            {
              "image": "my-app-image:latest",
              "name": "my-app-container",
              "ports": [
                {
                  "containerPort": 8080
                }
              ]
            }
          ]
        }
      }
    }
  },
  "service": {
    "apiVersion": "v1",
    "kind": "Service",
    "metadata": {
      "name": "my-service",
      "namespace": "my-namespace"
    },
    "spec": {
      "ports": [
        {
          "port": 80,
          "protocol": "TCP",
          "targetPort": 8080
        }
      ],
      "selector": {
        "app": "my-app"
      },
      "type": "LoadBalancer"
    }
  }
}
```

### 5.5. **`c12bms09-c20bre09-e130`:** The `Manifestor Chart Generator` Backend Microservice generates the Kustomize manifest files and saves them to the `k8or-kustomize-1609-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `imageUID` (String)
    * **Sort key:** `imageVersion` (String)
```json
{
  "imageUID": {
    "S": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
  },
  "imageVersion": {
    "S": "v0-0-01"
  },
  "helmChart": {
    "L": [
      {
        "M": {
          "apiVersion": {
            "S": "apps/v1"
          },
          "kind": {
            "S": "Deployment"
          },
          "metadata": {
            "M": {
              "labels": {
                "M": {
                  "app": {
                    "S": "app"
                  },
                  "release": {
                    "S": "release"
                  }
                }
              },
              "name": {
                "S": "release-deployment"
              }
            }
          },
          "spec": {
            "M": {
              "replicas": {
                "N": "3"
              },
              "selector": {
                "M": {
                  "matchLabels": {
                    "M": {
                      "app": {
                        "S": "app"
                      }
                    }
                  }
                }
              },
              "template": {
                "M": {
                  "metadata": {
                    "M": {
                      "labels": {
                        "M": {
                          "app": {
                            "S": "app"
                          }
                        }
                      }
                    }
                  },
                  "spec": {
                    "M": {
                      "containers": {
                        "L": [
                          {
                            "M": {
                              "envFrom": {
                                "L": [
                                  {
                                    "M": {
                                      "configMapRef": {
                                        "M": {
                                          "name": {
                                            "S": "release-configmap"
                                          }
                                        }
                                      }
                                    }
                                  }
                                ]
                              },
                              "image": {
                                "S": "nginx:latest"
                              },
                              "name": {
                                "S": "container"
                              },
                              "ports": {
                                "L": [
                                  {
                                    "M": {
                                      "containerPort": {
                                        "N": "80"
                                      }
                                    }
                                  }
                                ]
                              }
                            }
                          }
                        ]
                      }
                    }
                  }
                }
              }
            }
          }
        }
      },
      {
        "M": {
          "apiVersion": {
            "S": "v1"
          },
          "kind": {
            "S": "Service"
          },
          "metadata": {
            "M": {
              "labels": {
                "M": {
                  "app": {
                    "S": "app"
                  },
                  "release": {
                    "S": "release"
                  }
                }
              },
              "name": {
                "S": "release-service"
              }
            }
          },
          "spec": {
            "M": {
              "ports": {
                "L": [
                  {
                    "M": {
                      "port": {
                        "N": "80"
                      },
                      "targetPort": {
                        "N": "80"
                      }
                    }
                  }
                ]
              },
              "selector": {
                "M": {
                  "app": {
                    "S": "app"
                  }
                }
              },
              "type": {
                "S": "LoadBalancer"
              }
            }
          }
        }
      },
      {
        "M": {
          "apiVersion": {
            "S": "v1"
          },
          "kind": {
            "S": "ConfigMap"
          },
          "metadata": {
            "M": {
              "labels": {
                "M": {
                  "app": {
                    "S": "app"
                  },
                  "release": {
                    "S": "release"
                  }
                }
              },
              "name": {
                "S": "release-configmap"
              }
            }
          },
          "data": {
            "M": {
              "MY_CONFIG_VALUE": {
                "S": "Hello from ConfigMap!"
              }
            }
          }
        }
      }
    ]
  }, 
  "createdTimestampUTC": {
    "S": "2025-02-18 08:49:02"
  },
  "createdTimestampUnix": {
    "S": "1739632730"
  }
}
```
* **Endpoint:** `https://orbit.k8or.com/kustomize/SaveGeneratedKustomizeChartStore.php`
```
kustomize/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── SaveGeneratedKustomizeChartStore.php 
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
  "PID": "c12bms09-c20bre09-e130",
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
  "PID": "c12bms09-c20bre09-e130",
  "status": "success",
  "message": "The Helm Chart saved successfully.",
}
```

### 5.6. **`c12bms09-c20bre21-e135`:** The `Manifestor Chart Generator` Backend Microservice logs a `kustomization-saved` event in the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `eventType` (String)
```json
{
  "username": {
    "S": "jamesbond"
  },
  "eventType": {
    "S": "kustomization-saved"
  },
  "imageMetadata": {
    "M": {
      "imageUID": {
        "S": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
      },
      "icao": {
        "S": "British"
      },
      "tail": {
        "S": "G-AAAA"
      },
      "Part": {
        "S": "21117-1"
      },
      "imageName": {
        "S": "Trolls World Tour (2020)"
      },
      "imageVersion": {
        "S": "v0-0-01"
      },
      "imageCategory": {
        "S": "Movie"
      },
      "Description": {
        "S": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair."
      },
      "port": {
        "S": "80"
      },
      "envVars": {
        "L": [
          {
            "M": {
              "key": {
                "S": "VAR1"
              },
              "value": {
                "S": "value1"
              }
            }
          },
          {
            "M": {
              "key": {
                "S": "VAR2"
              },
              "value": {
                "S": "value2"
              }
            }
          }
        ]
      },
      "volumes": {
        "L": [
          {
            "M": {
              "key": {
                "S": "/host/path"
              },
              "value": {
                "S": "/container/path"
              }
            }
          }
        ]
      },
      "imageLabels": {
        "L": [
          {
            "M": {
              "key": {
                "S": "label1"
              },
              "value": {
                "S": "value1"
              }
            }
          }
        ]
      },
      "commandOverride": {
        "S": "/app/run.sh"
      },
      "entrypointOverride": {
        "S": "/app/entrypoint.sh"
      },
      "workingDir": {
        "S": "/app"
      },
      "image": {
        "S": "data:image/png;base64,iVBORw0KGgo"
      }
    }
  },
  "userMetadata": {
    "M": {
      "firstName": {
        "S": "James"
      },
      "lastName": {
        "S": "Bond"
      },
      "email": {
        "S": "jamesbond@k8or.com"
      }
    }
  },
  "eventMetadata": {
    "M": {
      "timestamp": {
        "S": "timestamp"
      }
    }
  }, 
  "createdTimestampUTC": {
    "S": "2025-02-18 08:49:02"
  },
  "createdTimestampUnix": {
    "S": "1739632730"
  }
}
```
* **Endpoint:** `https://orbit.k8or.com/kustomize/SavedKustomizeUserEventLogChartStore.php`
```
kustomize/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── SavedKustomizeUserEventLogChartStore.php 
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c12bms09-c20bre21-e135",
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
  },
  "eventMetadata": {
    "eventType": "kustomization-saved",
    "timestamp": "timestamp"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c12bms09-c20bre21-e135",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 5.7. **`c12bms09-c20bre24-EEE`:** The `Manifestor Chart Generator` Backend Microservice writes error logs to the `user-error-log-1624-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `errorType` (String)
* **Endpoint:** `https://orbit.k8or.com/kustomize/GetUserDataViewPort.php`
```
kustomize/
├── src/
│   ├── routes/
│   │   └── ErrorLog.php
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

---

## 6. Image Upload to ViewPort (After Form Submission):

*Finally*, *after* the chart generation, the frontend triggers the saving of the validated image to ViewPort via the **Portal Upload Logic** Backend Microservice, using the `/upload/UploadImage.php` endpoint.

### 6.1. **`c8fms03-c8bms06-e140`:** The `Portal Upload Board` Frontend Microservice requests the `Portal Upload Logic` Backend Microservice to save image details to `ViewPort`.
* **Endpoint:** `https://orbit.k8or.com/upload/UploadImage.php`
```
upload/
├── src/
│   ├── routes/
│   │   └── UploadImage.php
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
  "PID": "c8fms03-c8bms06-e140",
  "userMetadata": {
    "username": "jamesbond"
  },
  "imageMetadata": {
    "imageUID": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5",
    "imageName": "Trolls World Tour (2020) new",
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
    "PID": "c8fms03-c8bms06-e140",
    "postData": {
      "post_title": "Trolls World Tour (2020) new",
      "post_content": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
      "post_excerpt": "Trolls World Tour (2020)",
      "post_status": "publish",
      "post_author": 2, 
      "post_date": "2024-11-03 15:30:00", 
      "post_date_gmt": "2024-11-03 12:30:00", 
      "post_modified": "2024-11-03 15:30:00", 
      "post_modified_gmt": "2024-11-03 12:30:00", 
      "post_type": "post",
      "post_parent": 0,
      "menu_order": 0, 
      "comment_status": "closed", 
      "ping_status": "closed", 
      "post_password": "",
      "to_ping": "", 
      "pinged": "", 
      "post_content_filtered": "", 
      "post_mime_type": "", 
      "comment_count": 0,
      "category_name": "Music",
      "image_data": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAboAAACwCAYAAAB5G6tXAAAcl0lEQVR4Xu2debQV1ZXGvyIIUaOJEW2k09iIUyPEeQDbMeCIQqMGCYoTKEqIshygFZVERWlDCBhU8lSmyCBqUCEKEjUm4hwTiRoHQhza2B2jaY1JTIy7Vz28ct97975bdWqfoao+1uKvd84++/z2rv3dU8M5EfiPBEiABEiABApMICrw3Dg1EiABEiABEgCFjklAAiRAAiRQaAIUukKHl5MjARIgARKg0DEHSIAESIAECk2AQlfo8HJyJEACJEACFDrmAAmQAAmQQKEJUOgKHV5OjgRIgARIgELHHCABEiABEig0AQpdocPLyZEACZAACVDomAMkQAIkQAKFJkChK3R4OTkSIAESIAEKHXOABEiABEig0AQodIUOLydHAiRAAiRAoWMOkAAJkAAJFJoAha7Q4eXkSIAESIAEKHTMARIgARIggUIToNAVOrycHAmQAAmQQFBCJ/17SFAhOXggoonXBcUoKD6GzjDOhuDYjQRIwIhAUEWcBdAohrnrxDjnLmR0mARyTYBC1174uKKzktwUOitYaZQESKAOAQodhc75xUGhc46cA5JAqQlQ6Ch0zi8ACp1z5ByQBEpNgEJHoXN+AVDonCPngCRQagIUOgqd8wuAQuccOQckgVIToNBR6JxfABQ658g5IAmUmgCFjkLn/AKg0DlHzgFJoNQEKHQUOucXAIXOOXIOSAKlJkCho9A5vwAodM6Rc0ASKDUBCh2FzvkFQKFzjpwDkkCpCVDoKHTOLwAKnXPkHJAESk2AQkehc34BUOicI+eAJFBqAhQ6Cp3zC4BC5xw5BySBUhOg0FHonF8AFDrnyDkgCZSaQDGF7gtbArv3AzpulCm40fipQfHJNJmAOsuU87OfO/jsE8D/vKEzK55SocORVkggUAJBFXK1X/q77Ilo+u1BzS3Q+OfWLblyrOChZTr+U+h0ONIKCQRKICgxoNAFmiUBukWhCzAodIkEAiVAoQs0MHSrfQIUOmYICZBAUgIUuqSk2C4oAhS6oMJBZ0ggaAIUuqDDQ+fqEaDQMTdIgASSEqDQJSXFdkERoNAFFQ46QwJBE6DQBR0eOscVHXOABEggKwEKXVaC7O+FAFd0XrBzUBLIJQEKXS7DRqdDFbphIyX7x/CfhHfzzYBZ06KgrlFmHgnkkUBQFxG/o8tjCvnxOVSh69NPT+i6dQVW3Emh85NhHLVIBCh0RYpmieZCoStRsDlVEshIgEKXEaCL7nL5WYK1LwDvvg18+JfGQ3beGNj8C0CPnZr/R6MmBBXnxhNo3IJC15gRW5AACawnEFQB5K1LQGZ+S7B6JfC/vwPkY908/dzmwK59EX3zxqDibjJJCp0JNfYhgXISCKrglVno5IS9pXnF5urfRp2A/Q9DNPG6oHIg6fQpdElJsR0JkEBQRa5sQifTLhasvAP4+9/8ZmJ8e7PpvqByoREQCl0jQvw7CZBAhUBQxa1MQidH95JEz9tc5WrUAdixN6KZdwWVE/WmT6FzlRgchwTyTyCoolYGoZNTDxW8sS7czOm8MaLlzweVF7VgUejCTSF6RgKhEQiqoBVd6IJbxbWXjfsdiujKm4PKj2p3KXShlRL6QwLhEgiqkBVZ6OSw7QUf/yPcTKjlWfftEd1yf1A5UnGTQpevVKK3JOCTQFBFrKhCJwN6ivqnAq6yptu2iOY9FFSexFOn0LlKAI5DAvknEFQBK6LQ5VrkKvkdoNhR6PJffDgDEnBFgEJnkbQM+rLgg/ctjuDQdJ99EE1bHEy+UOgcxp5DkUDOCQRTuJpvR/XvobPz+y57Ipp+u9e5yekDBK+9kvP0aOX+CaMQnXWxV658RleslOJsSMAFgSCK1qfFqyBCJ7MmC5Y0uYif2zE26oTo3heDyBmu6NyGnqORQJ4JBFG0Cid0Q/YQvPdunvOivu+99kA04w7veUOhK2Z6cVYkYIOA94JVPaki3LqUqRME9y62EaswbEYdEN2/1nveUOjCSAd6QQJ5IOC9YBVO6Ib1Ffz+rTzE3tzHHfsguv5ur7lDoTMPH3uSQNkIeC1WrWEXYkWn9ZyxViZ2+AxwwBGILv1e3bjJhcMFv3wcVj9O79gR0X0ve80dCl3ZShXnSwLmBLwWq6IJnUwcKXjsx+bRaK9nyhMGZNQRgnUv2vEltvrvhyOa5O9cOwqdvdDSMgkUjQCFTjGictKBgrdeV7T4iSnDD7Zl3FDBmif0/YktbtUV0cJHveUPhS55WJvmirzzR+C5F4C/f7Sh3xafB3beEdhqS2DY8ZG3WCafiXlLMjBnZ6vnzCZp/pxszCj7uRdUcuf91qWVD8Q/uwmiZc8Zx0lGHCx481X9XPX8UgqFrn5IL5go8uiTwHsGexV02RI4sj9w0bn2i49+Um6w+I3xIqsfBz40OOoxC4M+/dYXb41/3boCK+40i8PXRoqseV7Di/U2Tj8JGHeOmS9x/0uvEnng4do5mdV2klkaF9AkxtO2yb3Q2Xg+t1tfRN9ekClOcsQOgo+qfsqnDUy99kcPQzRucibfTF2h0LUkN2u2yLxFZuJWLwZxwT95aFzkzAucaXxN+l0+WWT5SjNxqzfeDj2BO+cnnz+FbgPJGbNEFt/ZOCcpdCbZHvfxtDOKmlBXzTtatS6zkMg5xwpeWmNKs34/j9uCUeg2hOWQY0Te/oN+eCsWO3cCzhgBnH168oJvz5u2ls+dsH61YPNfUsGj0AHTbxCZvzj5Dw4KnWnmUujakLMhwti6G6IFj2QWYpMwU+iAM8aKPPG0CT2zPvEK78F7whG7+LnbrNnJC6rZrDf06tgRGDoEmHBefQZlFzqTH10UOtPM9CB0cvU4wY+Xmnpct5/Gii42Lsf2Efz5T7r+bboZoruepdBVUXVV6EwKilbwvzoYuPQiv4I3bKTIrxSfQaVhs8+ewM3X1Z6/q/g38tf1M7qLrxC5595GXtX+O4XOjJuXW5eqKwzlW5fNQmfjjVCP39Op8j54IKKJ16kItotCt//hIiYvmpheTrX69e4FLLzJj9iFMP+ePYClt7adv4v4J4mjS6EbPFxk7bokXlHomgmo3V7zsaKbdrFg+ULzaNfr6fGFD/3J6Fksq9CFUOQrUdz2X4Bli92K3e4Hith4r8okM2uJXdmETuPOAld0JtkX9/EgdKpCXT3vL/VANOcBldWGKc4Q+5VR6AYOFXnVwmeaWeLrUuxCErkKs9a3McskdFo/uih0pldgkYTO8/dqpiGw3a9sQnfWeSKrLX37nzVWLsROq6hmnWut/qNP2/DRc1mETjMeFDrTrCyS0MUMPL70YRoC2/3KJHS3LhG5Zpptotns99sHmPVdO7cxDx8i8mbA+6RvvhnwyIr1cy+D0GnfWaDQmV57voRu0JcFHxhsR5FknhS7FpTKJHRZH/YnSS+NNtUrGw17sY3Txog89YyWNXt2Dj0QmH5NFBVd6Gx80kKhM81LX0Jn483Gagbx6QXHnoTo65NK/8yuDELXdWtgux5AvJVVHv5tugnw2Cq9VV28F+KNs/MwcyD+xu4/BgJLFL8wCmkLsHiHnDi+NuJBoTPNcV9CZ/P0gmoWHj/UNg2Jdr8yCJ02Mxf22vvGLO34ms+B0o4dQvuQhC725U8fNN7Oy4Qbhc6EWtzHk9DFQ6t9IpFk7lt0QbTkyVKu7ih0SRLET5sJ44DhJ2Rb2bnY1ssPneSjhiR0yb1O35JCl57Z+h4+hW7IHoL33jX13KxffEtz130RXXtraUSPQmeWKi561fuYOs3YIX5KkMZ/jbYUOg2K620EVRjVVkM+hW7SaMHPVuhFKK2lLboAXxmMaPQlQcU27TQatafQNSLk9+9jzwTOPNVsVRfypxQuqVLo9GgHVQyLIHTNty8H7iL465/1omRiKeoAdOuOaO6DQcXYZCq1+lDotEjasbPLzsCiW8yEjqu59TGh0OnlZlBFsDBCd+2FghW360Upq6VY9LbvheiGe4KKd5ZpUeg20IuP0em3LzBjSn1h+e4NIj99FHjplSzU0/Vdszq90F02WeSHy9KNY9o65rbnbsABfYGThtb2tWmeyKoHgedfNB3FvB+Fzpxd655BFb6iCF3zqs7Wyd5ZY79RJ2D/w9Q2Mc7qjml/Ch2Q9Iy01oy/M1NkwRL7x9v0PwiYdnU6sXPxcXj3LwHLb0vnV8zQxuG27eU/hc60OrTtR6HTY9nGkhy5k+Dvf7M4QkbT23RHNP8nQeVA0hmVWejilciF34jPRktfrKv5jrlA5OHVSYmnb7dVF+CBu5P7uOgOkaumph8naY/4W7ezTgVGZzxANsuRNEl9jdtR6NLQar9tUEWuSCu6CnYZ0FMgH+tFzIalTT6H6O41QeVCo2mWVeiyFL9aTKd+T2TOgka0zf9+yfnAicclE7vxk0R+tNJ8rPZ6Vm/TpTFCLMpTpgM2T1LIEmvtY3o0mNWzwc8LTOl6fOuylsu5ELvY8fi25mHHIRo3OXjRK6PQ2do82eYOJMccCUy+NJnQDfqayG9+a3rR1++nLXLVI9l8cSavQvfP2wB77AbstD1wyrBksdePekuLQRW0Iq7ocrWyqzjbeWNEy58PKjdaXwhlEzqbxTpma+t23M47AEvmJit2mvtEVvIlvl35zMPJxjcptjZ/JORJ6PbYFZh7gz3OJrGp7hNUMSuy0MXQg39m1zqbeuyEqOm+oHLk0x8OV44VPKT0el6gJ4xXh+Pc0cDIEXYLifau9LH/Sfe/bJorMmNW1nLWtv+go4ArJ9rlZmsXlzwIXWUza/3I6VoMqogVXeiaxe7kgwS/e003ijatBXpqQplWdPEr8HOut1us4xSydRxQks8MbKwo074Mk+Uy2q+/yAfKn86GLHRdtgQevMd+TmaJCVd0WvQM7UjTNYKl84AP/2JowXG3Dp9BtPKVsH4UlWhFl+aFjqyZMeRkkZfXZrXSsn+Slw1GjBZ55lndcePTBL51sZtibOP7v1CFztazYt3ot7QWVvHq30NUJhvYyyj15iRTxwtWLUXQnyBUnA/spPOyrOhcF5VrZ4jMW6RyFX5q5KjDgCmT2heco04Qef2/dcdNspLUHFH7GWOIQmf7WbFmPLiis0XT0K7M/JZgxRLgz38ytOCo20adEN37YhA/jsoidMcdC0ya4GZVUski7YLdd2/g+9Pbn8NBR4u8o7gXepqXYLSuHu23RkMUujNPBcae6TYfNeITRNGqTKQMz+gaBS34Z3iBiF1ZhM71qiTOT+3VVZ9ewIKb2i+O2uKa5rOGRtdk0r9rfwcYmtC5vruQlHuSdhS6JJQ8tGku5I+sDPO2ZrdtEc17yGvulEHobL8aXy+tzzxX5NEn9ZI+ScHWFjofK4/rvi/y/TluudUbzcYH4yNOjHfkyd9qLmbktVi1DhJXdLXTVsYMErz8HPDxP/SuoqyWDjkG0SUzvOVPGYQuiUBkDWOt/j5WJtpC52MlHLPUnEeW+GsLna8fXVr57a1Q1ZoAha5xWGXUEYLfvgzv24p5voVZBqHLctRN40yq32La9SK3/CCLhZZ9kxRsTYGIR6fQiax5Xi+G2/0rcNeCfK7muKLTywMvluTEvoK33/IydvOgu/VF9O0FXn4slUHokjzbshF8baFLIjwUuraRTPIDwdWty0MOaP8YKBt5qGnTS5GqNwGu6MxC6+153mc3QbTsOS85RKEzy5UkvSh0SSjVbqMp2CEJnY+3f82j0LanlyJFodMMYUtbzt/aPHqYl02gKXT2cohCZ862qEKX5KN/c2r2e1Lo7DP2MoIzwdt+F0Q3LnOeR2UQOj6jM790+IxO9xkdhc48F9v05K1LRZifmLK+kbSnvTDLIHRZbl1lySTtfSeTzENzJZTkmWAWPu311ZxHEm6untFR6BQzhkKnCLPKlBy/l+CPf7BjPP5GZdU6rug+oatZ6HwVbH5HZ3apFPk7OgqdWU7U7EWhU4TZypQM2UPwnuIeS1X2KXQbYBRB6LS3skry9qg2N+6MwluX1SXQ+S/x9ko5hc6e0MWWrd3GVDzPLSmBMty6jFm43IG/wl5bdLjXZdKsbtmOty7NuNXqRaHTYxm8Jbnga4JfPKrvZ599EE1b7DSXyiJ0rvcXnH6DyE3zdVMkyepKexXpYycP7R8IFDq9PHRanBq5nfcVnUw5X+eYoerbguOnqsZIjXF1MD0ci1QWoYsxuzyP7sTTRZ77daMrNd3fkzzfsXEe3dAhwMQL3OzmMe4/RVb9JB2XRq0pdI0IJf+7ahFNPmztlmpF2EPhbb41OKCnqG/NpfydmhzbR9SPA/LAu0xCt/OOwJI59gv2ojtErpqa9Spu2z/Jq/6TrhG5427dsXnCuB7PJD9W9EbTt0ShU2Qqg74s+OB9RYsAlEUkDz4mAVgmoYt5nDMSOPt0u2I3eLjI2nVJ6Cdvs+kmwGOrGvvdNFdkxqzkdpO2PGEwcNlFjcdPaq9Wu7POE1n9RBYLtftyRafHlEKnxxIyrK/g98p7T37+i4jueFotThS6GgFXfJlG+zlNxVvbJztrfztX8TvNAag22Nl+VjezSeTG2YpFpMoUhU6Pq1oB1XAp97cuxw4RvPCMBooWNjRf35ej/03w4V91feTLKJ/ytFGsK8ZtvZhis1in2SNR+4UUFz8Sdj9Q5KOPdC+nijUKnR5XCp0eS8i0iwXLFypa/MRU9+0R3XK/SqzUfkxUz1JxRZQUXtluXWoUv1psr79ZpGkuYKtYJ3k+V/FL+xy86vl22RJ48B69W5jx88wp0+1xi32n0CWtBo3bqRTPxsMka6FWhJWfayXzfn0rtTm0HnTgcETnXZkpXnLOsYKX1qSZTqK2mivORAPGnOMT2B9alrR5++0Uhdrmiq4yic6d4pOegaFDshXu8yeKrHxAB2EtKyYvg9jkF3M76zRg1CnZuF3xXyK3LbXHTeNHjfbBq3wZRTHeaiLhU+iO21Pwf+8oUvnElMJBp1beCu3YEdF9L2cSYBNYZRa6Cq8degJ3zk9ftGfNFpm3CHhP+b2p1nHsfxAw7ep0/h0+RORN5cfcrf3q/iVg+W3p/IptzFkgMnch8La93fRauMoVnUllqN3HeYFqz/VCCN34kwVP/0wvQtWWMoidlZdQYt++uDWi2x53nkcUug2JEb+ocmA/4OrL6xfvHywWefCnwM+ftXu7rTpd09y2rPS7bLLID5UW6o0uwniF12/f9g8U9cGNK7pGkUv/d+cFquhCF8/PysqpGtwOvRHdcE/i2Fnb+iv2ab+vILrypsS+pE/R2j0odFok7djJcsSQzRc87MzWjlWu6PS4Oi9QpRC60wcIXntFL0r1LHXpChxwJKIxl7WJo1w+WvDUw8CHf7HnR9QB0f1rveQQhc5eWDUsnzsaGDki/e3BeGxb36VpzMulDQqdHm0vRaqe+0W4dVmZm/VVnV4OmFvq1h3RvJ94ySEKnXnYbPfs2QNYequZyFV846qOb11q5qmXIlUKoTv7GMHLv9KMVXC2fLxt+ekPiRK/dRlcIrRyaMI4YPgJ2YTu3AkiDzwc+kzt+scVnR5fCp0eyzaWrD4bs+h3ItNbdUW08FFv+cMVXaIoOW+0527AnOuziVzF6f0PF7H9ZqhzQCkGpNClgNWgqbdCVcuvIt26jOcn114oWHG7XrRCseTx2VyZVnRdtwa26wGsfjyUwLfvR9J9LZPOxsaRQUnHTtsu3mrssEOBH61M27N+ewqdHksKnR7LmpZk3FDBGgs7vlr2u13zu+6HaOpCr7lThhVdpdANHCry6us+A55s7NGnAWNG6azmKiOeNkbkKf1d9ZJNKEWrQw8Epl8TRZofvFPoUgSAKzo9WKaWZMTBgjdfNe0eVr8t/wnR4se8ilzzarkEz+gqhe6meSLTbwwrDVp7s9fuwOyZuiJXGSP0W5jVK1kKXZh56r1gVWMp2q3LFnMrgthtuhmiu54NImfKJHRxHp0xVuSJp8MsIrY2m66ebchvYVavZCl0YeZoEEWrgqbIQte8CnH1fZ2NXMuwK4sNd8omdDHDEG9huhC5Sv6EKHatV7IUOhtXe3abFLrsDFNZkIlnCB6zuJNuKm8SNg5oJffpj6IS3bqsjlJIt/GyPENKmHktmrk4MSCNX7VEnkKXhqC7thQ6d6xbjCRH9xKru5ZozWub7ojm+/kovL0plHFFV+Gx18EiH/5NK8Bmdnr3AhbeZOeZXCOPQhD7eitZCl2j6Pn5O4XOD/fmUeXrgwW//qVHD9oZOuoAHHQUoonXBZUjZV/RVeZ/yDEirnbRb50lxxwJTL7Uj8hVfBk8XGTtOj+XTnsiT6HzE5NGowZVxIr+jK5eMOTUQwVveLpqazkV6Cqu2tUyr+gqHE45W+TnDn8naR9e2qg4Nfr7Nd8VWXynu9MYYn++Ohi49KL6Ik+haxQ1P3+n0PnhXnNUibcNe+V5QD7249UWXRAteTKonKj746Ckz+ha82iaKzJrNmDzVmZ8nM3xg4EJ5/ldxdXLBRdvpCY9w45C56d0NRo1qKJW1hVd6yDJjVcJfrwUePftRvHL/vcOnwF674XoO4uCyoVGE+OKriWheBeR+Yt1BS8+5+74QcC4c8IUuNY5Eu+P+fBq3RVeUoGr+EKha3Tl+vl7UMWNQlc7CZp3V3lpjd6RO5/bHOi9t5dz5LTSnEJXn2R8zM3TvzATvVjc4l0+rrgkH+JWd5X3dRHTQ2ZjBgMOASZNSM+AQqd1hevaodDp8nRmTWZ+U/Deu8DaF4AP3q89bqfOwA69gfjzgHGTg4p1VlChCl3WednoP7NJ5JXfAG/9vq31HtsCPboDo05JX9Rt+GrLZnsMeu0ExPuKFp2BLbZ5sBtU8eOKLg8pE4aPFLow4kAvSCAPBCh0eYgSfWxDgELHpCABEkhKgEKXlBTbBUWAQhdUOOgMCQRNgEIXdHjoXD0CFDrmBgmQQFICFLqkpNguKAIUuqDCQWdIIGgCxRQ6LeQHDwx2CyytKfqwo/bSkZbzjLMWSdohgSAJUOjaCwsLoJWkpdBZwUqjJEACdQhQ6Ch0zi8OCp1z5ByQBEpNgEJHoXN+AVDonCPngCRQagIUOgqd8wuAQuccOQckgVIToNBR6JxfABQ658g5IAmUmgCFjkLn/AKg0DlHzgFJoNQEKHQUOucXAIXOOXIOSAKlJkCho9A5vwAodM6Rc0ASKDUBCh2FzvkFQKFzjpwDkkCpCVDoKHTOLwAKnXPkHJAESk2AQkehc34BUOicI+eAJFBqAhQ6Cp3zC4BC5xw5BySBUhOg0FHonF8AFDrnyDkgCZSaAIWOQuf8AqDQOUfOAUmg1AQodBQ65xcAhc45cg5IAqUmEJbQTTlfQopGNH5qUHxCYpPFF2Gcs+BjXxIggZQEWMhTAmNzEiABEiCBfBGg0OUrXvSWBEiABEggJQEKXUpgbE4CJEACJJAvAhS6fMWL3pIACZAACaQkQKFLCYzNSYAESIAE8kWAQpeveNFbEiABEiCBlAQodCmBsTkJkAAJkEC+CFDo8hUveksCJEACJJCSAIUuJTA2JwESIAESyBcBCl2+4kVvSYAESIAEUhKg0KUExuYkQAIkQAL5IkChy1e86C0JkAAJkEBKAhS6lMDYnARIgARIIF8EKHT5ihe9JQESIAESSEmAQpcSGJuTAAmQAAnkiwCFLl/xorckQAIkQAIpCVDoUgJjcxIgARIggXwRoNDlK170lgRIgARIICUBCl1KYGxOAiRAAiSQLwIUunzFi96SAAmQAAmkJEChSwmMzUmABEiABPJF4P8BvsIXkpXRlKoAAAAASUVORK5CYII="
    },
    "post_meta": [
      {
        "meta_key": "version",
        "meta_value": "v0-0-01"
      },
      {
        "meta_key": "scanned",
        "meta_value": "No"
      },
      {
        "meta_key": "part",
        "meta_value": "4562"
      },
      {
        "meta_key": "tail",
        "meta_value": "41-38116"
      },
      {
        "meta_key": "image_status",
        "meta_value": "Uploaded"
      },
      {
        "meta_key": "imageuid",
        "meta_value": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
      },
      {
        "meta_key": "environment",
        "meta_value": "Testing"
      },
      {
        "meta_key": "start_date",
        "meta_value": "20250207"
      }
    ]
  }
}
```
* **Response payload:** 
```json
{
  "PID": "c8fms03-c8bms06-e140",
  "status": "success",
  "message": "The Image was uploaded successfully.",
}
```

### 6.1. c8bms06-c32bre03-e145: Generate Post URL (Slug):
* **Endpoint:** `https://orbit.k8or.com/transfer/GenerateImageURLViewPort.php`
```
upload/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── GenerateImageURLViewPort.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms06-c32bre03-e145",
  "post_title": "Trolls World Tour (2020)",  // Used for slug generation
  "version": "v0-0-01",
  "environment": "Development",
  "username": "jamesbond"
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms06-c32bre03-e145",
  "success": true,
  "post_name": "trolls-world-tour-2020-v0-0-01-development" // The generated slug
}
```

### 6.2. c8bms06-c32bre03-e150: Assign GUID:
* **MySQL Table Schema:**
```sql
-- `app_image_mql_8606-dtb_k8d_aws`.wp_image_guid definition

CREATE TABLE `wp_image_guid` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `image_guid` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `related_object_type` varchar(255) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  `related_object_id` bigint(20) unsigned DEFAULT NULL,
  `created_at` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NULL DEFAULT NULL,
  `other_data` text COLLATE utf8mb4_unicode_ci,
  PRIMARY KEY (`id`),
  UNIQUE KEY `guid_index` (`image_guid`),
  KEY `related_object_index` (`related_object_type`,`related_object_id`)
) ENGINE=InnoDB AUTO_INCREMENT=501 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```
* **CSV Structure:**
```csv
"id","image_guid","related_object_type","related_object_id","created_at","updated_at","other_data"
1,c842fb05-adcf-40c8-b53a-9b3ea0705e55,,,"2025-02-17 13:17:36",,
2,"06a6d12a-2701-47bd-8167-f83fcc4ebe5c",,,"2025-02-17 13:17:36",,
3,f5abcbae-1e38-473a-9028-d49b71993c50,,,"2025-02-17 13:17:36",,
4,"157d4358-016b-46a5-827f-188f1ae55c5f",,,"2025-02-17 13:17:36",,
5,"40b780c4-e295-44fb-92e5-661ec1f51829",,,"2025-02-17 13:17:36",,
6,c682850c-c030-40ba-91fb-2a61f9f1af30,,,"2025-02-17 13:17:36",,
7,a803fc13-7634-4551-b912-def296dfc6c6,,,"2025-02-17 13:17:36",,
8,d00d3a94-aa6e-447e-9b99-626515ed6e35,,,"2025-02-17 13:17:36",,
9,cd92d0c4-3877-4132-8604-e537af50efb7,,,"2025-02-17 13:17:36",,
10,bfa6ea08-12a6-4ad4-ba77-325e8d644305,,,"2025-02-17 13:17:36",,
11,e8be71f7-9cf9-47c9-b9d9-20f0673bff51,,,"2025-02-17 13:17:36",,
12,"7e2ec94f-0821-4e08-b4e2-4b2566110ed0",,,"2025-02-17 13:17:36",,
13,fc8b582c-e653-44f8-8182-da987ddd457a,,,"2025-02-17 13:17:36",,
14,"2bc42d8d-735a-483e-b936-8f0645eaa9ac",,,"2025-02-17 13:17:36",,
```
* **Endpoint:** `https://orbit.k8or.com/transfer/AssignImageGUIDViewPort.php`
```
upload/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── AssignImageGUIDViewPort.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms06-c32bre03-e150",
  "post_id": 123,  // ID of the image (post),
  "username": "jamesbond"
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms06-c32bre03-e150",
  "success": true,
  "guid": "a1b2c3d4-e5f6-7890-1234-567890abcdef" // The retrieved GUID
}
```

### 6.3. c8bms06-c32bre03-e155: Insert Post:
* **Endpoint:** `https://orbit.k8or.com/transfer/InsertPostViewPort.php`
```
upload/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── InsertPostViewPort.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms06-c32bre03-e155",
  "username": "jamesbond",
  "post_data": {
    "post_title": "Trolls World Tour (2020)",
    "post_content": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair.",
    "post_excerpt": "Trolls World Tour (2020)",
    "post_status": "publish", //  "publish", "draft", etc.
    "post_author": 2, // *Uploading by* author ID
    "post_date": "2024-11-03 15:30:00", // Current time or scheduled time (local) - Update dynamically
    "post_date_gmt": "2024-11-03 12:30:00", // GMT equivalent of post_date - Update dynamically
    "post_modified": "2024-11-03 15:30:00", // Current time or scheduled time (local) - Update dynamically
    "post_modified_gmt": "2024-11-03 12:30:00", // GMT equivalent of post_modified - Update dynamically
    "post_type": "post",
    "post_parent": 0,
    "menu_order": 0, // Set as needed
    "comment_status": "closed", // Set as needed
    "ping_status": "closed", // Set as needed
    "post_password": "",
    "post_name": "trolls-world-tour-2020-v0-0-01-development", // *GENERATE UNIQUE SLUG DYNAMICALLY*; "post_name" should be generated by taking the entered post title, converting spaces to hyphens, and adding a version number first and environment name second to create a unique and descriptive "post_name"
    "guid": "http://orbit.k8or.com/viewport/?guid=fedcba98-7654-3210-abcdef-0123456789ac",
    "to_ping": "", // Leave empty
    "pinged": "", // Leave empty
    "post_content_filtered": "", // Leave empty
    "post_mime_type": "", // Leave empty
    "comment_count": 0 // 0 for a new post
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms06-c32bre03-e155",
  "success": true,
  "post_id": 1234 // The ID of the newly inserted post
}
```

### 6.4. c8bms06-c32bre03-e160: Insert Post Meta:
* **Endpoint:** `https://orbit.k8or.com/transfer/InsertPostMetadataViewPort.php`
```
upload/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── InsertPostMetadataViewPort.php
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms06-c32bre03-e160",
  "username": "jamesbond",
  "post_id": 456,
  "post_meta": [
    { "meta_key": "preview", "meta_value": "http://orbit.k8or.com/viewport/trolls-world-tour-2020-v0-0-01-development/" },
    { "meta_key": "website", "meta_value": "http://orbit.k8or.com/" },
    { "meta_key": "website-main-article", "meta_value": "http://orbit.k8or.com/viewport/trolls-world-tour-2020-v0-0-01-development/" },
    { "meta_key": "version", "meta_value": "v0-0-01" },
    { "meta_key": "scanned", "meta_value": "No" },
    { "meta_key": "part", "meta_value": "4562" },
    { "meta_key": "tail", "meta_value": "41-38116" },
    { "meta_key": "image_status", "meta_value": "Uploaded" },
    { "meta_key": "imageuid", "meta_value": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5" },
    { "meta_key": "environment", "meta_value": "Development" }
  ]
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms06-c32bre03-e160",
  "success": true,
  "message": "Post meta inserted successfully"
}
```

### 6.5. **`c8bms06-c20bre21-e165`:** The `Portal Upload Logic` Backend Microservice logs a `viewport-created` event in the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `eventType` (String)
```json
{
  "username": {
    "S": "jamesbond"
  },
  "eventType": {
    "S": "viewport-created"
  },
  "imageMetadata": {
    "M": {
      "imageUID": {
        "S": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
      },
      "icao": {
        "S": "British"
      },
      "tail": {
        "S": "G-AAAA"
      },
      "Part": {
        "S": "21117-1"
      },
      "imageName": {
        "S": "Trolls World Tour (2020)"
      },
      "imageVersion": {
        "S": "v0-0-01"
      },
      "imageCategory": {
        "S": "Movie"
      },
      "Description": {
        "S": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair."
      },
      "port": {
        "S": "80"
      },
      "envVars": {
        "L": [
          {
            "M": {
              "key": {
                "S": "VAR1"
              },
              "value": {
                "S": "value1"
              }
            }
          },
          {
            "M": {
              "key": {
                "S": "VAR2"
              },
              "value": {
                "S": "value2"
              }
            }
          }
        ]
      },
      "volumes": {
        "L": [
          {
            "M": {
              "key": {
                "S": "/host/path"
              },
              "value": {
                "S": "/container/path"
              }
            }
          }
        ]
      },
      "imageLabels": {
        "L": [
          {
            "M": {
              "key": {
                "S": "label1"
              },
              "value": {
                "S": "value1"
              }
            }
          }
        ]
      },
      "commandOverride": {
        "S": "/app/run.sh"
      },
      "entrypointOverride": {
        "S": "/app/entrypoint.sh"
      },
      "workingDir": {
        "S": "/app"
      },
      "image": {
        "S": "data:image/png;base64,iVBORw0KGgo"
      }
    }
  },
  "userMetadata": {
    "M": {
      "firstName": {
        "S": "James"
      },
      "lastName": {
        "S": "Bond"
      },
      "email": {
        "S": "jamesbond@k8or.com"
      }
    }
  },
  "eventMetadata": {
    "M": {
      "timestamp": {
        "S": "timestamp"
      }
    }
  }, 
  "createdTimestampUTC": {
    "S": "2025-02-18 08:49:02"
  },
  "createdTimestampUnix": {
    "S": "1739632730"
  }
}
```
* **Endpoint:** `https://orbit.k8or.com/upload/ViewPortCreatedUserEventLogChartStore.php`
```
upload/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── ViewPortCreatedUserEventLogChartStore.php 
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms06-c20bre21-e165",
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
  },
  "eventMetadata": {
    "eventType": "viewport-created",
    "timestamp": "timestamp"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms06-c20bre21-e165",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 6.6. **`c8bms06-c20bre21-e170`:** The `Portal Upload Logic` Backend Microservice logs an `image-uploaded` event in the `user-event-log-1621-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `eventType` (String)
```json
{
  "username": {
    "S": "jamesbond"
  },
  "eventType": {
    "S": "image-uploaded"
  },
  "imageMetadata": {
    "M": {
      "imageUID": {
        "S": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
      },
      "icao": {
        "S": "British"
      },
      "tail": {
        "S": "G-AAAA"
      },
      "Part": {
        "S": "21117-1"
      },
      "imageName": {
        "S": "Trolls World Tour (2020)"
      },
      "imageVersion": {
        "S": "v0-0-01"
      },
      "imageCategory": {
        "S": "Movie"
      },
      "Description": {
        "S": "The story is set in a colorful and vibrant world of the Trolls, small and happy creatures with wild and colorful hair."
      },
      "port": {
        "S": "80"
      },
      "envVars": {
        "L": [
          {
            "M": {
              "key": {
                "S": "VAR1"
              },
              "value": {
                "S": "value1"
              }
            }
          },
          {
            "M": {
              "key": {
                "S": "VAR2"
              },
              "value": {
                "S": "value2"
              }
            }
          }
        ]
      },
      "volumes": {
        "L": [
          {
            "M": {
              "key": {
                "S": "/host/path"
              },
              "value": {
                "S": "/container/path"
              }
            }
          }
        ]
      },
      "imageLabels": {
        "L": [
          {
            "M": {
              "key": {
                "S": "label1"
              },
              "value": {
                "S": "value1"
              }
            }
          }
        ]
      },
      "commandOverride": {
        "S": "/app/run.sh"
      },
      "entrypointOverride": {
        "S": "/app/entrypoint.sh"
      },
      "workingDir": {
        "S": "/app"
      },
      "image": {
        "S": "data:image/png;base64,iVBORw0KGgo"
      }
    }
  },
  "userMetadata": {
    "M": {
      "firstName": {
        "S": "James"
      },
      "lastName": {
        "S": "Bond"
      },
      "email": {
        "S": "jamesbond@k8or.com"
      }
    }
  },
  "eventMetadata": {
    "M": {
      "timestamp": {
        "S": "timestamp"
      }
    }
  }, 
  "createdTimestampUTC": {
    "S": "2025-02-18 08:49:02"
  },
  "createdTimestampUnix": {
    "S": "1739632730"
  }
}
```
* **Endpoint:** `https://orbit.k8or.com/upload/UploadedImageUserEventLogChartStore.php`
```
upload/
├── src/
│   ├── routes/
│   ├── functions/
│   │   └── UploadedImageUserEventLogChartStore.php 
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol:** `https`
* **Request Type:** `POST`
* **Request Payload:**
```json
{
  "PID": "c8bms06-c20bre21-e170",
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
  },
  "eventMetadata": {
    "eventType": "image-uploaded",
    "timestamp": "timestamp"
  }
}
```
* **Response Payload:**
```json
{
  "PID": "c8bms06-c20bre21-e170",
  "status": "success",
  "message": "The user event was recorded successfully.",
}
```

### 6.7. **`c8bms06-c20bre24-EEE`:** The `Portal Upload Logic` Backend Microservice writes error logs to the `user-error-log-1624-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `errorType` (String)
* **Endpoint:** `https://orbit.k8or.com/upload/GetUserDataViewPort.php`
```
upload/
├── src/
│   ├── routes/
│   │   └── ErrorLog.php
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