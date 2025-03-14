# Issue Tracking Identifier Creation

This document details the standardized methodology for creating and applying issue tracking identifiers within the k8or Orbit application. 

## Constituent Elements of the Identifier System:

The identifier system comprises two primary identifier types:

1.  **Issue Identifier (Issue ID):** Designates a unique instance of a reported anomaly or defect.
2.  **Message Identifier (Message ID):** Delineates a specific communication event within the context of an issue.

Both identifier types are constructed from a set of standardized constituent elements, ensuring consistency and uniformity.

### 1. Ticket Identifier (Ticket ID): Contextual Linkage

* **Definition:** The identifier that corresponds directly to the relevant Jira ticket.
* **Format:** `k8ororbit-252` (exemplary) - Project-specific nomenclature.
* **Acquisition Procedure:** Extracted directly from the Jira ticket interface.
* **Purpose:** To establish an explicit and direct linkage between the reported issue and its originating Jira task, facilitating seamless cross-referencing and contextualization.

### 2. Issue Date (Date): Temporal Contextualization

* **Definition:** The date on which the issue was initially reported and recorded.
* **Format:** `YYYYMMDD` (e.g., `20250220`).
* **Construction Procedure:** Composed of the year (YYYY), month (MM), and day (DD) in sequential order, without delimiters.
* **Purpose:** To provide a precise chronological marker for the issue's origination, enabling temporal filtering and analysis.

### 3. Issue Sequence (Issue Number): Differentiation of Concurrent Issues

* **Definition:** A sequential numerical designator used to differentiate multiple issues reported on the same date for a single Jira ticket.
* **Format:** `iXXX` (e.g., `i001`, `i002`, `i010`).
* **Construction Procedure:** Initialized at `i001` for the first issue reported on a given date and incremented by one for each subsequent issue.
* **Purpose:** To maintain uniqueness among issues reported concurrently within the same Jira ticket context.

### 4. Message Sequence (Message Number): Delineation of Issue-Specific Communications

* **Definition:** A sequential numerical designator used to delineate individual communication events within the context of a specific issue.
* **Format:** `mXXX` (e.g., `m0001`, `m0002`, `m0010`).
* **Construction Procedure:** Initialized at `m0001` for the first message within an issue and incremented by one for each subsequent message.
* **Purpose:** To distinguish individual communications within an issue context, facilitating granular tracking of discussions and resolutions.

## Identifier Construction Standards:

### 1. Issue Identifier (Issue ID) Construction:

* **Step 1:** Initiate with the Ticket Identifier.
* **Step 2:** Append a hyphen (`-`) as a delimiter.
* **Step 3:** Append the Issue Date.
* **Step 4:** Append a hyphen (`-`) as a delimiter.
* **Step 5:** Append the Issue Sequence.
* **Exemplar:** `k8ororbit-252-20250220-i001`.

### 2. Message Identifier (Message ID) Construction:

* **Step 1:** Initiate with the Issue Identifier.
* **Step 2:** Append a hyphen (`-`) as a delimiter.
* **Step 3:** Append the Message Sequence.
* **Exemplar:** `k8ororbit-252-20250220-i001-m0001`.

## Issue Example

### Issue Title: `Uploaded Image Not Displaying on Search UI`

* **Issue ID:** `k8ororbit-252-20250220-i001`
* **Old Diagram ID:** `c8-c16-c20-c32-c52-c56-c96-c100-c104-c108-cc212-c216-c228-c340-v0-0-05`
* **New Diagram ID:** `c8-c16-c20-c32-c52-c56-c96-c100-c104-c108-cc212-c216-c228-c340-v0-0-06`
* **Description:** `Uploaded image not displaying on Search UI.`
* **Cause:** `Missing fields in payload to create image in WordPress database: 'start_date' (ACF post meta), post featured image, and post category.`
* **Solution:** `Implemented a modification to the Upload Backend Microservice (MSV) to include the 'start_date' (ACF post meta), post featured image, and post category fields within the payload sent to the ViewPort (WordPress) BE MSV for image creation. The post featured image is now captured from the Upload FE MSV UI, encoded as base64, and transmitted to the ViewPort (WordPress) BE MSV. The Upload MSV now directly communicates with the ViewPort (WordPress) BE MSV to facilitate the transfer and storage of the image file within the WordPress uploads directory. This ensures proper data propagation and rendering on the Search UI. Deployed updated versions of the Search, Viewport, and Upload MSVs.`
* **Status:** `Done`
* **Reporter:** `Anna/BarbRock`
* **Assignee:** `Harinder/PoppyBean`
* **Priority:** `High`
* **Created At:** `2025-02-20T10:00:00Z`
* **Updated Resources:**
	* Diagram
	* Functionality document
	* ViewPort (WordPress) BE MSV
	* Upload BE MSV
	* Search FE MSV

### Message Log:

* **1. Message ID:** `k8ororbit-252-20250220-i001-m0001`
    * **Author:** `Harinder/PoppyBean`
    * **Timestamp:** `2025-02-20T10:05:00Z`
    * **Text:** `The uploaded image is not rendering correctly on the Search UI. This appears to be related to missing post data, potentially impacting both the post metadata and the displayed content.`

* **2. Message ID:** `k8ororbit-252-20250220-i001-m0002`
    * **Author:** `Harinder/PoppyBean`
    * **Timestamp:** `2025-02-20T10:20:00Z`
    * **Text:** `The Upload Backend MSV is functioning nominally. However, the payload requires updates to include the 'start_date' (ACF field), post featured image, and post category. How do we retrieve and transmit the post featured image?`

* **3. Message ID:** `k8ororbit-252-20250220-i001-m0003`
    * **Author:** `Anna/BarbRock`
    * **Timestamp:** `2025-02-20T10:35:00Z`
    * **Text:** `The post featured image should be selected through the Upload FE MSV UI, converted to base64 encoding, and then included in the payload sent to the backend.`

* **4. Message ID:** `k8ororbit-252-20250220-i001-m0004`
    * **Author:** `Harinder/PoppyBean`
    * **Timestamp:** `2025-02-20T11:20:00Z`
    * **Text:** `Where should the post featured image be stored?`

* **5. Message ID:** `k8ororbit-252-20250220-i001-m0005`
    * **Author:** `Anna/BarbRock`
    * **Timestamp:** `2025-02-20T11:45:00Z`
    * **Text:** `Where are images stored when uploaded via the WordPress admin panel?`

* **6. Message ID:** `k8ororbit-252-20250220-i0006`
    * **Author:** `Harinder/PoppyBean`
    * **Timestamp:** `2025-02-20T11:46:00Z`
    * **Text:** `On the EC2 instance, within the 'wp-uploads' directory.`

* **7. Message ID:** `k8ororbit-252-20250220-i0007`
    * **Author:** `Anna/BarbRock`
    * **Timestamp:** `2025-02-20T11:47:00Z`
    * **Text:** `Okay, let's utilize that directory.`

* **8. Message ID:** `k8ororbit-252-20250220-i0008`
    * **Author:** `Harinder/PoppyBean`
    * **Timestamp:** `2025-02-20T11:48:00Z`
    * **Text:** `The Upload MSV and ViewPort (WordPress) BE MSV are separate. How will the Upload MSV write directly to the WordPress upload directory?`

* **9. Message ID:** `k8ororbit-252-20250220-i0009`
    * **Author:** `Anna/BarbRock`
    * **Timestamp:** `2025-02-20T11:49:00Z`
    * **Text:** `The process can be structured as follows: 1) The Upload MSV receives the uploaded image. 2) It encodes the image data as base64. 3) It sends the base64 encoded image data to the ViewPort (WordPress) BE MSV. 4) The ViewPort (WordPress) BE MSV receives the base64 data and imports it into its 'wp-uploads' directory.`

* **10. Message ID:** `k8ororbit-252-20250220-i0010`
    * **Author:** `Harinder/PoppyBean`
    * **Timestamp:** `2025-02-20T11:50:00Z`
    * **Text:** `So, the Upload MSV will directly communicate with the ViewPort (WordPress) BE MSV?`

* **11. Message ID:** `k8ororbit-252-20250220-i0011`
    * **Author:** `Anna/BarbRock`
    * **Timestamp:** `2025-02-20T11:51:00Z`
    * **Text:** `Yes, or alternatively, the Frontend MSV could send the image directly to the ViewPort (WordPress) BE MSV.`

* **12. Message ID:** `k8ororbit-252-20250220-i0012`
    * **Author:** `Harinder/PoppyBean`
    * **Timestamp:** `2025-02-20T11:52:00Z`
    * **Text:** `In that case, we can update the category and image details by having the Upload MSV communicate directly with the ViewPort (WordPress) BE MSV.`

* **13. Message ID:** `k8ororbit-252-20250220-i0013`
    * **Author:** `Anna/BarbRock`
    * **Timestamp:** `2025-02-20T11:53:00Z`
    * **Text:** `Understood.`

* **14. Message ID:** `k8ororbit-252-20250220-i0014`
    * **Author:** `Harinder/PoppyBean`
    * **Timestamp:** `2025-02-20T11:54:00Z`
    * **Text:** `The updated payload has been implemented. The Search, Viewport, and Upload MSVs have been pushed with the changes.`

* **15. Message ID:** `k8ororbit-252-20250220-i0015`
    * **Author:** `Anna/BarbRock`
    * **Timestamp:** `2025-02-20T11:55:00Z`
    * **Text:** `Acknowledged.`

### JSON Issue Structure

```json
{
    "issue_id": "k8ororbit-252-20250220-i001",
    "description": "Uploaded image not displaying on Search UI.",
    "cause": "Missing fields in payload to create image in WordPress database: 'start_date' (ACF post meta), post featured image, and post category.",
    "solution": "Implemented a modification to the Upload Backend Microservice (MSV) to include the 'start_date' (ACF post meta), post featured image, and post category fields within the payload sent to the ViewPort (WordPress) BE MSV for image creation. The post featured image is now captured from the Upload FE MSV UI, encoded as base64, and transmitted to the ViewPort (WordPress) BE MSV. The Upload MSV now directly communicates with the ViewPort (WordPress) BE MSV to facilitate the transfer and storage of the image file within the WordPress uploads directory. This ensures proper data propagation and rendering on the Search UI. Deployed updated versions of the Search, Viewport, and Upload MSVs.",
    "status": "Done",
    "reporter": "Anna/BarbRock",
    "assignee": "Harinder/PoppyBean",
    "priority": "High",
    "created_at": "2025-02-20T10:00:00Z",
    "messages": [
        {
            "message_id": "k8ororbit-252-20250220-i001-m0001",
            "message_author": "Harinder/PoppyBean",
            "text": "The uploaded image is not rendering correctly on the Search UI. This appears to be related to missing post data, potentially impacting both the post metadata and the displayed content.",
            "timestamp": "2025-02-20T10:05:00Z"
        },
        {
            "message_id": "k8ororbit-252-20250220-i001-m0002",
            "message_author": "Harinder/PoppyBean",
            "text": "The Upload Backend MSV is functioning nominally. However, the payload requires updates to include the 'start_date' (ACF field), post featured image, and post category. How do we retrieve and transmit the post featured image?",
            "timestamp": "2025-02-20T10:20:00Z"
        },
        {
            "message_id": "k8ororbit-252-20250220-i001-m0003",
            "message_author": "Anna/BarbRock",
            "text": "The post featured image should be selected through the Upload FE MSV UI, converted to base64 encoding, and then included in the payload sent to the backend.",
            "timestamp": "2025-02-20T10:35:00Z"
        },
        {
            "message_id": "k8ororbit-252-20250220-i001-m0004",
            "message_author": "Harinder/PoppyBean",
            "text": "Where should the post featured image be stored?",
            "timestamp": "2025-02-20T11:20:00Z"
        },
        {
            "message_id": "k8ororbit-252-20250220-i001-m0005",
            "message_author": "Anna/BarbRock",
            "text": "Where are images stored when uploaded via the WordPress admin panel?",
            "timestamp": "2025-02-20T11:45:00Z"
        },
        {
            "message_id": "k8ororbit-252-20250220-i0006",
            "message_author": "Harinder/PoppyBean",
            "text": "On the EC2 instance, within the 'wp-uploads' directory.",
            "timestamp": "2025-02-20T11:46:00Z"
        },
        {
            "message_id": "k8ororbit-252-20250220-i0007",
            "message_author": "Anna/BarbRock",
            "text": "Okay, let's utilize that directory.",
            "timestamp": "2025-02-20T11:47:00Z"
        },
        {
            "message_id": "k8ororbit-252-20250220-i0008",
            "message_author": "Harinder/PoppyBean",
            "text": "The Upload MSV and ViewPort (WordPress) BE MSV are separate. How will the Upload MSV write directly to the WordPress upload directory?",
            "timestamp": "2025-02-20T11:48:00Z"
        },
        {
            "message_id": "k8ororbit-252-20250220-i0009",
            "message_author": "Anna/BarbRock",
            "text": "The process can be structured as follows: 1) The Upload MSV receives the uploaded image. 2) It encodes the image data as base64. 3) It sends the base64 encoded image data to the ViewPort (WordPress) BE MSV. 4) The ViewPort (WordPress) BE MSV receives the base64 data and imports it into its 'wp-uploads' directory.",
            "timestamp": "2025-02-20T11:49:00Z"
        },
        {
            "message_id": "k8ororbit-252-20250220-i0010",
            "message_author": "Harinder/PoppyBean",
            "text": "So, the Upload MSV will directly communicate with the ViewPort (WordPress) BE MSV?",
            "timestamp": "2025-02-20T11:50:00Z"
        },
        {
            "message_id": "k8ororbit-252-20250220-i0011",
            "message_author": "Anna/BarbRock",
            "text": "Yes, or alternatively, the Frontend MSV could send the image directly to the ViewPort (WordPress) BE MSV.",
            "timestamp": "2025-02-20T11:51:00Z"
        },
        {
            "message_id": "k8ororbit-252-20250220-i0012",
            "message_author": "Harinder/PoppyBean",
            "text": "In that case, we can update the category and image details by having the Upload MSV communicate directly with the ViewPort (WordPress) BE MSV.",
            "timestamp": "2025-02-20T11:52:00Z"
        },
        {
            "message_id": "k8ororbit-252-20250220-i0013",
            "message_author": "Anna/BarbRock",
            "text": "Understood.",
            "timestamp": "2025-02-20T11:53:00Z"
        },
        {
            "message_id": "k8ororbit-252-20250220-i0014",
            "message_author": "Harinder/PoppyBean",
            "text": "The updated payload has been implemented. The Search, Viewport, and Upload MSVs have been pushed with the changes.",
            "timestamp": "2025-02-20T11:54:00Z"
        },
        {
            "message_id": "k8ororbit-252-20250220-i0015",
            "message_author": "Anna/BarbRock",
            "text": "Acknowledged.",
            "timestamp": "2025-02-20T11:55:00Z"
        }
    ]
}
```