Below is a **detailed, step-by-step guide** to turn your central PC into a Kopia **Repository Server** on Windows, and to connect Windows / Linux clients. 
I include exact commands, recommended best practices (TLS, running as a service), and the client steps you’ll need. 
I’ll also note the common gotchas.
I’m citing Kopia docs and community guidance for the important commands and flags. ([kopia.io](https://kopia.io/docs/installation/?utm_source=chatgpt.com "Download & Installation"))

---

# Quick plan (what we’ll do)

1. Prepare the central PC (disk, account, firewall).
    
2. Install Kopia CLI on the central PC.
    
3. Create a repository (filesystem) on the central PC.
    
4. Add server users (one per client or per user@hostname).
    
5. Start Kopia **server** (generate TLS cert first run), capture the server cert fingerprint.
    
6. Make the server persistent (run as a service using NSSM or Task Scheduler).
    
7. On each client: connect KopiaUI or kopia CLI to the server repo, then create policies & schedule snapshots.
    

---

## A — Prereqs & decisions

- Central PC: Windows 10/11 or Windows Server with a large data disk (e.g., `D:\KopiaRepo`) reserved for backups.
    
- Admin access on the central PC to install binaries, open firewall ports and create services.
    
- Default Kopia server port: **51515** (you can choose another).
    
- Production recommendation: **use TLS** (self-signed is fine for LAN) — avoid `--insecure` in production. Kopia supports server mode and will proxy repository access so clients don't need repo credentials. ([kopia.io](https://kopia.io/docs/repository-server/?utm_source=chatgpt.com "Repository Server"))
    

---

## B — Install Kopia CLI on the central PC

1. Download the **Kopia CLI** binary for Windows from Kopia releases / installation page (do **not** use KopiaUI installer on the server — use the CLI binary). Example page: Kopia installation page / releases. ([kopia.io](https://kopia.io/docs/installation/?utm_source=chatgpt.com "Download & Installation"))
    
2. Unzip and place `kopia.exe` in a stable folder, e.g. `C:\Program Files\Kopia\kopia.exe`. Optionally add `C:\Program Files\Kopia` to PATH.
    

---

## C — Create the repository (filesystem backend)

Run this **from the account that will own the Kopia config** (see notes about config-file below):

```powershell
# open admin PowerShell in the folder where kopia.exe is
cd "C:\Program Files\Kopia"
.\kopia.exe repository create filesystem --path="D:\KopiaRepo" --description="DeptBackups"
```

- You will be prompted to set a repository password — this password encrypts the repository contents. Keep it safe.
    
- You can use `--create-only` to create without connecting, or other backends (S3, WebDAV, rclone) if you later change design. ([kopia.io](https://kopia.io/docs/reference/command-line/common/repository-create-filesystem/?utm_source=chatgpt.com "repository create filesystem"))
    

**Important**: Kopia stores a repository config file in the user profile by default (`%APPDATA%\kopia\repository.config`). If the server will run as a different Windows account (e.g., LocalSystem or a dedicated service account), either:

- Create the repo while logged in as that account, **or**
    
- Explicitly use `--config-file` when starting the server (examples below). See notes below about `--config-file`. ([kopia.io](https://kopia.io/docs/reference/command-line/?utm_source=chatgpt.com "Command Line"))
    

---

## D — Add server users (allow clients to connect)

On the server (same account/context that owns the repository config):

```powershell
# add a user for client "ALICE-PC" (username@hostname)
.\kopia.exe server users add alice@ALICE-PC
# you’ll be prompted to enter & confirm the password for alice@ALICE-PC
```

- Kopia identifies users as `username@hostname`. By default `hostname` is the client machine’s name. Add one user per client (or use a naming scheme you control). Clients must connect as that user (or use `--override-username`/`--override-hostname` when connecting). ([kopia.io](https://kopia.io/docs/reference/command-line/common/server-users-add/?utm_source=chatgpt.com "server users add"))
    

---

## E — Start the Kopia server (first run: generate TLS cert)

**First run**: generate a self-signed TLS cert and start the server (server prints the certificate fingerprint you’ll use from clients).

Example (PowerShell):

```powershell
# make a folder for certs/logs/config
New-Item -ItemType Directory -Path "C:\KopiaServer" -Force

# First-time start: generate a cert and start server, storing cert/key in C:\KopiaServer
.\kopia.exe server start `
  --address="https://0.0.0.0:51515" `
  --tls-generate-cert `
  --tls-cert-file="C:\KopiaServer\kopia.crt" `
  --tls-key-file="C:\KopiaServer\kopia.key" `
  --ui `
  --log-dir="C:\KopiaServer\logs" `
  --server-username="admin" `
  --server-password="(set-a-strong-password-here)"
```

Notes:

- `--tls-generate-cert` will cause Kopia to create a cert/key; **do not** pass `--tls-generate-cert` on subsequent restarts (it will fail if files already exist). The server will print a **SERVER CERT SHA256** fingerprint — copy that (you’ll use it on clients). ([kopia.io](https://kopia.io/docs/reference/command-line/common/server-start/?utm_source=chatgpt.com "server start"))
    
- `--ui` enables the web UI on the same port (`https://server-ip:51515`). Access from a browser as `https://<server-ip>:51515`.
    
- If you prefer to use an internal CA or real cert, generate/mount your certs and start server with `--tls-cert-file` and `--tls-key-file` (omit `--tls-generate-cert`). ([kopia.io](https://kopia.io/docs/reference/command-line/common/server-start/?utm_source=chatgpt.com "server start"))
    

---

## F — Make the server process persistent (recommended: NSSM)

`kopia server start` runs in the foreground. To run Kopia as a Windows service you can use **NSSM** (Non-Sucking Service Manager) which wraps any executable as a service.

1. Download NSSM: [https://nssm.cc/](https://nssm.cc/) (grab the appropriate zip). (community guidance also recommends this method). ([Kopia Forum](https://kopia.discourse.group/t/made-a-guide-on-how-to-deploy-kopia-linux-windows-server-mode-docker/2258?utm_source=chatgpt.com "Made a guide on how to deploy Kopia - linux, windows, server ..."))
    
2. Install the Kopia server as a service (example, run as LocalSystem — or create a dedicated service user and set its password in NSSM):
    

Example steps (PowerShell / command prompts):

```powershell
# unzip nssm and copy nssm.exe into your PATH or keep its path handy
# run (cmd as admin):
nssm install KopiaServer
# an NSSM GUI appears:
#   Path: C:\Program Files\Kopia\kopia.exe
#   Arguments: server start --address="https://0.0.0.0:51515" --tls-cert-file="C:\KopiaServer\kopia.crt" --tls-key-file="C:\KopiaServer\kopia.key" --ui --log-dir="C:\KopiaServer\logs" --server-username="admin" --server-password="(the-admin-pass)"
#   Startup Directory: C:\Program Files\Kopia
# In NSSM "Log on" tab you can select Local System or a specific account, and set output/log files on I/O tab.
# Then:
nssm start KopiaServer
```

Alternative: use a Scheduled Task that runs at system startup with highest privileges (works but NSSM gives a real Windows service). See Kopia forum threads where users use NSSM for persistent server instances. ([Kopia Forum](https://kopia.discourse.group/t/made-a-guide-on-how-to-deploy-kopia-linux-windows-server-mode-docker/2258?utm_source=chatgpt.com "Made a guide on how to deploy Kopia - linux, windows, server ..."))

**Important**: if you run the service under a different account than the one used to create the repository, use `--config-file` to point to that account’s Kopia config (or create the repository while logged into that account). See notes about `--config-file` above. ([Kopia Forum](https://kopia.discourse.group/t/how-to-specify-the-root-dir-for-kopia-server/1775?utm_source=chatgpt.com "How to specify the root dir for kopia server - General"))

---

## G — Open firewall port on server

Allow inbound TCP 51515 (or whatever port you used). Example PowerShell:

```powershell
New-NetFirewallRule -DisplayName "Kopia Server (51515)" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 51515
```

Also make sure any network routers, VLANs, or endpoint firewalls allow the traffic inside your LAN.

---

## H — Capture the server certificate fingerprint

When the server first starts with `--tls-generate-cert`, it prints a `SERVER CERT SHA256: <FINGERPRINT>`. Save that string. If you missed it, you can export the certificate file and compute the SHA256 fingerprint, or re-run generation on a test VM — community threads show this is how clients identify the server. ([kopia.io](https://kopia.io/docs/repository-server/?utm_source=chatgpt.com "Repository Server"))

---

## I — Connect a client (KopiaUI) to the server

On a client machine (Windows with KopiaUI installed), you can use the GUI or CLI.

**KopiaUI (GUI)**:

1. Open KopiaUI → Repositories → Connect → choose **"Repository Server"**.
    
2. Enter `https://<server-ip>:51515`, the **server username** and **password** you set for that client (the `username@hostname` you added on server), and the server cert fingerprint when prompted (if using self-signed). The UI will walk you through it. (Kopia GUI uses the `repository connect server` flow under the hood). ([kopia.io](https://kopia.io/docs/repositories/?utm_source=chatgpt.com "Repositories"))
    

**CLI example** (useful for scripting / mass deploy):

```powershell
# on client (kopia.exe installed)
# connect to server repository (replace values)
.\kopia.exe repository connect server `
  --url="https://192.168.1.10:51515" `
  --server-cert-fingerprint="8B9C6F54..." `
  --override-username=alice `
  --override-hostname=ALICE-PC
```

- After `repository connect server` you will be asked for the password for `alice@ALICE-PC`. Once connected, you can create snapshots (`kopia snapshot create --path C:\Users\alice\Documents`) or set policies for scheduled snapshots. ([kopia.io](https://kopia.io/docs/reference/command-line/common/repository-connect-server/?utm_source=chatgpt.com "repository connect server"))
    

---

## J — Scheduling snapshots on clients

- KopiaUI has built-in scheduling (you said you already used scheduling with shared folder mode). In server mode, scheduling is still done per-client (Kopia creates snapshots on the client).
    
- CLI example to create a snapshot now:
    

```powershell
.\kopia.exe snapshot create --path="C:\Users\alice\Documents"
```

- To automate: create a Windows Scheduled Task that runs KopiaUI or `kopia snapshot create` with the desired paths. You can also set Kopia policy for automatic snapshot intervals (see `kopia policy` commands in docs).
    

---

## K — Useful commands to check server & users (run on server)

```powershell
# list users
.\kopia.exe server users list

# server status (from server or remote)
.\kopia.exe server status --address="https://0.0.0.0:51515" --server-cert-fingerprint="<fingerprint>"

# refresh server to pick up changed user list (if you add users while server is running)
.\kopia.exe server refresh --address="https://localhost:51515" --server-control-username="admin" --server-control-password="(control-pass)"
```

(There are separate control credentials / server control username flags for server control operations; see docs.) ([kopia.io](https://kopia.io/docs/reference/command-line/common/server-status/?utm_source=chatgpt.com "server status"))

---

## L — Common gotchas & troubleshooting

- **Remember the fingerprint**: If you miss the fingerprint at first cert generation, it’s a pain — capture it on first run. ([kopia.io](https://kopia.io/docs/repository-server/?utm_source=chatgpt.com "Repository Server"))
    
- **Do not pass `--tls-generate-cert` on restarts** — only the first start should use it. ([kopia.io](https://kopia.io/docs/repository-server/?utm_source=chatgpt.com "Repository Server"))
    
- **Server config file location**: If the server runs as a different Windows account (LocalSystem), Kopia’s config and repository connection must be visible to that account (use `--config-file` or create the repo under the same account). Many forum posts show people having issues when service runs as different user. ([Kopia Forum](https://kopia.discourse.group/t/how-to-specify-the-root-dir-for-kopia-server/1775?utm_source=chatgpt.com "How to specify the root dir for kopia server - General"))
    
- **User identity**: Kopia users are `username@hostname`. Either add users matching the client computer names or use `--override-*` flags when clients connect. ([kopia.io](https://kopia.io/docs/repository-server/?utm_source=chatgpt.com "Repository Server"))
    
- **If the UI asks for different credentials**: you may need to use the `server-control` credentials for admin actions vs per-user credentials for repository access — read the server flags carefully. ([kopia.io](https://kopia.io/docs/reference/command-line/common/server-start/?utm_source=chatgpt.com "server start"))
    

---

## M — Minimal checklist for you to act on right now

-  Pick the central PC and a repository disk (e.g., `D:\KopiaRepo`).
    
-  Download Kopia CLI on server and KopiaUI on a test client. ([kopia.io](https://kopia.io/docs/installation/?utm_source=chatgpt.com "Download & Installation"))
    
-  Create filesystem repository on server (`kopia repository create filesystem --path=...`). ([kopia.io](https://kopia.io/docs/reference/command-line/common/repository-create-filesystem/?utm_source=chatgpt.com "repository create filesystem"))
    
-  Add a test user (`kopia server users add alice@ALICE-PC`). ([kopia.io](https://kopia.io/docs/reference/command-line/common/server-users-add/?utm_source=chatgpt.com "server users add"))
    
-  Start server first time with `--tls-generate-cert`, note the fingerprint. ([kopia.io](https://kopia.io/docs/reference/command-line/common/server-start/?utm_source=chatgpt.com "server start"))
    
-  Connect a client with KopiaUI using the server URL + fingerprint; test a snapshot and a restore. ([kopia.io](https://kopia.io/docs/reference/command-line/common/repository-connect-server/?utm_source=chatgpt.com "repository connect server"))
    

---

We can now:

- Generate the exact **NSSM arguments** and a ready-to-paste script for installing Kopia as a Windows service under NSSM (I’ll include handling of config-file and logs), **or**
    
- Produce a one-page **deployment checklist** and a small PowerShell script to pre-create the Kopia users and push a small client config (useful for mass roll-out).

---

# Kopia Server Setup Actually Done 

## 1. Creates the repository (filesystem)

```bash
kopia repository create filesystem --path "<path_to_repo>"
```
**NOTE:** *Kopia* will ask you to enter a `repository password` (required) note this password.

## 2. First time running Kopia Server
```bash
kopia server start 
--address=0.0.0.0:51515 
--server-username=<username> 
--server-password=<password> 
--tls-generate-cert 
--tls-cert-file "<path_to_cert_file>\server.cert" 
--tls-key-file "<path_to_key_file>\server.key"
```
**NOTE:** 
- *Kopia* will self generate a `tls-cert`. The secure server key and cert will stored into the designated file paths. 
- On first run, the server will also output it's `Trusted server certificate fingerprint (SHA256)` which needs to be saved and stored carefully. This is only printed once and will be needed every time the client UI is connected to this server repo.
- Also note down the server username and password, this is used to sign into the server web UI as a admin
## 3. Running Server for all other times
```bash
kopia server start 
--address=0.0.0.0:51515 
--server-username=<username> 
--server-password=<password> 
--tls-cert-file "<path_to_cert_file>\server.cert" 
--tls-key-file "<path_to_key_file>\server.key"
```
- Just the `--tls-generate-cert` is removed cause it's already generated and specified using the file paths.
- This is also the command specified in the [[NSSM]] service

## 4. Converting the Server into a Windows Service (which is persistent) through [[NSSM]]
```bash
nssm install <kopia_server_service_name>
```
- Params used:
	- path to kopia-server.exe
	- path to kopia-server folder
	- arguments: server start command (mentioned above)
	- Set user profile login of user who is running Kopia server
	- (Optional) Connected the Log file for server outputs
- Once the service is created we run it and make it a auto start as follows:
```bash
nssm start <kopia_server_service_name>
nssm set <kopia_server_service_name> Start SERVICE_AUTO_START
```
- Some other common commands used:
```bash
kopia server users list
kpoia snapshot list --all
kopia repository status
```
## 5. Adding users (client) creds to the server:
```bash
kopia server users add username@hostname_lowercase
```
- You will prompted to enter the password for the user
- The client will have to use this password while connecting to the repo through the client UI.
- **NOTE:** the username (who the client will be connecting as) cannot be changed so, make sure to enter the right `username@hostname_lowercase` 

---

# Kopia Policy Setup

- We want to minimize the infra footprint of each backup
- For this Kopia already comes with incremental backups (only changes data chunks are backup after the initial one), deduplication across clients and inbuilt compression algorithms.

If we set these global policy on the server UI as admin, we will be defining the **repository-wide default policy**. Any client connecting to will inherit these settings unless they have their own specific policy.
```bash
kopia policy set 
--global 
--keep-latest 3 
--keep-daily 0 
--keep-weekly 0 
--keep-monthly 0
--keep-annual 0 
--compression pgzip-best-compression
--schedule-interval 24h
```
Explanation:
- You would want to set the global policy itself so that all folder/file snapshots follow the same policy
- Though UI seems the max interval can only be set to 12 hours
- `PGZIP` gives the best compression-performance ratio out of all the compression algorithms.