# Devops-pipeline

#### Overview

This project is a comprehensive DevOps pipeline designed to automate the build, test, and deployment process for a Java application. The pipeline is configured to use Jenkins and integrates several tools and technologies to ensure quality and efficiency throughout the CI/CD process.

#### Pipeline Stages

1. **Checkout**: Retrieves the latest code from the Git repository using specified credentials.

2. **Compile**: Compiles the Java application using Maven.

3. **Test**: Runs unit tests to ensure code quality.

4. **File System Scan**: Scans the file system for vulnerabilities using Trivy and generates a report.

5. **SonarQube Analysis**: Performs static code analysis using SonarQube to assess code quality and maintainability.

6. **Quality Gate**: Waits for SonarQube to evaluate the quality gate status and aborts the pipeline if the quality criteria are not met.

7. **Build**: Packages the application into a deployable artifact using Maven.

8. **Publish to Nexus**: Deploys the packaged artifact to a Nexus repository for artifact management.

9. **Build and Tag Docker Image**: Builds a Docker image from the application and tags it with the latest version.

10. **Docker Image Scan**: Scans the Docker image for vulnerabilities using Trivy and generates a report.

11. **Push Docker Image**: Pushes the Docker image to a Docker registry.

12. **Deploy to Kubernetes**: Applies the Kubernetes deployment configuration to update the application in the cluster.

#### Notifications

At the end of each build, an email notification is sent to the specified address with the build status and a link to the console output. The email includes an attachment of the Trivy image scan report.

#### Ansible Configuration

Server configuration, including setup and management of the Jenkins server ,SonarQube and Nexus, is handled using Ansible. This ensures consistent and repeatable infrastructure provisioning.

#### Prerequisites

- Jenkins with required plugins (e.g., SonarQube Scanner, Docker, Kubernetes CLI).
- Access to Git repository.
- Configured SonarQube server and Nexus repository.
- Docker registry credentials.
- Kubernetes cluster and access credentials.
- Ansible for server configuration.

#### Configuration

Ensure the following configurations are updated in your Jenkins pipeline:

- **Git Credentials**: `git-credentials`
- **SonarQube Server**: `sonar-qube-server`
- **SonarQube Token**: `sonar-token`
- **Maven Settings**: `global-setting`
- **Docker Registry Credentials**: `docker-cred`
- **Kubernetes Credentials**: `k8-cred`

#### How to Use

1. Update the Jenkins pipeline configuration with your project-specific details.
2. Ensure all required tools and credentials are properly configured.
3. Run the pipeline to execute the build, test, and deployment process.


## How to Run

1. Clone the repository
2. Open the project in your IDE of choice
3. Run the application
4. To use initial user data, use the following credentials.
  - username: bugs    |     password: bunny (user role)
  - username: daffy   |     password: duck  (manager role)
5. You can also sign-up as a new user and customize your role to play with the application! 😊

Feel free to adjust the pipeline stages and configurations to match your project’s specific needs.
