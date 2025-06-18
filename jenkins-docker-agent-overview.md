# Using Docker as a Jenkins Agent (Worker Node)

## Pros of Using Docker as a Jenkins Agent

- **Isolation:** Every build runs inside its own Docker container, ensuring one build doesn't interfere with another.
- **Consistency:** The build environment is defined by the Docker image, guaranteeing the same setup across all builds and environments.
- **Clean Builds:** Containers are removed after each build, preventing leftover files or dependencies from affecting future builds.
- **Efficiency:** Containers are lightweight and start quickly, using fewer resources compared to virtual machines.
- **Scalability:** Jenkins can spin up multiple containers in parallel, enabling more builds and dynamic scaling.
- **Easy Dependency Management:** All dependencies are included in the Docker image, so the host system remains clean.

---

## Jenkins Architecture with Docker Agents

When using Docker agents, the Jenkins architecture looks like this:

```
+-------------------+        +----------------------+
| Jenkins Controller|<------>|    Docker Host       |
|   (Master)        |        | (can be same as      |
+-------------------+        |  controller or remote)|
                             +-----------+----------+
                                         |
                                         |
                             +-----------+----------+
                             |  Docker Container(s) |
                             |   (Jenkins Agents)   |
                             +----------------------+
```

- **Jenkins Controller**: Orchestrates builds, schedules jobs, and manages agent provisioning.
- **Docker Host**: Runs Docker and is responsible for creating containers for builds.
- **Docker Containers (Agents)**: Each container acts as an isolated Jenkins agent for running a specific build.

---

## How to Set Up Docker as a Jenkins Agent

### 1. Prerequisites

- Jenkins installed (controller/master)
- Docker installed on the Jenkins server or a remote host
- Jenkins plugins: **Docker** and/or **Docker Pipeline**

### 2. Configure Jenkins to Use Docker Agents

#### **A. Using Jenkins Pipeline (Declarative Syntax)**
In your `Jenkinsfile`, specify a Docker image for your agent:

```groovy
pipeline {
    agent {
        docker { image 'node:18-alpine' }
    }
    stages {
        stage('Test') {
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }
    }
}
```

- Jenkins will start a new container from the `node:18-alpine` image for the duration of the pipeline.

#### **B. Using the Docker Plugin (Cloud Agents)**
1. **Install the Docker plugin** in Jenkins.
2. Go to **Manage Jenkins > Manage Nodes and Clouds > Configure Clouds > Add New Cloud > Docker**.
3. Configure the Docker host URI (e.g., `unix:///var/run/docker.sock` for local, or `tcp://host:2375` for remote).
4. Define Docker agent templates:
   - Specify Docker image (e.g., `maven:3.8.3-jdk-11`)
   - Set remote filesystem, labels, environment variables, etc.
5. Jenkins will provision a container as a build agent whenever a job matching the label runs.

---

## Summary Table

| Pros                     | Architecture Change                              | How to Use                                       |
|--------------------------|--------------------------------------------------|--------------------------------------------------|
| Isolation, consistency,  | Builds run in ephemeral Docker containers        | Configure Docker plugin or use Docker in pipeline|
| clean builds, scalability| Jenkins controller orchestrates container agents | Specify agent in Jenkinsfile or cloud config     |

---

## References

- [Jenkins Docker Plugin Documentation](https://plugins.jenkins.io/docker-plugin/)
- [Jenkins Pipeline: Using Docker](https://www.jenkins.io/doc/book/pipeline/docker/)
- [Jenkins: Using Docker as agents](https://www.jenkins.io/doc/book/using/using-agents/#using-docker-as-agents)