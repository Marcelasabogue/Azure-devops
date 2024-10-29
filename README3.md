# Step 3: Application Deployment 🐵🙊🙉🙈

I selected a sample application developed in Elixir using the Phoenix framework. A dockerized image of the application was created, and the necessary configuration files were generated for deploying it on Kubernetes, including the dependent services. I ensured that the application was accessible after deployment, guaranteeing proper exposure and functionality in the Kubernetes environment.

[![App Deployment](./media/appdespl.png)](https://youtu.be/hQEI9WRtkIU)

*(Click the image above to view an illustrative video of the process)*

> **Note:** At the time of deployment, I decided to deploy a different application image (`aks-store-quickstart.yaml`). However, I will discuss the creation process of both. For this case, a sample application was deployed, but an Elixir application like the one created in the repository can also be utilized.

# Elixir Application 

To get the Phoenix application ready, I first installed Elixir (version 1.7) and Erlang by following the instructions on the Elixir installation page [Phoenix documentation](https://hexdocs.pm/phoenix/installation.html). Then, I installed Hex, the package manager for Elixir, by running `mix local.hex`. Next, I installed the Phoenix application generator with `mix archive.install hex phx_new`. I opted for PostgreSQL as the database, following the corresponding installation guides. With everything set up, I created my first application using `mix phx.new mono_app`, and finally, I entered the application directory and ran the server with `mix phx.server`, allowing me to access the application in the browser and start developing it.

([Repository of the Elixir application created](https://github.com/Marcelasabogue/AplicacionElixir))

I aimed to dockerize the application using the following files:
 - **.devcontainer**: Configures the development environment.
 - **Dockerfile**: Builds a custom container image.
 - **devcontainer.json**: Configures the development environment in a container for tools like VS Code.
 - **docker-compose.yml**: Defines and manages multi-container applications. Below are the functions of each:

## 1. **.devcontainer**

- This directory contains the necessary configuration for a containerized development environment.

## 2. **Dockerfile**

- This `Dockerfile` creates a Docker image for Elixir and Phoenix applications, using the base image of Elixir 1.13.1. It installs Node.js, Phoenix, and its dependencies, and configures an application directory. A non-root user is established for added security, and port 4000, which is the default port for Phoenix, is exposed. It also includes configurations for installing Yarn and allows the use of sudo without a password, if needed.

## 3. **devcontainer.json**
This `devcontainer.json` configuration file defines a development environment for a Phoenix application using Docker. It sets the environment name to "Docker Phoenix" and specifies that the `docker-compose.yml` file will be used to manage services. The main service is named "app" and sets the working directory to `/app`. Port 4000 is forwarded for external access, and upon closing the environment, Docker services are stopped. Additionally, it includes relevant Visual Studio Code extensions for Elixir, Phoenix, and Svelte. Finally, it configures an environment variable to enable history in the Erlang console.

## 4. **docker-compose.yml**
This `docker-compose.yml` file defines a multi-container environment for a Phoenix application and a PostgreSQL database. In the services section, it specifies:

1. **app**:
   - Builds the image using the Dockerfile located in `.devcontainer`.
   - Runs the command `sleep infinity` to keep the container running.
   - Sets environment variables for port and binding.
   - Forwards port 4000, allowing access to the application from outside the container.
   - Mounts the project directory to `/app` inside the container and integrates the user’s Git configuration to avoid reconfigurations.
   - Depends on the database service `db`.

2. **db**:
   - Uses the PostgreSQL 12 image and configures access credentials.
   - Exposes port 5432 and stores data in a persistent volume called `database`.
   - Finally, a volume named `database` is defined to ensure the persistence of database data.

# RabbitMQ Application - Microservices Application on Kubernetes

### Purpose
RabbitMQ acts as a messaging system that allows communication between services asynchronously. This is crucial for decoupling services, facilitating their scalability and maintenance.

### Implementation
- **Deployment**: A `Deployment` is defined that ensures there is always one instance of RabbitMQ running.
  - **Replicas**: Set to 1 for simplicity.
  - **Container**: Uses the RabbitMQ image with management tools.
  - **Ports**:
    - `5672`: For the AMQP protocol.
    - `15672`: For the HTTP management interface.
  - **Configurations**: Environment variables for default username and password are set.
  - **ConfigMap**: Enables plugins such as management and Prometheus.

### Service
- **Type**: `ClusterIP`, allowing other services to connect internally without exposing the port externally.

## 2. Order Service

### Purpose
This service handles operations related to orders. It communicates with RabbitMQ to send and receive messages about customer orders.

### Implementation
- **Deployment**: A `Deployment` is created for the order service.
  - **Container**: Uses a specific image that manages orders.
  - **Ports**: Exposes port `3000`.
  - **Environment Variables**: Configure the connection to RabbitMQ and other relevant parameters.
  - **Init Container**: Ensures that the order service starts only when RabbitMQ is available.

### Service
- **Type**: `ClusterIP`, exposing port `3000` for internal access.

## 3. Product Service

### Purpose
Manages product information, allowing other services and the frontend to query product details.

### Implementation
- **Deployment**: A `Deployment` is defined for the product service.
  - **Container**: Uses an image designed for managing products.
  - **Ports**: Exposes port `3002`.

### Service
- **Type**: `ClusterIP`, exposing port `3002` for internal access.

## 4. Store Front

### Purpose
The frontend service provides the user interface to interact with the orders and products system.

### Implementation
- **Deployment**: A `Deployment` is defined for the frontend.
  - **Container**: Uses an image that serves the frontend application.
  - **Ports**: Exposes port `8080`.
  - **Environment Variables**: Configure the URLs to connect to the order and product services.

### Service
- **Type**: `LoadBalancer`, which exposes port `80` externally, allowing users to access the application from the internet.

## Workflow

1. **Start RabbitMQ**: When the application is deployed, RabbitMQ starts first, ensuring that other services can connect to it.
2. **Start Order Service**: After RabbitMQ is operational, the order service starts, allowing order management.
3. **Start Product Service**: This service can operate independently but interacts with RabbitMQ as needed.
4. **Start Store Front**: Finally, the frontend is deployed and connects to the order and product services, providing an interface for users.


