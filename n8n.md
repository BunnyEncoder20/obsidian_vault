# n8n Automations

## 1. Running n8n
- Install as a docker image:
```bash

```
- Run using this command
```bash
docker run -it --rm --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n start --tunnel   
```
- The --tunnel flag allows self hosted version to send request to outside domains. Prefer using this.
- Maybe alias the command ?