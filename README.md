
# 🐒 DevOps-Project -🐒

## Descripción del Desafío 🐒
 For this project, a CI/CD pipeline was created to deploy a Kubernetes cluster on Azure, using Infrastructure as Code (IaC) tools like Terraform. Grafana and Prometheus were utilized for application monitoring, and email notifications were implemented through Terraform. This pipeline also includes the deployment of a sample application developed with Elixir and Phoenix.
The process was broken down into specific steps, explaining the technical decisions made, the implementation carried out, and how each phase was executed. The relationship between the tools used in this project is presented in the following image.


![Arquitectura](./media/diagrama.png)


| Steps                                               | Description                                                                                                                                                                                                                                                                                  |
|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [1. CI/CD Pipeline:](README1.md)                |Configuration of a pipeline in GitHub Actions that automatically runs upon a push to the main branch of the repository. This pipeline ensures continuous integration, allowing for automatic code verification and test execution to maintain project quality.           |
| [2. Kubernetes Deployment:](README2.md)         | Creation of a Kubernetes cluster on Azure using Terraform as the Infrastructure as Code (IaC) tool. This approach allows for efficient and reproducible definition and provisioning of the cluster, facilitating cloud infrastructure management                     |
| [3. Application Deployment:](README3.md)      | A sample application was selected, but one in Elixir using the Phoenix framework can also be considered. A Dockerized image of the application was created, and the necessary configuration files were generated to deploy it on Kubernetes, including the dependent services               |
| [4. Monitoring/Notification of Application Deployment:](README4.md) |Implementation of a monitoring and notification system for the application deployment. Upon completion of the deployment, an email notification is generated using Terraform. Additionally, the deployed application is configured to export metrics, which are collected and visualized using Grafana and Prometheus, ensuring effective monitoring of the application's performance and availability |
