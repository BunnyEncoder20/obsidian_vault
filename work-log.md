# Description

- This file contains all data on the works I have contributed in my job at L&T

# Format

## [YYYY-MM-DD] Feature/Project Name

**Context (The "Why"):**

- Briefly: Why was this needed? What was the pain point?
  - _e.g., The legacy approval system was timing out on large datasets._

**The Work (The "What"):**

- [ ] Implemented the `POST /approve` endpoint using [Tech Stack].
- [ ] Refactored the database schema to handle concurrent writes.
- [ ] Wrote unit tests covering edge cases X, Y, Z.

**The Impact (The "So What" - COLD HARD DATA):**

- **Metrics:** Reduced API response time from 1.2s to 200ms.
- **Business Value:** Enabled the sales team to process bulk orders without crashing the UI.
- **Efficiency:** Automated a manual SQL script, saving the team 2 hours per week.

**Problem/Conflict Resolved:**

- _e.g., Product wanted Feature A, but Engineering said it was impossible. I proposed Solution B which met 90% of requirements and shipped on time._

**Tags:** #optimization #backend #leadership #crisis-management

# Logs

**[05/09/25 09:16] varun@backend >>** Metadata info to iFrame - aded InfoName tag and Techname tag infromation to displayed name of xml doc file tkinter iframe in s1kd_PMC.py program (Advanced PM Content Editor).

**[18/09/25 15:11] varun@devops >>** Deployed the Taiga instance and Gitlab instance on self-hosted server

**[18/09/25 16:30] varun@backend >>** reinstalled PostgreSQL to reset default creds. Started work on `ietm-ts-search`.

```bash
# postgres password set temp
username: postgres
password: postgres
```

**[19/09/25 14:30] varun@backend >>** IETM: Fixed indexing and English analyzer (tokenization algorithm) of `ietm` data

**[19/09/25 15:45] varun@backend >>** IETM: Implemented Elastic Search features: fuzzy, Boolean, phrase and keyword. Rewrote entire Elastic Searching controller

**[20/09/25 09:40] varun@Infra >>** Researched Backup solutions and installed and successfully made snapshots and scheduled backups to remote NAS using [[Kopia]]

**[20/09/25 15:15] varun@Infra >>** Successfully installed [[Kopia]] Server CLI for centralized backups on LAN server (GPU) PC and created backup repo

**[22/09/25 17:05] varun@Infra >>** configured [[Kopia]] Server Service on Server and took backups 2 computers to centralized server. [[Kopia]] backups POC completed

**[23/09/25 12:03] varun@Documentation >>** wrote down Kopia Server Setup and policy creation in detail into a markdown.

**[23/09/25 17:10] varun@Documentation >>** Completed creating SOP for Kopia Server Setup.
CURRENT_DATE
**[24/09/25 10:30] varun@Documentation >>** Completed creating SOP for Kopia Client Setup.

**[24/09/25 17:20] varun@Infra >>** Created Docker Image for running Kopia Server as a container.

**[25/09/25 12:10] varun@Infra >>** Installed Linux (Pop!OS) on work machine. Windows 11 not suitable for development work.

**[01/10/25 10:15] varun@Infra >>** Restored broken bootloader of PC and installed Omarchy Linux successfully.

**[03/10/25 12:00] varun@Frontend >>** created ImageSideBar component for IETM project

**[03/10/25 17:05] varun@DevOps >>** containerized IETM project (both back-end (+postgres +elasticsearch), front-end )

**[04/10/25 10:00] varun@DevOps >>** created and debugged docker-entrypoint shell scripts for IETM Dockerization. All docker-compose services working correctly now.

**[04/10/25 16:13] varun@backend >>** built basic API structures in nest framework. Tested with postman REST API client

**[07/10/25 14:00] varun@Infra >>** deployed backup software kopia on 2 servers and 7 client machines at MD Dockyard office.

**[08/10/25 16:30] varun@backend >>** containerized lms-backend including core stack components: Nestjs, PostgreSQL and redis.

**[09/10/25 17:15] varun@backend >>** Integrated PrismaORM into docker-compose setup. It was NOT easy. generating client and running migrations still pending on image.

---

---

---

## [29-12-2025 16:42] Super-System-v1 | varun@backend

**Context:**

- The only way to show true backend work is through benchmarking and stress testing.
- To show the quality of backend work, I have implemented feature (log to console) the timing each controller takes to complete it's request cycle. These are shown as warnings for distinguishing from other server Logs

**The Work:**

- [x] Implemented common/interceptor/benchmark.interceptor.ts file
- [x] Declared it as global interceptor and added to main.ts
- [x] The logs are showing up as intended.

**The Impact:**

- **Metrics:** the times for each controller are not visible.
- **Business Value:** This is valuable to indentify slower/heavier api endpoints which would need optmizations in the future, or indentify poorly coded services.
- **Efficiency:** for individual devs metrics this would do. For stress testing, proper test scripts with help of autocannon need to be written.

**Problem/Conflict Resolved:**:

- Now, we would not be optmizing blindly and only work on those modules and services which are actually slow and need work.

**Tags:** #optimization #benchmark

## [07-01-2026 15:57] Super-System-v1 varun@backend

**Context:**

- FE had made huge progress while I was away on leave for a week.
- The actual backend was lacking the following major modules:
  - Content module
  - Media module
  - Search module (integrating with elasticsearch docker instance)
- The followingn modules were made to e read only and needed complete CRUD operations:
  - Systems module
  - Subsystems module
- The following modules needed to be made from scratch:
  - Manuals module
  - Chapter module
  - Data-modules module

**The Work:**

- [x] Implemented Content module for servering icn and other resources for the html (data-modules) raw content directly from the backend file system with file servering, streaming and security.
- [x] Implemented CRUD operations for systems and subsystems with proper guards and validatios pipes
- [x] Implemented Manuals, Chapters and Data-modules modules with complete CRUD operations.
- [x] Integrated Elasticsearch with Search module for IETM search functionality (4 modes for searching, highlights, filtering, bulk indexing, auto-init on module startup, Postgresql fallback)
- [x] Wrote spec files for each module, totalling 111 testes (all passing)

**The Impact:**

- **Content Module:** for file severing and security, featuring:
  - system-level media servering
  - path sanitization (directory traversal attack prevention)
  - MIME type detection for all media types
  - Range requests support for video/audio streaming
  - Cache control headers for better performance
  - Organization of subfolder validation checks
  - Datamodules content servering (by id and dmc)
  - HTML processing with image source rewriting
- **Search Module:** featuring:

  - ES 8.15.0 integration with docker
  - 4 search modes: fuzzy, phrase, boolean, keyword
  - postgresql fallback search
  - bulk indexing with queue system
  - Equipment code boosting (primary and secondary codes)
  - Search result highlighting
  - filtering by system, manual, type
  - Index status tracking
  - Auto-initialization on module startup

- **Media Module:** upload and management of media files:

  - Access-controlled media servering
  - Hierarchical permission checks
  - File upload with validation
  - Support for manual, datamodule and system level media files
  - MIME type detection
  - File cleanup on deletion

- **Metrics:**
  - Tests: 13 test suites, 1 skipped, 176 passed, 177 total
  - Performace: content servering < 200ms, search queries < 500ms
- **Business Value:** The backend is now feature complete for the MVP version of IETM Super System v1. The FE can now integrate and test the complete system.
- **Efficiency:** The backend is now in a state where binding can begin. Moreove, this also starts the path for writing tests for each module and service, as Test driven development is the way to go for high quality code.

**Problem/Conflict Resolved:**

- The backend was lagging behind due to FE using AI generated code. Being the only developer handling both backend and the databases (schema, migrations, etc), I was given only 4 weeks to bring the backend to a working (binding level) state (even though I had asked for 7-8 weeks for proper tested work). After 2 weeks of intense coding and working late nights, I was able to bring the backend to a state where binding can begin.
- Managers seem to know very little about actual software development and due to infunce of AI, keep unrealisitic deadlines. I had to manage expectations and deliver on time, even if it meant cutting corners on services and test coverage (which I plan to fix in the coming weeks).

**Tags:** #backend #crud #elasticsearch #tdd #testing #leadership
