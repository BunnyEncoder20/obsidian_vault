# Functional Requirements to Development Needs

### Legends

- BE: backend
- FE: frontend
- T01: Task 01

### FR-IETM-M01:

| **Department** | **Dev Task ID**                                                                                                                                   | **Developmental Task Summary**                                                                                                                                                                                                                                                     |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Backend**    | BE-IETM-01.1<br>BE-IETM-01.1.1<br>BE-IETM-01.1.2                                                                                                  | Design and implement an **XML-aware storage solution** (DB or Blob) for S1000D Data Modules.                                                                                                                                                                                       |
| **Backend**    | BE-IETM-01.2                                                                                                                                      | Develop the **API endpoints** to retrieve the full, structured XML content for display.                                                                                                                                                                                            |
| **Security**   | BE-IETM-01.3                                                                                                                                      | The **SOW** context implies this is proprietary data. Security must enforce who sees *what* content.<br><br>Implement and audit **Role-Based Access Control (RBAC)** to ensure only authorized users/roles can view or interact with specific IETM Data Modules (Level 4 content). |
| **Frontend**   | FE-IETM-01.1                                                                                                                                      | Implement the **XML parsing and transformation logic** (e.g., using XSLT or specialized JS libraries) to convert the S1000D XML into a compliant HTML structure.                                                                                                                   |
| **Frontend**   | FE-IETM-01.2                                                                                                                                      | Integrate core **IETM Level 4 interactivity** features, including dynamic table-of-contents generation and hyperlinking between data modules.                                                                                                                                      |
| **DevOps**     | Implement a **CI/CD Pipeline** to automate the build, test, and deployment of the IETM application.                                               | Ensures fast, repeatable, and error-free updates, which is essential for managing living technical documentation.                                                                                                                                                                  |
| **Security**   | Configure and manage a **Web Application Firewall (WAF)** to protect against common web attacks, especially targeting the XML/data parsing layer. | XML processing can be vulnerable to attacks like XML External Entity (XXE) injection, making WAF setup critical.                                                                                                                                                                   |

## FR-IETM-02

| **Req ID** | **Department** | **Task ID** | **Developmental Task Summary**                                                                                           |                                                                                                                        |
| ---------- | -------------- | ----------- | ------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| FR-IETM-02 | **DevOps**     | DO-02.1     | Deploy and manage the **STE-100 Validation Engine** or TMS instance within the production environment.                   |                                                                                                                        |
| FR-IETM-02 | **Security**   | SEC-02.1    | Ensure all authoring and compliance actions are recorded via an **Immutable Audit Log** to trace content accountability. | Hm, you know what, never mind this functional requirement seems like it will be taken care of with the authoring team. |

### FR-IETM-03

| **Req ID** | **Department** | **Task ID** | **Developmental Task Summary**                                                                                                                          |
| ---------- | -------------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR-IETM-03 | **Frontend**   | FE-03.1     | Design and implement the **Component Library and Style Guide** based on ISO/TR 9241-100 ergonomic principles (e.g., consistency, responsiveness).       |
| FR-IETM-03 | **Frontend**   | FE-03.2     | Conduct **Accessibility (WCAG) and Usability Audits** on key user flows (e.g., search, navigation) to ensure compliance with ergonomic standards.       |
| FR-IETM-03 | **Backend**    | BE-03.1     | **Optimize all API endpoints** for maximum response speed and minimum payload size, as UI performance is a core factor in ergonomics.                   |
| FR-IETM-03 | **Security**   | SEC-03.1    | Integrate and enforce **Content Security Policy (CSP)** headers to prevent injection attacks that could compromise the integrity of the user interface. |

## FR-IETM-04

| **Req ID** | **Department** | **Task ID** | **Developmental Task Summary**                                                                                                                                                      |
| ---------- | -------------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR-IETM-04 | **Backend**    | BE-04.1     | Develop the **Content Flow Logic Service** API to return conditional data modules based on user role (**RBAC**), system state, or prior user input.                                 |
| FR-IETM-04 | **Backend**    | BE-04.2     | Implement a **Dynamic Session State API** for reading/writing user progress within a procedural flow (e.g., saving steps completed in a maintenance task).                          |
| FR-IETM-04 | **Frontend**   | FE-04.1     | Design and implement the **Conditional Rendering Engine** to dynamically display content and prompt user interaction based on the BE Flow Logic.                                    |
| FR-IETM-04 | **Frontend**   | FE-04.2     | Implement **client-side state management** to track user inputs and progress, sending updates to the BE Session State API.                                                          |
| FR-IETM-04 | **DevOps**     | DO-04.1     | Configure and manage **Stateful Service Deployment** to ensure high availability and persistence for the Dynamic Session State API.                                                 |
| FR-IETM-04 | **Security**   | SEC-04.1    | Implement and verify **Fine-Grained Authorization** at the API layer, ensuring the "access enablement" logic prevents unauthorized retrieval of flow steps or restricted documents. |

## FR-IETM-05

| **Req ID** | **Department** | **Task ID** | **Developmental Task Summary**                                                                                                                                          |
| ---------- | -------------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR-IETM-05 | **Backend**    | BE-05.2     | Develop **Optimized Search and Navigation APIs** to efficiently traverse the hierarchy (e.g., "Find all manuals related to Engine Component X").                        |
| FR-IETM-05 | **Frontend**   | FE-05.1     | Implement a **Tree-View or Hierarchical Navigation Component** that visually represents the System-to-Manual structure for user browsing.                               |
| FR-IETM-05 | **Frontend**   | FE-05.2     | Integrate dynamic **Breadcrumb Trails** and **Filtering Logic** that reflect the user's current position within the macro hierarchy.                                    |
| FR-IETM-05 | **Security**   | SEC-05.1    | Enforce **Hierarchy-Based Access Control**, ensuring a user's role prevents them from even viewing system components or manuals above their authorized clearance level. |

## FR-IETM-06

| **Req ID** | **Department** | **Task ID** | **Developmental Task Summary**                                                                                                                             |
| ---------- | -------------- | ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR-IETM-06 | **Backend**    | BE-06.1     | Design and implement an **Authentication and Authorization Service** to handle the _cross-system login_ process.                                           |
| FR-IETM-06 | **Backend**    | BE-06.2     | Implement **API Gateway Authentication** to validate security tokens (like JWTs) for every request and enforce authenticated access to all data endpoints. |
| FR-IETM-06 | **Backend**    | BE-06.3     | Develop a service to retrieve and synchronize **User Roles and Permissions** from the central IdP upon successful login. (Late stage / Post deployment)    |
| FR-IETM-06 | **Frontend**   | FE-06.1     | Implement the **Login/Logout User Interface** and client-side logic to initiate and handle the SSO redirection flow securely.                              |
| FR-IETM-06 | **DevOps**     | DO-06.1     | Configure and manage secure **Secret Management** for API keys and certificates required for cross-system communication (e.g., IdP integration).           |

## FR-IETM-07

| **Req ID** | **Department** | **Task ID** | **Developmental Task Summary**                                                                                                                             |
| ---------- | -------------- | ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR-IETM-07 | **Frontend**   | FE-07.1     | Integrate a high-performance **2D Image Viewer Library** supporting deep zooming (e.g., using tiled images or SVG) to maintain **zoom clarity**.           |
| FR-IETM-07 | **Frontend**   | FE-07.2     | Implement a **3D Visualization Engine** (e.g., using WebGL, Three.js, or Babylon.js) to render interactive 3D models (exploded/orthographic views).        |
| FR-IETM-07 | **Security**   | SEC-07.1    | Implement **secure streaming** and **Digital Rights Management (DRM)** or watermarking to protect proprietary high-value technical graphics and 3D models. |

## FR-IETM-08

| **Req ID** | **Department** | **Task ID** | **Developmental Task Summary**                                                                                                                                                                               |
| ---------- | -------------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| FR-IETM-08 | **Frontend**   | FE-08.1     | Integrate a **Multimedia Player Component** capable of displaying synchronized 3D animations, voiceover audio, and supporting text/subtitles.                                                                |
| FR-IETM-08 | **Frontend**   | FE-08.2     | Develop logic for **Text/Screenshot Walk-throughs** that allow users to pause or step through the animation based on key procedure points.                                                                   |
| FR-IETM-08 | **Backend**    | BE-08.1     | Design and implement an **Animation Synchronization Service** to ensure the 3D model state, voiceover track, and text subtitles are delivered and played in lockstep. (What even is this file type? CMI5???) |
| FR-IETM-08 | **Backend**    | BE-08.2     | Define and implement a **Media Asset Storage Strategy** optimized for streaming large video/audio files (voiceovers) and 3D animation data. (Blob storage but how exactly)                                   |
| FR-IETM-08 | **DevOps**     | DO-08.1     | Configure **Media Streaming Endpoints** and ensure low-latency delivery using adaptive bitrate streaming (e.g., HLS/DASH) for video/audio assets.                                                            |
| FR-IETM-08 | **Security**   | SEC-08.1    | Apply security controls to **protect high-value training content**(animations/voiceovers) from unauthorized download or distribution via URL signing or DRM.                                                 |

## FR-IETM-09

| **Req ID** | **Department** | **Task ID** | **Developmental Task Summary**                                                                                                                               |
| ---------- | -------------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| FR-IETM-09 | **Frontend**   | FE-09.1     | Ensure **100% adherence to standard web technologies** (HTML5, CSS3, modern JavaScript) for all viewing and interactivity features.                          |
| FR-IETM-09 | **Frontend**   | FE-09.2     | Conduct **Cross-Browser Compatibility Testing** (especially on supported Windows browsers like Edge, Chrome, and Firefox) to guarantee consistent rendering. |
| FR-IETM-09 | **Backend**    | BE-09.2     | **Eliminate any dependency** on proprietary desktop runtime environments or browser plugins (e.g., Java Applets, Flash, ActiveX) in the server response.     |
| FR-IETM-09 | **DevOps**     | DO-09.1     | Configure **Automated Frontend Testing** in the CI/CD pipeline to flag any libraries or components known to rely on proprietary browser features or plugins. |
| FR-IETM-09 | **Security**   | SEC-09.1    | Audit all third-party libraries and code for any underlying reliance on proprietary viewers, as these often introduce **security vulnerabilities**.          |

## FR-IETM-10

| **Req ID** | **Department** | **Task ID** | **Developmental Task Summary**                                                                                                                                                             |
| ---------- | -------------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| FR-IETM-10 | **Backend**    | BE-10.1     | Design and implement the **Database Schema** for storing user notes, ensuring they link to the authenticated **User ID** (from FR-06) and the specific **Content/Manual ID** (from FR-05). |
| FR-IETM-10 | **Backend**    | BE-10.2     | Develop the dedicated **CRUD API Endpoints** (`POST`, `GET`, `PUT/PATCH`, `DELETE`) for managing user notes, including validation and input sanitization.                                  |
| FR-IETM-10 | **Frontend**   | FE-10.1     | Design and implement the **Note Taking UI Component** that allows users to create, view, and edit notes linked to specific sections of the IETM content.                                   |
| FR-IETM-10 | **Frontend**   | FE-10.2     | Implement **Client-side Logic** to securely send CRUD requests to the BE API and dynamically display/refresh notes without reloading the page.                                             |
| FR-IETM-10 | **DevOps**     | DO-10.1     | Integrate **Automated API Testing** into the CI/CD pipeline, specifically writing and running **CRUD test suites** against the BE-10.2 endpoints.                                          |
| FR-IETM-10 | **Backend**    | BE-10.3     | Implement **Owner-Based Access Control** on the server (API layer) to ensure a user can *only* Read, Update, or Delete notes they personally created.                                      |

## FR-IETM-11

PMC ???

## FR-IETM-12

| **Req ID** | **Department** | **Task ID** | **Developmental Task Summary**                                                                                                                                   |
| ---------- | -------------- | ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR-IETM-12 | **Frontend**   | FE-12.1     | Implement **Image Map or Hotspot Functionality** to overlay interactive regions on 2D graphics, linking them to specific content/procedures.                     |
| FR-IETM-12 | **Frontend**   | FE-12.2     | Develop **Interactive 3D Navigation Logic** allowing users to click a component in the 3D model (from FR-07) to instantly navigate to its dedicated manual page. |
| FR-IETM-12 | **DevOps**     | DO-12.1     | Integrate **Performance Monitoring** for graphic-heavy pages to ensure the addition of rich interactivity does not introduce significant latency.                |
| FR-IETM-12 | **Security**   | SEC-12.1    | Ensure that the navigation links retrieved via the Contextual Content API respect the user's **Access Control** rules (SEC-06.1) before redirecting the user.    |

## FR-IETM-13

| **Req ID** | **Department** | **Task ID** | **Developmental Task Summary**                                                                                                                                                   |
| ---------- | -------------- | ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR-IETM-13 | **Frontend**   | FE-13.1     | Enhance the **3D Visualization Engine** (from FE-07.2) to support **part manipulation**, including translation, rotation, and sequencing of components for assembly/disassembly. |
| FR-IETM-13 | **Frontend**   | FE-13.2     | Implement **User Input Controls** to interact with the 3D environment (e.g., drag-and-drop, step forward/backward through the procedure).                                        |
| FR-IETM-13 | **Backend**    | BE-13.2     | Implement a **Persistent State Tracking API** to record the user's progress and current state within the virtual procedure.                                                      |
| FR-IETM-13 | **Security**   | SEC-13.1    | Secure the **Procedure Logic Service (BE-13.1)** to prevent users from bypassing or manipulating the required order of steps via API calls.                                      |

## FR-IETM-14

| **Req ID** | **Department** | **Task ID** | **Developmental Task Summary**                                                                                                                                                |
| ---------- | -------------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR-IETM-14 | **Frontend**   | FE-14.1     | Design and implement the **Complete Navigation Set** (e.g., search, index, Table of Contents (TOC), hierarchy view) to ensure all content is accessible.                      |
| FR-IETM-14 | **Frontend**   | FE-14.2     | Implement **Client-side Event Tracking** to capture user navigation actions (clicks, page views, search queries) before sending them to the Backend.                          |
| FR-IETM-14 | **Backend**    | BE-14.1     | Design and implement the **Traversal Audit Log Service** and database schema to securely receive, timestamp, and store all navigation events and user details.                |
| FR-IETM-14 | **Backend**    | BE-14.2     | Develop an **Audit Reporting API** to allow authorized users to query and analyze user traversal data (e.g., to generate compliance reports).                                 |
| FR-IETM-14 | **DevOps**     | DO-14.1     | Implement and monitor the **High-Volume Log Ingestion Pipeline** to handle the constant stream of audit events from the Frontend and Backend without performance degradation. |
| FR-IETM-14 | **Security**   | SEC-14.1    | Enforce **Audit Log Immutability** and integrity checks to ensure traversal data cannot be tampered with, meeting high-security compliance standards.                         |

## FR-IETM-14

| **Req ID** | **Department** | **Task ID (Example)** | **Developmental Task Summary**                                                                                                                                     |
| ---------- | -------------- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| FR-IETM-15 | **Backend**    | BE-15.1               | Implement the **Server-Side PDF Generation Service** to transform IETM content (XML/HTML) into a structured PDF format.                                            |
| FR-IETM-15 | **Backend**    | BE-15.2               | Configure the PDF Generation Service to dynamically embed a **digital watermark**(e.g., username, date, or "CONFIDENTIAL") into the PDF output.                    |
| FR-IETM-15 | **Frontend**   | FE-15.1               | Design and implement the **Print/PDF Export UI** that includes options for watermarking parameters (if applicable) and initiates the server-side export.           |
| FR-IETM-15 | **Frontend**   | FE-15.2               | Implement a **Print-Specific CSS Stylesheet** to optimize the IETM content for readability and proper page breaks when a user uses the browser's "Print" function. |
| FR-IETM-15 | **DevOps**     | DO-15.1               | Deploy and maintain the **PDF Generation Libraries/Microservice** within a dedicated, high-availability environment due to high resource usage.                    |
| FR-IETM-15 | **Security**   | SEC-15.1              | Implement **PDF Security Controls** (e.g., password protection, disabling content copying/editing) and ensure the watermark is **non-removable**.                  |

## FR-IETM-16

| **Req ID** | **Department** | **Task ID (Example)** | **Developmental Task Summary**                                                                                                                          |
| ---------- | -------------- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR-IETM-16 | **Backend**    | BE-16.1               | Implement and integrate a **Dedicated Search Engine** (i.e. Elasticsearch) to support **Full-text** and **Boolean/Keyword** queries across all content. |
| FR-IETM-16 | **Backend**    | BE-16.2               | Develop the **Search History and Contextual API** to record user search queries and fetch previous results/contextual suggestions.                      |
| FR-IETM-16 | **Frontend**   | FE-16.1               | Design and implement the **Advanced Search UI** including input fields for Boolean logic (AND, OR, NOT) and filters for **cross-DB** context.           |
| FR-IETM-16 | **Frontend**   | FE-16.2               | Implement the **Search History Display** and user controls to easily re-run or clear past search queries.                                               |
| FR-IETM-16 | **DevOps**     | DO-16.1               | Deploy, configure, and manage the **Dedicated Search Engine Cluster** (BE-16.1), including index creation and content synchronization pipelines.        |
| FR-IETM-16 | **Security**   | SEC-16.1              | Ensure **Search Results Trimming** is enforced at the search engine level so that only content authorized for the current user (per FR-06) is returned. |

## FR-IETM-18

| **Req ID** | **Department** | **Task ID (Example)** | **Developmental Task Summary**                                                                                                                                                        |
| ---------- | -------------- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR-IETM-18 | **Frontend**   | FE-18.1               | Implement **Service Workers and Local Storage APIs** to cache all necessary IETM application assets (HTML, CSS, JS, images) for 100% offline access.                                  |
| FR-IETM-18 | **Frontend**   | FE-18.2               | Integrate an **Offline-First Data Strategy** to read directly from the local cache/database (from FR-17) without attempting to reach the server.                                      |
| FR-IETM-18 | **DevOps**     | DO-18.1               | Define and automate the **Update Distribution Mechanism** (e.g., physical media, intranet distribution point) for content and software updates without requiring the public internet. |
| FR-IETM-18 | **Security**   | SEC-18.1              | Implement and verify **certificate pinning** and **secure offline credential storage** to maintain authentication and integrity even without an internet check.                       |

## FR-IETM-19

| **Req ID** | **Department** | **Task ID (Example)** | **Developmental Task Summary**                                                                                                                                          |
| ---------- | -------------- | --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR-IETM-19 | **Backend**    | BE-19.1               | Define the **Audit Schema** for print events, mandating capture of **User Login, Timestamp, Printer Identifier,** and the **Document ID** being printed.                |
| FR-IETM-19 | **Backend**    | BE-19.2               | Develop a service to **integrate with the printing subsystem** (OS or network printing service) to securely capture and log the required printer metadata.              |
| FR-IETM-19 | **Backend**    | BE-19.1               | Enhance the **Traversal Audit Log Service (from FR-14)** to include the specific **Print Event API endpoint** for receiving and storing print counter data.             |
| FR-IETM-19 | **Backend**    | BE-19.2               | Develop a **Print Counter Retrieval API** for authorized users to query the total number of prints for specific documents or users.                                     |
| FR-IETM-19 | **Frontend**   | FE-19.1               | Ensure that all print actions initiated via the browser or the application trigger the necessary **Backend logging event** before the content is rendered for printing. |
| FR-IETM-19 | **DevOps**     | DO-19.1               | Configure **System Monitoring and Alerts** for suspicious printing activity (e.g., mass printing events or prints outside of regular hours).                            |

## FR-IETM-21:

| **Req ID** | **Department** | **Task ID (Example)** | **Developmental Task Summary**                                                                                                                       |
| ---------- | -------------- | --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR-IETM-21 | **Backend**    | BE-21.1               | Implement the **Natural Language Processing (NLP) Service** to interpret user input and map queries to IETM content IDs or specific procedures.      |
| FR-IETM-21 | **Backend**    | BE-21.2               | Develop the **Query Suggestion API** using algorithms (e.g., term frequency, user history) to return relevant suggested questions or document links. |
| FR-IETM-21 | **Frontend**   | FE-21.1               | Design and implement the **Chatbot UI/Widget** with input, conversation history, and a clear method for displaying suggested queries/answers.        |
| FR-IETM-21 | **Frontend**   | FE-21.2               | Implement **Real-time Input Logic** to call the Query Suggestion API (BE-21.2) as the user types, displaying suggestions dynamically.                |
| FR-IETM-21 | **DevOps**     | DO-21.1               | Deploy and manage the **NLP Model and Service** with sufficient computational resources for low-latency query processing.                            |
| FR-IETM-21 | **Security**   | SEC-21.1              | Ensure all chatbot interactions and conversational data are protected and, if stored, secured with the same **Audit Log** principles (FR-14).        |
