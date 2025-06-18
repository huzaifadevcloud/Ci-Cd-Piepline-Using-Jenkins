# Jenkins Overview

## What is Jenkins?
Jenkins is an open-source automation server widely used for implementing continuous integration (CI) and continuous delivery (CD) pipelines. It automates software building, testing, and deployment, making it easier for development teams to deliver high-quality software quickly and efficiently.

---

## Jenkins Architecture

Jenkins follows a **master-agent (controller-worker)** architecture:

### 1. Jenkins Controller (Master)
- **Role:** The central server that manages the entire Jenkins environment.
- **Responsibilities:**
  - Schedules and monitors jobs
  - Dispatches jobs to worker nodes (agents)
  - Manages build queues
  - Handles the web UI and REST API
  - Stores all job configurations and build results

### 2. Jenkins Agent (Worker)
- **Role:** Machine(s) that execute build jobs assigned by the controller.
- **Benefits:**
  - Runs jobs in parallel
  - Distributes workloads
  - Supports different environments (Linux, Windows, Docker, Kubernetes, etc.)

---

## How Master and Worker Nodes are Created

### Master Node Creation
- The master node (controller) is set up when Jenkins is installed.
- By default, the Jenkins server acts as the master and can also run jobs itself.

### Adding Worker (Agent) Nodes
- Additional worker nodes can be added to distribute build tasks.
- Common methods to connect agents:
  - **SSH:** The controller connects to the agent via SSH.
  - **JNLP (Java Web Start):** The agent initiates the connection to the controller.
  - **Cloud/Container-Based:** Using plugins (e.g., Kubernetes) for dynamic agent provisioning.

#### Steps to Add a Worker Node:
1. Go to **Jenkins Dashboard > Manage Jenkins > Manage Nodes and Clouds**.
2. Click **New Node** and enter a name.
3. Select **Permanent Agent**.
4. Configure:
   - Remote root directory (workspace)
   - Launch method (SSH, JNLP, etc.)
   - Labels (for targeting jobs)
5. Save and launch the agent.

---

## Summary Table

| Component     | Role                                               | Creation Method                 |
|---------------|----------------------------------------------------|---------------------------------|
| Controller    | Schedules jobs, manages UI, stores configs/results | Installed as Jenkins server     |
| Worker (Agent)| Executes builds and tests                          | Added via Jenkins UI or plugins |

---

## Key Features

- **Extensible:** Large plugin ecosystem for integrations and customizations.
- **Scalable:** Distributes builds across multiple nodes.
- **Cross-platform:** Runs on various OS and environments.
- **Easy Integration:** Works with Git, SVN, Docker, Kubernetes, and more.

---

## Useful Links

- [Jenkins Official Documentation](https://www.jenkins.io/doc/)
- [Jenkins GitHub Repository](https://github.com/jenkinsci/jenkins)
- [Jenkins Plugins](https://plugins.jenkins.io/)