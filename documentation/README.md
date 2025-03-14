# k8or Orbit Application's Image Management for Kubernetes on AWS

k8or Orbit application simplifies container image management for Kubernetes deployments on AWS. This document describes available image manipulation actions.

# Introduction

k8or Orbit application provides a set of actions to handle various image-related tasks, from initial upload to deployment and cleanup, streamlining image management within the Kubernetes/AWS environment.

# k8or Orbit Actions

## Upload Image  

The **Frontend User** uploads an image from the **Portal Upload Frontend UI**. The image is saved to the k8or Orbit backend resources but is **NOT** automatically deployed to the K3s cluster. 

* **See [upload-dir](upload-dir/README.md) for details.**

## Search Image

The **Frontend User** visits the **Portal Search Frontend UI** where they can search for images by name, category, or other criteria. 

* **See [search-dir](search-dir/README.md) for details.**

## Deploy Image

1.  User clicks the **"Deploy" button** on the **Portal Search Frontend UI**.
    * **Deploy Form:** A deployment form is displayed within the modal. This form contains fields for specifying replicas, resource limits, other deployment-specific configurations and user-related information, such as first name, last name and email address.
    * User fills out the form with the desired values and submits it.
2.  **Confirmation Popup:** A modal/popup appears, asking: **"Are you sure you want to deploy this image to the *current environment*?"** (**Yes**/**No**).
3.  **If "Yes"**:
    * The **image** is **deployed** to the **K3s cluster** of the **current environment** (e.g., development, testing, production).
    * A success or failure message is displayed to the **Frontend User**. 

* **See [deploy-dir](deploy-dir/README.md) for more information and form details.**

**Backend Integration Overview:**
1.  **User Event Log:** A record of the user event is written to a DynamoDB table labeled **"deployment-initiated"**.
2.  **Image Transfer:** The image is copied from its current location in an S3 bucket to a designated Docker Hub organization/repository.
3.  **Helm Chart Retrieval:** The Helm chart associated with the image is retrieved from a DynamoDB table.
4.  **Helm Chart to GitLab:** The retrieved Helm chart is then copied to a GitLab repository.
5.  **Deployment List Update:** The `deploy-list.yaml` file in the GitLab repository is updated to include the new deployment resources (defined by the Helm chart).
6.  **K3s Cluster Deployment:** The resources defined in the updated `deploy-list.yaml` file are deployed to the K3s cluster of the target environment (development, testing, or production).
7.  **Deployment Status Update:** The status of the deployed image is updated in an MySQL database from **"Deployed: none"** to **"Deployed: Jan 25, 2025"**.
8.  **Deployment Event Logs:** Detailed logs of the deployment process for *all* deployed resources are written to a DynamoDB table labeled **"deployed"**.
9.  **Error Logging:** If any errors occur during the deployment process, they are logged to a separate DynamoDB table for user error logs. 

## Transfer Image

1.  User clicks the **"Transfer" button** on the **Portal Search Frontend UI**.
    * **Transfer Form:** A transfer form is displayed within the modal. This form contains fields for specifying any transfer-specific configurations and user-related information, such as first name, last name and email address.
    * User fills out the form and submits it.
2.  **Confirmation Popup:** A modal/popup appears: **"Are you sure you want to transfer this image to the *next environment*? This action will delete the image from the *current environment*".** (**Yes**/**No**).
3.  **If "Yes":**
    * The **image** is **transferred** (**copied to the next environment** and **deleted from the current environment**).
    * A success or failure message is displayed. 

* **See [transfer-dir](transfer-dir/README.md) for more information and form details.**

**Backend Integration Overview:**

1.  **Image Tag Update:** The image tag **"environment"** is updated in the MySQL database to reflect the desired target environment.
2.  **Image Transfer:** The image is copied from the current environment's S3 bucket and pushed to the new, desired environment's S3 bucket.
3.  **Image Deletion (Current Bucket):** The **image** is **deleted** from the current environment's S3 bucket.
4.  **Image to Deleted Bucket:** The image is pushed to the **"deleted-image-bucket"** in S3.
5.  **Helm Chart Transfer:** The image's Helm chart is copied from the current environment's DynamoDB table and pushed to the desired environment's DynamoDB table.
6.  **Helm Chart to Deleted Table:** The image's Helm chart is removed from the current environment's DynamoDB table and pushed to the **"deleted-helm-chart"** DynamoDB table.
7.  **User Event Log:** A record of the user event (move initiation) is written to a DynamoDB table labeled **"move-initiated"**.
8.  **User Error Logs:** If any errors occur during the move process, they are written to a separate DynamoDB table for user error logs.

## Scan Image

1.  User clicks the **"Scan" button** on the **Portal Search Frontend UI**.
    * **Scan Form:** A scan form is displayed within the modal. This form contains fields for specifying scan-specific parameters and user-related information, such as first name, last name and email address.
    * User fills out the form and submits it.
2.  **Confirmation Popup:** A modal/popup appears: **"Are you sure you want to scan this image?"** (**Yes**/**No**).
3.  **If "Yes":**
    * The **image** is **scanned**.
    * Scan results are displayed to the **Frontend User**. 

* **See [scan-dir](scan-dir/README.md) for more information and form details.**

## View Chart

1. User clicks the **"Chart" button** on the **Portal Search Frontend UI**.
2. The **Helm chart** for the image is **displayed**. 

* **See [chart-dir](chart-dir/README.md) for more information and form details.**

**Backend Integration Overview:**

1.  **Helm Chart Retrieval:** The Helm chart is retrieved from the DynamoDB table.
2.  **Frontend Display:** The retrieved Helm chart is displayed on the frontend UI.
3.  **No Backend Updates:** There are no backend updates associated with this action; it is a read-only operation.

## Delete Image

1.  User clicks the **"Delete" button** on the **Portal Search Frontend UI**.
    * **Delete Form:** A delete form is displayed within the modal. This form contains fields for specifying deletion-specific configurations and user-related information, such as first name, last name and email address.
    * User fills out the form and submits it.
2.  **Confirmation Popup:** A modal/popup appears: **"Are you sure you want to delete this image from the *current environment*?"** (**Yes**/**No**).
3.  **If "Yes":**
    * The **image** is **deleted** from the **current environment**.
    * A success or failure message is displayed. 

* **See [delete-dir](delete-dir/README.md) for more information and form details.**

**Backend Integration Overview:**

1. **User Event Log:** A record of the user event (deletion initiation) is written to a DynamoDB table.
2. **GitLab Update:** The `delete-list.yaml` file in the GitLab repository is updated to remove the image-related resources.
3. **K3s Cluster Deletion:** The image and its associated resources are deleted from the K3s cluster.
4. **Deployment Status Update:** The image's deployment status in the MySQL database is updated from **"Deployed: Jan 24, 2025"** to **"Deployed: none"**.
5. **Image Marked Deleted:** The **image** is **marked** as **deleted** in the MySQL database.
6. **Image to Deleted Bucket:** The image is copied from the current S3 bucket to the `deleted-image-bucket` and then deleted from the current bucket.
7. **Helm Chart to Deleted Table:** The image's Helm chart is copied to the `deleted-helm-chart` DynamoDB table and then deleted from the current DynamoDB table.
8. **Image Retained (Marked Deleted):** The image is **NOT** deleted from the MySQL database but is marked as deleted. The image will remain listed on the **Portal Search Frontend UI**, allowing users to restore it.
9. **Deployment Event Log:** The deployment event DynamoDB table is updated with a new log entry indicating **"deleted"**.
10. **User Error Logs:** If any errors occur during the deletion process, they are written to a separate DynamoDB table for user error logs.

### Note: The deletion process deletes the image from the K3s cluster and its associated backend resources, except the MySQL database. While the image data and Helm chart are moved to "deleted" backend resources, the image metadata remains in the MySQL database (marked as "deleted") for potential restoration.

## Restore Image

1.  User selects a *deleted* image (from any environment) and clicks the **"Restore" button** on the **Portal Search Frontend UI**.
    * **Restore Form:** A restore form is displayed within the modal. This form contains fields for specifying restore-specific configurations and user-related information, such as first name, last name and email address.
    * User fills out the form and submits it.
2.  **Confirmation Popup:** A modal/popup appears: **"Are you sure you want to restore this image to the *current environment*?"** (**Yes**/**No**).
3.  **If "Yes":**
    * The **image** is **restored** to the **current environment**.
    * A success or failure message is displayed. 

* **See [restore-dir](restore-dir/README.md) for more information and form details.**

**Backend Integration Overview:**

1.  **User Event Log:** A record of the user event (restore initiation) is written to a DynamoDB table.
2.  **Environment Restriction:** The image is restored to the **SAME** **environment from which it was deleted**. *Restoring to a different environment is not allowed.*
3.  **Image Transfer:** The image is copied from the `deleted-image-bucket` to the current environment's S3 bucket and then deleted from the `deleted-image-bucket`.
4.  **Helm Chart Transfer:** The image's Helm chart is copied from the `deleted-helm-chart` DynamoDB table to the current environment's DynamoDB table and then deleted from the `deleted-helm-chart` DynamoDB table.
5.  **Image Status Update:** The image's status in the MySQL database is updated from **"deleted"** to reflect the **current environment**.
6.  **User Error Logs:** If any errors occur during the restore process, they are written to a separate DynamoDB table for user error logs.

### Note: The restore process *only* restores the image and its metadata. It does *NOT* automatically deploy the image to the K3s cluster. A separate deployment step is required after a successful restore. Restore and deployment are distinct processes.

---

## Event Log Types:

### Upload Image Process:

* **DynamoDB Table: `upload-image-event-log-1621-tbl-k8d-aws`**

    * **`upload-initiated`:** Recorded when the user initiates the upload process by submitting the form. This marks the beginning of the upload workflow from the backend perspective.

    * **`image-received`:** Recorded when the image data is successfully received by the backend microservice. This confirms the image has been transmitted.

    * **`temporary-image-stored`:** Recorded after the image is temporarily stored in the `temporary-app-image-1203-bkt-k8d-aws` S3 bucket. This indicates successful temporary storage pending validation.

    * **`image-validation-started`:** Recorded just before the image validation process begins. This provides a clear start marker.

    * **`image-validated`:** Recorded after the image is successfully validated. This confirms the image meets all criteria.

    * **`image-stored`:** Recorded after the validated image is permanently stored in the `app-image-1206-bkt-k8d-aws` S3 bucket.

    * **`user-data-saved`:** Recorded after the user data (from the form) is successfully saved to the `user-data-1612-tbl-k8d-aws` DynamoDB table.

    * **`image-metadata-saved`:** Recorded after the image metadata is successfully saved to the `app-image-metadata-1615-tbl-k8d-aws` DynamoDB table.

    * **`user-input-saved`:** Recorded after the user input (for manifest generation) is successfully saved to the `user-input-manifest-1606-tbl-k8d-aws` DynamoDB table.

    * **`kustomization-started`:** Recorded when the kustomize manifest generation process begins.

    * **`kustomization-completed`:** Recorded when the kustomize manifest files are successfully generated and saved to the `k8or-kustomize-1609-tbl-k8d-aws` DynamoDB table.

    * **`database-update-started`:** Recorded before the image details are updated in the MySQL database.

    * **`database-updated`:** Recorded after the image details (including manifest references) are successfully saved to the MySQL database. This signifies the completion of all backend processing.

    * **`upload-completed`:** Recorded when the *entire* upload process, including database updates, is finished. This provides a single "all done" event.

---

### Deploy Image Process:

* **DynamoDB Table: `deploy-image-event-log-1621-tbl-k8d-aws`**

    * **`deployment-initiated`:** Recorded when the user initiates the deployment process by submitting the deployment details form. 

    * **`deployment-detail-received`:** Logged by the `c8bms15` backend microservice upon successful receipt of the deployment details.

    * **`image-retrieval-started`:** Logged by the `c8bms15` backend microservice before retrieving the image from S3.

    * **`image-retrieved`:** Logged by the `c8bms15` backend microservice after successfully retrieving the image from S3.

    * **`image-push-started`:** Logged by the `c56bms03` syncmaster microservice before pushing the image to Docker Hub.

    * **`image-pushed`:** Logged by the `c56bms03` syncmaster microservice after successfully pushing the image to Docker Hub.

    * **`manifest-retrieval-started`:** Logged by the `c56bms03` syncmaster microservice before retrieving the manifests from DynamoDB.

    * **`manifest-retrieved`:** Logged by the `c56bms03` syncmaster microservice after successfully retrieving the manifests from DynamoDB.

    * **`manifest-push-started`:** Logged by the `c56bms03` syncmaster microservice before pushing the manifests to GitLab.

    * **`manifest-pushed`:** Logged by the `c56bms03` syncmaster microservice after successfully pushing the manifests to GitLab.

    * **`deploy-list-update-started`:** Logged before updating the `deploy-list.yaml` file.

    * **`deploy-list-updated`:** Logged after successfully updating the `deploy-list.yaml` file.

    * **`argo-cd-sync-started`:**  While not directly logged by the microservices, it's essential to track when Argo CD starts its synchronization.  This would be an Argo CD event.

    * **`argo-cd-sync-plan-generated`:** An Argo CD internal event indicating that the synchronization plan has been created.

    * **`akuity-agent-instruction-sent`:** Logged when Argo CD sends instructions to the Akuity agent.

    * **`kubernetes-manifests-applied`:** While not logged by the microservices, this event represents the successful application of the manifests to the K3s cluster.  This event could be tracked by the Akuity agent.

    * **`argo-cd-sync-completed`:** Logged by Argo CD after the synchronization is completed.

    * **`deployment-status-updated`:** Logged after the image status in the MySQL database is updated.

    * **`deployment-completed`:** Logged when the *entire* deployment process is considered complete.  This acts as a final confirmation.

    * **`deployment-notification-sent`:** Logged when Argo CD sends the notification to the Deploy BE MSV.

    * **`deployment-notification-received`:** Logged by the Deploy BE MSV when it receives the notification.

---

### Transfer Image Process:

* **DynamoDB Table: `transfer-image-event-log-1621-tbl-k8d-aws`**

    * **`transfer-initiated`:** Recorded when the user initiates the transfer process by submitting the transfer details. This is the first server-side event. It's logged by `c8bms18`.

    * **`transfer-detail-received`:** Log the successful receipt of the transfer details by the backend microservice (`c8bms18`). This confirms that the backend received the request.

    * **`image-retrieval-started`:** Before retrieving the image, log that the retrieval process has begun.

    * **`image-retrieved`:** Recorded when the image file is successfully retrieved from the source S3 bucket (`c16bre06`).

    * **`image-upload-started`:** Log the initiation of the upload to the target S3 bucket.

    * **`image-uploaded`:** Recorded when the image is successfully uploaded to the destination S3 bucket (`c16bre09`).

    * **`manifest-retrieval-started`:** Log the start of the manifest retrieval.

    * **`manifest-retrieved`:** Recorded when the kustomize manifest files are successfully retrieved from the DynamoDB table (`c20bre09`).

    * **`manifest-save-started`:** Log the start of the saving of the manifests to the target DynamoDB table.

    * **`manifest-saved`:** Recorded when the retrieved kustomize manifest files are successfully saved to the destination DynamoDB table (`c20bre30`).

    * **`database-update-started`:** Before updating the database, log that the update process has begun.

    * **`database-updated`:** Recorded when the image metadata is successfully updated in the MySQL database (`c32bre03`) with the "Testing" label.

    * **`transfer-completed`:** Recorded when the entire transfer process is successfully completed. This acts as a final confirmation.

---

# Upload Image Legs:

* The **Image Upload Process** is analyzed across multiple **Legs**, each representing a distinct interaction between the **Frontend User**, **Frontend/Backend Microservices**, and **Backend Resources**. 
* Each **Leg** (e.g, `L4`, `L8`, `L12`) is further subdivided into parts (e.g, `P4`, `P8`, `P12`), detailing the specific **Start** and **End Resources** involved and the **Sequence of Operations**. 
* The **Process Identification (PID) Naming Convention** used is as follows: **`[Start Resource]-[End Resource]-[Execution Order]`**.

## Leg L4: Image Upload Static Data Retrieval

This **Leg** describes the complete flow of an **Image Upload Static Data Retrieval**, from the **Frontend User** interaction to the **Image Upload Static Data Retrieval**.

* **Part L4P4: User Interaction with Frontend Microservice**
    * The **PID** of **Part L4P4** is **`c2usr03-c8fms03-e05`**.
    * The **Start Resource** of **Part L4P4** is the **`c2usr03` Frontend User**.
    * The **End Resource** of **Part L4P4** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **Execution Order** of **Part L4P4** is `e05`.
    * **Description:** The **`c2usr03` Frontend User** interacts with the **`c8fms03` Upload Image Frontend Microservice UI** to initiate the **Image Upload Process**. This interaction triggers the subsequent steps in the flow.

* **Part L4P8: Frontend to Backend Microservice Communication**
    * The **PID** of **Part L4P8** is **`c8fms03-c8bms06-e10`**.
    * The **Start Resource** of **Part L4P8** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **End Resource** of **Part L4P8** is the **`c8bms06` Upload Image Backend Microservice**.
    * The **Execution Order** of **Part L4P8** is `e10`.
    * **Description:** The **`c8fms03` Upload Image Frontend Microservice** communicates with the **`c8bms06` Upload Image Backend Microservice** to retrieve **Static Data** required for the **Upload Process**.

* **Part L4P12: Backend Microservice Data Retrieval**
    * The **PID** of **Part L4P12** is **`c8bms06-c20bre27-e15`**.
    * The **Start Resource** of **Part L4P12** is the **`c8bms06` Upload Image Backend Microservice**.
    * The **End Resource** of **Part L4P12** is the **`c20bre27` DynamoDB Table**.
    * The **Execution Order** of **Part L4P12** is `e15`.
    * **Description:** The **`c8bms06` Upload Image Backend Microservice** queries the **`c20bre27` DynamoDB Table** to fetch the necessary **Image Upload Static Data**. This data is then returned to the **`c8fms03` Upload Image Frontend Microservice** for presentation to the **Frontend User**.

## Leg L8:

* **Part L8P4: TBD**
    * The **PID** of **Part L8P4** is **`c2usr03-c8fms03-e20`**.
    * The **Start Resource** of **Part L8P4** is the **`c2usr03` Frontend User**.
    * The **End Resource** of **Part L8P4** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **Execution Order** of **Part L8P4** is `e20`.
    * **Description:** TBD

* **Part L8P8: TBD**
    * The **PID** of **Part L8P8** is **`c8fms03-c12bms03-e25`**.
    * The **Start Resource** of **Part L8P8** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **End Resource** of **Part L8P8** is the **`c12bms03` Manifestor Image Validator Backend Microservice**.
    * The **Execution Order** of **Part L8P8** is `e25`.
    * **Description:** TBD

* **Part L8P12: TBD**
    * The **PID** of **Part L8P12** is **`c12bms03-c16bre03-e30`**.
    * The **Start Resource** of **Part L8P12** is the **`c12bms03` Manifestor Image Validator Backend Microservice**.
    * The **End Resource** of **Part L8P12** is the **`c16bre03` S3 Bucket `temporary-app-image-1203-bkt-k8d-aws`**.
    * The **Execution Order** of **Part L8P12** is `e30`.
    * **Description:** TBD

## Leg L12:

* **Part L12P4: TBD**
    * The **PID** of **Part L12P4** is **`c2usr03-c8fms03-e20`**.
    * The **Start Resource** of **Part L12P4** is the **`c2usr03` Frontend User**.
    * The **End Resource** of **Part L12P4** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **Execution Order** of **Part L12P4** is `e20`.
    * **Description:** TBD

* **Part L12P8: TBD**
    * The **PID** of **Part L12P8** is **`c8fms03-c12bms03-e25`**.
    * The **Start Resource** of **Part L12P8** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **End Resource** of **Part L12P8** is the **`c12bms03` Manifestor Image Validator Backend Microservice**.
    * The **Execution Order** of **Part L12P8** is `e25`.
    * **Description:** TBD

* **Part L12P12: TBD**
    * The **PID** of **Part L12P12** is **`c12bms03-c16bre06-e35`**.
    * The **Start Resource** of **Part L12P12** is the **`c12bms03` Manifestor Image Validator Backend Microservice**.
    * The **End Resource** of **Part L12P12** is the **`c16bre06` S3 Bucket `app-image-1206-bkt-k8d-aws`**.
    * The **Execution Order** of **Part L12P12** is `e35`.
    * **Description:** TBD

## Leg L16:

* **Part L16P4: TBD**
    * The **PID** of **Part L16P4** is **`c2usr03-c8fms03-e20`**.
    * The **Start Resource** of **Part L16P4** is the **`c2usr03` Frontend User**.
    * The **End Resource** of **Part L16P4** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **Execution Order** of **Part L16P4** is `e20`.
    * **Description:** TBD

* **Part L16P8: TBD**
    * The **PID** of **Part L16P8** is **`c8fms03-c12bms06-e40`**.
    * The **Start Resource** of **Part L16P8** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **End Resource** of **Part L16P8** is the **`c12bms06` Manifestor Save Metadata Backend Microservice**.
    * The **Execution Order** of **Part L16P8** is `e40`.
    * **Description:** TBD

* **Part L16P12: TBD**
    * The **PID** of **Part L16P12** is **`c12bms06-c20bre06-e45`**.
    * The **Start Resource** of **Part L16P12** is the **`c12bms06` Manifestor Save Metadata Backend Microservice**.
    * The **End Resource** of **Part L16P12** is the **`c20bre06` DynamoDB Table**.
    * The **Execution Order** of **Part L16P12** is `e45`.
    * **Description:** TBD

## Leg L20:

* **Part L20P4: TBD**
    * The **PID** of **Part L20P4** is **`c2usr03-c8fms03-e20`**.
    * The **Start Resource** of **Part L20P4** is the **`c2usr03` Frontend User**.
    * The **End Resource** of **Part L20P4** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **Execution Order** of **Part L20P4** is `e20`.
    * **Description:** TBD

* **Part L20P8: TBD**
    * The **PID** of **Part L20P8** is **`c8fms03-c12bms06-e40`**.
    * The **Start Resource** of **Part L20P8** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **End Resource** of **Part L20P8** is the **`c12bms06` Manifestor Save Metadata Backend Microservice**.
    * The **Execution Order** of **Part L20P8** is `e40`.
    * **Description:** TBD

* **Part L20P12: TBD**
    * The **PID** of **Part L20P12** is **`c12bms06-c20bre12-e50`**.
    * The **Start Resource** of **Part L20P12** is the **`c12bms06` Manifestor Save Metadata Backend Microservice**.
    * The **End Resource** of **Part L20P12** is the **`c20bre12` DynamoDB Table `user-data-1612-tbl-k8d-aws`**.
    * The **Execution Order** of **Part L20P12** is `e50`.
    * **Description:** TBD

## Leg L24:

* **Part L24P4: TBD**
    * The **PID** of **Part L24P4** is **`c2usr03-c8fms03-e20`**.
    * The **Start Resource** of **Part L24P4** is the **`c2usr03` Frontend User**.
    * The **End Resource** of **Part L24P4** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **Execution Order** of **Part L24P4** is `e20`.
    * **Description:** TBD

* **Part L24P8: TBD**
    * The **PID** of **Part L24P8** is **`c8fms03-c12bms06-e40`**.
    * The **Start Resource** of **Part L24P8** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **End Resource** of **Part L24P8** is the **`c12bms06` Manifestor Save Metadata Backend Microservice**.
    * The **Execution Order** of **Part L24P8** is `e40`.
    * **Description:** TBD

* **Part L24P12: TBD**
    * The **PID** of **Part L24P12** is **`c12bms06-c20bre15-e55`**.
    * The **Start Resource** of **Part L24P12** is the **`c12bms06` Manifestor Save Metadata Backend Microservice**.
    * The **End Resource** of **Part L24P12** is the **`c20bre15` DynamoDB Table `app-image-metadata-1615-tbl-k8d-aws`**.
    * The **Execution Order** of **Part L24P12** is `e55`.
    * **Description:** TBD

## Leg L24:

* **Part L24P4: TBD**
    * The **PID** of **Part L24P4** is **`c2usr03-c8fms03-e20`**.
    * The **Start Resource** of **Part L24P4** is the **`c2usr03` Frontend User**.
    * The **End Resource** of **Part L24P4** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **Execution Order** of **Part L24P4** is `e20`.
    * **Description:** TBD

* **Part L24P8: TBD**
    * The **PID** of **Part L24P8** is **`c8fms03-c12bms09-e60`**.
    * The **Start Resource** of **Part L24P8** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **End Resource** of **Part L24P8** is the **`c12bms09` Manifestor Chart Generator Backend Microservice**.
    * The **Execution Order** of **Part L24P8** is `e60`.
    * **Description:** TBD

* **Part L24P12: TBD**
    * The **PID** of **Part L24P12** is **`c12bms09-c20bre03-e65`**.
    * The **Start Resource** of **Part L24P12** is the **`c12bms09` Manifestor Chart Generator Backend Microservice**.
    * The **End Resource** of **Part L24P12** is the **`c20bre03` DynamoDB Table**.
    * The **Execution Order** of **Part L24P12** is `e65`.
    * **Description:** TBD

## Leg L32:

* **Part L32P4: TBD**
    * The **PID** of **Part L32P4** is **`c2usr03-c8fms03-e20`**.
    * The **Start Resource** of **Part L32P4** is the **`c2usr03` Frontend User**.
    * The **End Resource** of **Part L32P4** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **Execution Order** of **Part L32P4** is `e20`.
    * **Description:** TBD

* **Part L32P8: TBD**
    * The **PID** of **Part L32P8** is **`c8fms03-c12bms09-e60`**.
    * The **Start Resource** of **Part L32P8** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **End Resource** of **Part L32P8** is the **`c12bms09` Manifestor Chart Generator Backend Microservice**.
    * The **Execution Order** of **Part L32P8** is `e60`.
    * **Description:** TBD

* **Part L32P12: TBD**
    * The **PID** of **Part L32P12** is **`c12bms09-c20bre06-e70`**.
    * The **Start Resource** of **Part L32P12** is the **`c12bms09` Manifestor Chart Generator Backend Microservice**.
    * The **End Resource** of **Part L32P12** is the **`c20bre06` DynamoDB Table**.
    * The **Execution Order** of **Part L32P12** is `e70`.
    * **Description:** TBD

## Leg L36:

* **Part L36P4: TBD**
    * The **PID** of **Part L36P4** is **`c2usr03-c8fms03-e20`**.
    * The **Start Resource** of **Part L36P4** is the **`c2usr03` Frontend User**.
    * The **End Resource** of **Part L36P4** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **Execution Order** of **Part L36P4** is `e20`.
    * **Description:** TBD

* **Part L36P8: TBD**
    * The **PID** of **Part L36P8** is **`c8fms03-c12bms09-e60`**.
    * The **Start Resource** of **Part L36P8** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **End Resource** of **Part L36P8** is the **`c12bms09` Manifestor Chart Generator Backend Microservice**.
    * The **Execution Order** of **Part L36P8** is `e60`.
    * **Description:** TBD

* **Part L36P12: TBD**
    * The **PID** of **Part L36P12** is **`c12bms09-c20bre09-e75`**.
    * The **Start Resource** of **Part L36P12** is the **`c12bms09` Manifestor Chart Generator Backend Microservice**.
    * The **End Resource** of **Part L36P12** is the **`c20bre09` DynamoDB Table**.
    * The **Execution Order** of **Part L36P12** is `e75`.
    * **Description:** TBD

## Leg L40:

* **Part L40P4: TBD**
    * The **PID** of **Part L40P4** is **`c2usr03-c8fms03-e20`**.
    * The **Start Resource** of **Part L40P4** is the **`c2usr03` Frontend User**.
    * The **End Resource** of **Part L40P4** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **Execution Order** of **Part L40P4** is `e20`.
    * **Description:** TBD

* **Part L40P8: TBD**
    * The **PID** of **Part L40P8** is **`c8fms03-c12bms09-e60`**.
    * The **Start Resource** of **Part L40P8** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **End Resource** of **Part L40P8** is the **`c12bms09` Manifestor Chart Generator Backend Microservice**.
    * The **Execution Order** of **Part L40P8** is `e60`.
    * **Description:** TBD

* **Part L40P12: TBD**
    * The **PID** of **Part L40P12** is **`c12bms09-c20bre21-e80`**.
    * The **Start Resource** of **Part L40P12** is the **`c12bms09` Manifestor Chart Generator Backend Microservice**.
    * The **End Resource** of **Part L40P12** is the **`c20bre21` DynamoDB Table `user-event-log-1621-tbl-k8d-aws`**.
    * The **Execution Order** of **Part L40P12** is `e80`.
    * **Description:** TBD

## Leg L44:

* **Part L44P4: TBD**
    * The **PID** of **Part L44P4** is **`c2usr03-c8fms03-e20`**.
    * The **Start Resource** of **Part L44P4** is the **`c2usr03` Frontend User**.
    * The **End Resource** of **Part L44P4** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **Execution Order** of **Part L44P4** is `e20`.
    * **Description:** TBD

* **Part L44P8: TBD**
    * The **PID** of **Part L44P8** is **`c8fms03-c12bms09-e60`**.
    * The **Start Resource** of **Part L44P8** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **End Resource** of **Part L44P8** is the **`c12bms09` Manifestor Chart Generator Backend Microservice**.
    * The **Execution Order** of **Part L44P8** is `e60`.
    * **Description:** TBD

* **Part L44P12: TBD**
    * The **PID** of **Part L44P12** is **`c12bms09-c20bre24-e85`**.
    * The **Start Resource** of **Part L44P12** is the **`c12bms09` Manifestor Chart Generator Backend Microservice**.
    * The **End Resource** of **Part L44P12** is the **`c20bre24` DynamoDB Table `user-error-log-1624-tbl-k8d-aws`**.
    * The **Execution Order** of **Part L44P12** is `e85`.
    * **Description:** TBD

## Leg L44:

* **Part L44P4: TBD**
    * The **PID** of **Part L44P4** is **`c2usr03-c8fms03-e20`**.
    * The **Start Resource** of **Part L44P4** is the **`c2usr03` Frontend User**.
    * The **End Resource** of **Part L44P4** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **Execution Order** of **Part L44P4** is `e20`.
    * **Description:** TBD

* **Part L44P8: TBD**
    * The **PID** of **Part L44P8** is **`c8fms03-c8bms06-e90`**.
    * The **Start Resource** of **Part L44P8** is the **`c8fms03` Upload Image Frontend Microservice**.
    * The **End Resource** of **Part L44P8** is the **`c8bms06` Upload Image Backend Microservice**.
    * The **Execution Order** of **Part L44P8** is `e90`.
    * **Description:** TBD

* **Part L44P12: TBD**
    * The **PID** of **Part L44P12** is **`c8bms06-c32bre03-e95`**.
    * The **Start Resource** of **Part L44P12** is the **`c8bms06` Upload Image Backend Microservice**.
    * The **End Resource** of **Part L44P12** is the **`c32bre03` MySQL Database `app_image_mql_3203_dtb_k8d_aws`**.
    * The **Execution Order** of **Part L44P12** is `e95`.
    * **Description:** TBD