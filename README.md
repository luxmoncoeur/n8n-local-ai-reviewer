# AI Code Reviewer (n8n + Ollama)

An automated code review system utilizing n8n and Ollama to analyze GitHub Pull Requests.

## How It Works

1. **Trigger**: A GitHub Pull Request triggers a webhook event.
2. **Processing**: n8n captures the code diff via the provided URL.
3. **Analysis**: The code diff is processed by Ollama for evaluation (bugs, security, quality).
4. **Feedback**: AI posts a review comment directly on the GitHub PR.

## Tech Stack

* **n8n**: Workflow automation.
* **Ollama**: Local LLM execution.
* **GitHub Webhooks**: Event orchestration.
* **Cloudflare Tunnel**: Local server exposure.

## Setup Guide

### 1. Requirements

* Install [Docker Desktop](https://www.docker.com/products/docker-desktop/).
* Install [Ollama](https://ollama.com/).
* Install [cloudflared](https://www.google.com/search?q=https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/).

### 2. Installation (Docker)

Run the following command to install n8n with Asia/Manila timezone and data persistence:

```bash
docker run -d --name n8n -p 5678:5678 \
  -e GENERIC_TIMEZONE="Asia/Manila" \
  -e TZ="Asia/Manila" \
  -v ~/.n8n:/home/node/.n8n \
  docker.n8n.io/n8nio/n8n

```

### 3. n8n Workflow Configuration

1. Open n8n (`http://localhost:5678`).
2. **Import Workflow**: Click on the workflow menu and select "Import from File". Upload your `.json` workflow file.
3. **GitHub Credentials**:
* Create a GitHub Personal Access Token (PAT) with `repo` permissions.
* In n8n, go to Credentials and add your GitHub token.


4. **Ollama Connection**:
* If Ollama is running on your host machine, set the base URL in the n8n Ollama node to `http://host.docker.internal:11434`.


5. **Activate**: Toggle the workflow to "Active".

### 4. Running the Tunnel

Execute the following in your terminal to expose the local instance:
`npx cloudflared tunnel --url http://localhost:5678`

*Note: Update the Payload URL in GitHub Webhook settings if the tunnel URL changes.*

## Usage

1. Push code changes to a new branch and open a Pull Request.
2. The AI reviewer will automatically analyze the diff and post a comment on the PR.

## Limitations

* **Host Dependency**: Requires the host machine and Docker container to be running.
* **Dynamic URLs**: Free Cloudflare tunnels require manual GitHub webhook updates upon restart.
* **Hardware Load**: Local LLM processing is resource-intensive.

## Improvement Areas

* **Permanent Infrastructure**: Migrate to a VPS for 24/7 uptime and a static URL.
* **Context Awareness**: Expand analysis beyond diffs to include full repository context.
* **Alerts**: Implement error handling notifications for failed workflow executions.

## Sample Execution
<img width="436" height="621" alt="image" src="https://github.com/user-attachments/assets/0ec17744-5db4-45bc-a564-920a8a104dd9" />
