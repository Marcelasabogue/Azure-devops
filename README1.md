# Paso 1: Pipeline de CI/CD 🐵🙊🙉🙈

Configuración de un pipeline en GitHub Actions que se ejecuta automáticamente al hacer `push` a la rama principal del repositorio. Este pipeline asegura la integración continua, permitiendo la verificación automática del código y la ejecución de pruebas para mantener la calidad del proyecto. 

Para la automatización se emplearon dos GitHub Actions, como se observa en la carpeta `.github/workflows`, donde están los archivos `AzureAKS.yml` y `AzureAKSMonitoring.yml`.

# Primer Pipeline `AzureAKS.yml`

Este pipeline se emplea para crear un clúster de Azure Kubernetes Service (AKS) utilizando Terraform. A continuación, se explica cada parte del archivo de configuración.

## Activación del Flujo de Trabajo

El flujo de trabajo se activa con un evento de `push` en el repositorio. Esto significa que cada vez que se realice un `push`, se ejecutará este pipeline.

## Permisos

Se establecen los permisos necesarios para el flujo de trabajo. Aquí se permite:

- **id-token:** escritura, para autenticarse con Azure.
- **contents:** lectura, para acceder al contenido del repositorio.

## Definición de Trabajos

El trabajo principal se denomina `AKS-Cluster-Deployment` y se ejecuta en un entorno de Ubuntu.

## Configuración Predeterminada

Se configuran los comandos para que se ejecuten en un directorio específico (`AKS`) utilizando el shell de Bash. 

## Pasos del Trabajo

1. **Checkout del Repositorio** Se utiliza la acción `checkout` para obtener el código del repositorio. Esto es esencial para que los siguientes pasos tengan acceso a los archivos del repositorio.

2. **Inicio de Sesión en Azure CLI** Se inicia sesión en Azure utilizando las credenciales almacenadas en secretos (en action secrets). Esto permite que el pipeline interactúe con los recursos de Azure.

3. **Configuración de Terraform** Se configura Terraform con la última versión y se utiliza un token de configuración de credenciales almacenado en los secretos. Esto prepara el entorno para la ejecución de comandos de Terraform.

4. **Inicialización de Terraform** Se inicializa el entorno de Terraform, lo que permite que se descarguen los proveedores y se configure el backend.

5. **Validación de Terraform** Se valida la configuración de Terraform para asegurarse de que no hay errores de sintaxis o de configuración.

6. **Plan de Terraform** Se genera un plan de ejecución de Terraform. Este paso muestra lo que se va a crear, modificar o eliminar. Si este paso falla, se continúa con un estado de error.

7. **Estado del Plan de Terraform** Si el paso anterior falla, se termina el flujo de trabajo con un error. Esto asegura que no se continúe con la aplicación si hay problemas en el plan.

8. **Aplicación de Terraform** Se aplican los cambios definidos en el plan de Terraform. Esto crea o actualiza la infraestructura en Azure según la configuración especificada.

9. **Salida de Terraform** Se obtienen los outputs de Terraform, que pueden ser utilizados en pasos posteriores o para verificar que la infraestructura se ha creado correctamente.

10. **Despliegue de la Aplicación** Se obtienen las credenciales del clúster AKS y se aplica la configuración de Kubernetes utilizando el archivo `AKSApp/aks-store-quickstart.yaml` . Esto despliega la aplicación en el clúster AKS recién creado.

# Segundo Pipeline `AzureAKSMonitoring.yml`
Este pipeline está diseñado para gestionar la infraestructura en Azure Kubernetes Service (AKS) utilizando Terraform y Helm. A continuación, se desglosan los pasos clave del proceso para la implementación y monitoreo automatizado:

## Descripción del Pipeline

### Trigger
- **Evento de disparo**: El pipeline se activa en cada `push` a la rama del repositorio.

### Permisos
- Se especifican permisos para escribir un token de ID y leer el contenido del repositorio.

### Jobs
- **Job Principal**: `AKS-Cluster-Monitoring`
  - Se ejecuta en un entorno de Ubuntu (`ubuntu-latest`).
  - Configuración predeterminada de shell y directorio de trabajo en `AKS`.

### Pasos del Job

1. **Checkout**:
   - Se utiliza la acción `actions/checkout@v3.1.0` para clonar el repositorio en el entorno de ejecución.

2. **Login a Azure CLI**:
   - Se autentica en Azure usando `azure/login@v1` con credenciales almacenadas en secretos.

3. **Configuración de Terraform**:
   - Se configura Terraform usando `hashicorp/setup-terraform@v2.0.2`, especificando la versión y el token de configuración.

4. **Inicialización de Terraform**: Se ejecuta `terraform init` para inicializar el entorno de trabajo.

5. **Validación de Terraform**: Se valida la configuración con `terraform validate` para asegurar que no haya errores en la definición.

6. **Plan de Terraform**: Se genera un plan de implementación con `terraform plan`. Este paso puede fallar sin detener el pipeline debido a `continue-on-error: true`.

7. **Verificación del Estado del Plan**: Si el plan falla, se finaliza el job con un código de error.

8. **Aplicación de Terraform**: Se aplica el plan de infraestructura con `terraform apply -auto-approve`, creando o modificando recursos en Azure.

9. **Salida de Terraform**: Se recuperan y muestran las salidas definidas en la configuración de Terraform usando `terraform output`.

10. **Habilitación de Monitoreo en AKS**:
    - Se obtienen las credenciales del clúster de AKS.
    - Se añade el repositorio de Helm para Prometheus.
    - Se actualizan los repositorios de Helm y se instala la pila de monitoreo de Prometheus en el namespace `monitoring`.

11. **Limpieza**: Se elimina la configuración del kubeconfig con `rm -rf ~/.kube` para evitar problemas de seguridad.


🐵🙊🙉🙈
