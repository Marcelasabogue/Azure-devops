# Step 1: CI/CD Pipeline 🐵🙊🙉🙈
**Configuration of a pipeline in GitHub Actions that automatically runs upon a `push` to the main branch of the repository. This pipeline ensures continuous integration, allowing for automatic code verification and test execution to maintain project quality.**

For automation, two GitHub Actions were used, as observed in the `.github/workflows` folder, where the files `AzureAKS.yml` and `AzureAKSMonitoring.yml` are located.

# First Pipeline `AzureAKS.yml`

This pipeline is used to create an Azure Kubernetes Service (AKS) cluster using Terraform. Below, each part of the configuration file is explained.

## Workflow Trigger

The workflow is triggered by a `push` event in the repository. This means that every time a `push` is made, this pipeline will run.

## Permissions

The necessary permissions for the workflow are established. Here, the following is allowed:

- **id-token:** write, to authenticate with Azure.
- **contents:** read, to access the repository content.

## Job Definition

The main job is called `AKS-Cluster-Deployment` and runs in an Ubuntu environment.

## Default Settings

Commands are configured to run in a specific directory (`AKS`) using the Bash shell.

## Job Steps

1. **Checkout the Repository**: The `checkout` action is used to obtain the repository code. This is essential for subsequent steps to access the repository files.

2. **Login to Azure CLI**: Login to Azure is performed using credentials stored in secrets (in action secrets). This allows the pipeline to interact with Azure resources.

3. **Terraform Configuration**: Terraform is configured with the latest version, and a credentials configuration token stored in secrets is used. This prepares the environment for executing Terraform commands.

4. **Terraform Initialization**: The Terraform environment is initialized, allowing providers to be downloaded and the backend to be configured.

5. **Terraform Validation**: The Terraform configuration is validated to ensure there are no syntax or configuration errors.

6. **Terraform Plan**: A Terraform execution plan is generated. This step shows what will be created, modified, or deleted. If this step fails, it continues with an error state.

7. **Terraform Plan State**: If the previous step fails, the workflow ends with an error. This ensures that the application does not proceed if there are issues in the plan.

8. **Terraform Apply**: The changes defined in the Terraform plan are applied. This creates or updates the infrastructure in Azure according to the specified configuration.

9. **Terraform Output**: The outputs of Terraform are obtained, which can be used in subsequent steps or to verify that the infrastructure was created correctly.

10. **Application Deployment**: The AKS cluster credentials are obtained, and the Kubernetes configuration is applied using the file `AKSApp/aks-store-quickstart.yaml`. This deploys the application to the newly created AKS cluster.

# Second Pipeline `AzureAKSMonitoring.yml`

This pipeline is designed to manage infrastructure in Azure Kubernetes Service (AKS) using Terraform and Helm. Below, the key steps of the process for automated implementation and monitoring are outlined:

## Pipeline Description

### Trigger
- **Trigger Event**: The pipeline is activated on each `push` to the branch of the repository.

### Permissions
- Permissions are specified to write an ID token and read the repository content.

### Jobs
- **Main Job**: `AKS-Cluster-Monitoring`
  - Runs in an Ubuntu environment (`ubuntu-latest`).
  - Default shell configuration and working directory set to `AKS`.

### Job Steps

1. **Checkout**:
   - The action `actions/checkout@v3.1.0` is used to clone the repository in the execution environment.

2. **Login to Azure CLI**:
   - Authenticates in Azure using `azure/login@v1` with credentials stored in secrets.

3. **Terraform Configuration**:
   - Terraform is configured using `hashicorp/setup-terraform@v2.0.2`, specifying the version and configuration token.

4. **Terraform Initialization**: `terraform init` is executed to initialize the working environment.

5. **Terraform Validation**: The configuration is validated with `terraform validate` to ensure there are no errors in the definition.

6. **Terraform Plan**: An implementation plan is generated with `terraform plan`. This step can fail without stopping the pipeline due to `continue-on-error: true`.

7. **Plan State Verification**: If the plan fails, the job is finalized with an error code.

8. **Terraform Apply**: The infrastructure plan is applied with `terraform apply -auto-approve`, creating or modifying resources in Azure.

9. **Terraform Output**: The outputs defined in the Terraform configuration are retrieved and displayed using `terraform output`.

10. **Enable Monitoring in AKS**:
    - The AKS cluster credentials are obtained.
    - The Helm repository for Prometheus is added.
    - Helm repositories are updated, and the Prometheus monitoring stack is installed in the `monitoring` namespace.

11. **Cleanup**: The kubeconfig configuration is removed with `rm -rf ~/.kube` to avoid security issues.



🐵🙊🙉🙈
