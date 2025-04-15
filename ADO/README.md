### An Azure DevOps Agent is a compute resource (VM, container, or physical server) that executes pipeline jobs (build, test, deploy). 

# There are two types:

## 1. Microsoft-hosted agents - Managed by Microsoft.

a . Auto-scaled, secure, pre-installed tools (e.g., Node, Python, Terraform).

b. Each pipeline run gets a new, clean VM, which is destroyed afterward.

c. No customization or persistence.

## 2. Self-hosted agents - You maintain the machine or container.

a. Persistent across builds.

b. Custom software/tools not available in Microsoft-hosted agents.

c. Faster builds (cached dependencies).

d. Access to private networks.

# Different Ways to Set Up Self-Hosted Agents: 

## 1: Agent on a Physical Server or VM (Host-based) - Install the agent directly on a Linux/Windows machine or EC2/VM. 

### Use When:
You want full control and persistent tooling.

You need to run builds that touch host resources.

You want to avoid Docker complexities.

## 2: Agent Running Inside a Docker Container - Run the agent as a Docker container. 

### Use When:
You want the agent environment to be reproducible and isolated.

Your team uses containers heavily.

Be sure to:
Mount /var/run/docker.sock if you want to run build containers from within the agent container.

Ensure required tools (e.g., Terraform, Ansible, CLI) are baked into the image.

## 3: Kubernetes-based Agents - Use scale-set agents or deploy agents as pods. 

### Use When:
You need to auto-scale agents on demand.

You're running DevOps pipelines inside a Kubernetes-native environment.

## 4: Agent with Docker-enabled Host Running Container Jobs - Run the agent directly on the host, and have it execute each job inside temporary containers (best of both worlds). 

### Use When:
You want persistent agent availability.

You want clean build environments per job (like Microsoft-hosted agents).

You need performance, flexibility, and isolation.

## Best Practice: Agent on Host + Build in Containers  - This is currently the most recommended setup for real-world DevOps pipelines. 

### Architecture:
Agent is installed once on the host machine.

Builds are executed in ephemeral containers using the container: keyword in your pipeline YAML.

Clean, isolated builds + persistent agent = fast & secure.

### Tools required on host:
Docker

Azure DevOps agent

System monitoring/logging (optional)

