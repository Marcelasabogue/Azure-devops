# Step 2: Kubernetes Deployment 🐵🙊🙉🙈

For the implementation of Kubernetes clusters on Azure, I used Terraform to create several scripts that include the definition of a resource group with a randomly generated unique name, the configuration of the Kubernetes cluster with a node group and administrator credentials, and parameterization through variables to facilitate adjustments in the configuration. Additionally, I included outputs that expose key information such as certificates and credentials, and generated an SSH key pair for secure authentication. The provider configuration and the use of a remote backend for Terraform state ensure efficient and reproducible management of infrastructure as code.

Next, I will define the content and functionality of each script involved in the implementation. Each of these scripts plays a crucial role in creating and managing the infrastructure, ensuring that all components are properly configured and interoperable. I will break down the Terraform scripts used to define resources, handle credentials, and facilitate the deployment of the Kubernetes cluster on Azure.

### - `main.tf`

Automation for creating and configuring a Kubernetes environment on Azure, ensuring that all resources are configured properly and efficiently.

1. **Random Name Generation**: Uses the `random_pet` resource to generate a random suffix to be used in the name of the resource group and the Kubernetes cluster name.

2. **Resource Group Creation**: A resource group is created in the specified location, with a name that includes the generated random suffix.

3. **Kubernetes Cluster Configuration**: A Kubernetes cluster is defined that:
   - Uses the same location as the resource group.
   - Has a random name and DNS prefix.
   - Is assigned a system-managed identity to facilitate secure interaction with other Azure resources.

4. **Node Group Configuration**: A node group is specified with a defined virtual machine size and a number of nodes determined by variables.

5. **User Profile**: A Linux profile is established that includes a username and an SSH key for accessing the cluster.

6. **Network Configuration**: The network profile of the cluster is defined, specifying the network plugin and the SKU type of the load balancer.

### - `outputs.tf`

This script is responsible for exposing critical information about the Azure resources and the Kubernetes cluster configuration, ensuring that sensitive data is adequately protected.

1. **Resource Group Name**:
   - `resource_group_name`: Returns the name of the created resource group.

2. **Kubernetes Cluster Name**:
   - `kubernetes_cluster_name`: Provides the name of the created Kubernetes cluster.

3. **Certificates and Keys**:
   - `client_certificate`: Returns the client certificate for authentication, marked as sensitive to protect the information.
   - `client_key`: Provides the client key, also sensitive.
   - `cluster_ca_certificate`: Returns the cluster's certificate authority certificate, sensitive.
   - `cluster_password`: Provides the cluster password, marked as sensitive.
   - `cluster_username`: Returns the username to access the cluster, sensitive.
   - `host`: Provides the address of the cluster host, also sensitive.

4. **Kubernetes Configuration**:
   - `kube_config`: Returns the complete Kubernetes configuration (kubeconfig) in raw format, marked as sensitive.

### - `providers.tf`

This script sets up the Terraform environment to interact with Azure, defines the required versions of the providers, and configures remote state storage in Terraform Cloud.

1. **Terraform Version**:
   - It specifies that a version of Terraform greater than or equal to 1.0 is required.

2. **Required Providers**:
   - **azapi**: Provider for Azure API resources, with a version of approximately 1.5.
   - **azurerm**: Provider for Azure Resource Manager resources, with a version of approximately 3.0.
   - **random**: Provider for generating random resources, with a version of approximately 3.0.
   - **time**: Provider for managing time-related resources, with a specific version of 0.9.1.

3. **Remote Backend**:
   - Configures the remote backend in Terraform Cloud (`app.terraform.io`), belonging to the "Marcela" organization.
   - Specifies the workspace named "mono_devops" to store the infrastructure state.

4. **Azure Provider**:
   - Declares the `azurerm` provider and initializes it with default features.

### - `ssh.tf`

This script configures the generation and storage of an SSH key pair in Azure, facilitating its use for secure connections to cloud resources.

1. **Random Name Generation**:
   - The `random_pet` resource is used to create a unique name for the SSH key, with the prefix "ssh" and no separator.

2. **Action to Generate SSH Key Pair**:
   - The `azapi_resource_action` resource executes an action to generate an SSH key pair in Azure, specifying the resource type (`Microsoft.Compute/sshPublicKeys@2022-11-01`).
   - The `resource_id` refers to the ID of the resource where the SSH key will be stored.
   - The action is performed using the `POST` method, and the public and private key output values are exported.

3. **Creation of the SSH Key Resource**:
   - Declares the `azapi_resource` resource to create the public SSH key in Azure, using the randomly generated name and the resource group defined earlier.

4. **Script Output**:
   - Defines an output called `key_data`, which exports the value of the public key generated by the previous action.

### - `variables.tf`

This Terraform script defines several variables that are used to configure resources in Azure. These variables allow parameterization of the Azure resource configuration, facilitating the reuse and customization of the script according to the user's specific needs. Below is a description of each variable:

1. **`resource_group_location`**:
   - **Type**: `string`
   - **Default Value**: `"eastus"`
   - **Description**: Specifies the geographical location where the resource group will be created.

2. **`resource_group_name_prefix`**:
   - **Type**: `string`
   - **Default Value**: `"rg"`
   - **Description**: Prefix for the resource group name that combines with a random ID to ensure the name is unique in the Azure subscription.

3. **`node_count`**:
   - **Type**: `number`
   - **Default Value**: `2`
   - **Description**: Initial number of nodes for the node group in the Kubernetes cluster.

4. **`msi_id`**:
   - **Type**: `string`
   - **Default Value**: `null`
   - **Description**: ID of the Managed Service Identity. This value must be set if the example is run using the managed identity as an authentication method.

5. **`username`**:
   - **Type**: `string`
   - **Default Value**: `"azureadmin"`
   - **Description**: Administrator username for the new cluster.

![Services Creator](./media/servicios.png)

