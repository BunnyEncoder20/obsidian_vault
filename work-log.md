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
