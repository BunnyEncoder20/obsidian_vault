# NocoDB for Project Management

## What is NocoDB? 
- At its core, NocoDB turns any SQL database (MySQL, Postgres, etc.) into a smart spreadsheet-like interface, with:
	- Views (Grid, Kanban, Gallery, Form)
	- Role-based sharing
	- REST & GraphQL APIs
	- Webhooks and automation triggers
- That makes it perfect for project tracking — like Airtable but open-source and self-hosted.

## How to use as Project Management Tool?
We can model your project as one or more Tables:

| Table                  | Purpose                 | Example Columns                                                                           |
| ---------------------- | ----------------------- | ----------------------------------------------------------------------------------------- |
| **Projects**           | Main project metadata   | name, description, repo_url, owner, status, priority, start_date, end_date                |
| **Tasks**              | Individual issues/tasks | title, description, status, assignee, due_date, github_issue_id, label, project_id (link) |
| **Sprints/Milestones** | Optional grouping       | name, start_date, end_date, progress                                                      |
| **Team Members**       | Users/owners            | name, role, email, github_username                                                        |
You can then:
- Use **Kanban view** for agile boards (e.g. “Todo / In Progress / Done”)
- Use **Form view** for adding new tasks quickly (Updates, Progress, Features, Issues)
- Add **computed fields** for tracking progress (% complete, delays, etc.)
- Share **filtered views** with specific team members

## Connecting NocoDB to a Github Repo
### Webhooks (Direct)
You can use **GitHub → NocoDB** webhooks.

*Example:*
You create a webhook in your GitHub repo pointing to NocoDB’s API endpoint that records an event.

*Use case:*
Whenever an issue is created or closed in GitHub:
- GitHub sends a webhook (JSON payload)
- You catch it in NocoDB (via API endpoint or via a small middleware like FastAPI or n8n)
- The middleware writes or updates the corresponding task in your Tasks table

**Webhook Flow:**
```bash
GitHub → webhook (POST) → FastAPI/n8n → NocoDB REST API (create/update record)
```
That gives you automatic syncing of Github issues with your NocoDB Task tracker