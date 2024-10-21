# 🐒 Junior-DevOps-Engineer-Challenge -🐒
Mono es una plataforma tecnológica que permite a las empresas lanzar productos fintech y mover dinero fácilmente a través de Latam. A medida que Mono continúa creciendo, estamos en la búsqueda de los mejores y más brillantes talentos, como Marcela Sabogal, para construir el futuro de la banca empresarial.

<img src="https://latamlist.com/wp-content/uploads/2023/08/mono.png" alt="Logo mono" width="300"/>

## Desafío 🐒

Para este desafío se creó un pipeline de CI/CD que permite desplegar un clúster de Kubernetes en un proveedor de nube, utilizando una herramienta de Infrastructure as Code (IaC), seguido del despliegue de una aplicación de ejemplo. Para abordar este desafío, el proceso se descompuso en pasos específicos. En cada uno de estos pasos, se explican las decisiones técnicas consideradas, la implementación realizada y cómo se ejecutó cada fase.


## Abordaje del Desafío 🐒

1. **Paso 1: Despliegue de la aplicación**
   - **🙉 Decisiones técnicas**: Se eligieron Elixir y Erlang para asegurar un rendimiento adecuado de la aplicación, se utilizó Hex para gestionar dependencias, se optó por PostgreSQL como base de datos recomendada, y se integró Ecto para facilitar el manejo de bases de datos en la aplicación.
     
   - **🙈 Implementación**: 
   - **🙊 Ejecución**: Para tener la aplicación Phoenix lista, primero instalé Elixir (versión 1.7) y Erlang siguiendo las instrucciones de la página de instalación de Elixir [Phoenix documentación](https://hexdocs.pm/phoenix/installation.html). Luego, instalé Hex, el gestor de paquetes de Elixir, ejecutando `mix local.hex`. A continuación, instalé el generador de aplicaciones de Phoenix con `mix archive.install hex phx_new`. Opté por PostgreSQL como base de datos, siguiendo las guías de instalación correspondientes. Con todo esto listo, creé mi primera aplicación usando `mix phx.new mono_app`, y finalmente, entré en el directorio de la aplicación y ejecuté el servidor con mix phx.server, lo que me permitió acceder a la aplicación en el navegador y comenzar a desarrollarla.


2. **Paso 2: [Nombre del paso]**
   - **🙉 Decisiones técnicas**: [Descripción de las decisiones]
   - **🙈 Implementación**: [Descripción de la implementación]
   - **🙊 Ejecución**: [Descripción de cómo se ejecutó]

3. **Paso 3: [Nombre del paso]**
   - **🙉 Decisiones técnicas**: [Descripción de las decisiones]
   - **🙈 Implementación**: [Descripción de la implementación]
   - **🙊 Ejecución**: [Descripción de cómo se ejecutó]
     
 🙉  Decisiones tecnicas: Para la aplicacion se selecciono 
 🙈 Implementación: 
 🙊 ¿Cómo se hizo?

🙉 Decisiones técnicas
Elección de Elixir: Explica por qué seleccionaste Elixir, destacando su capacidad para manejar concurrencia y escalabilidad, su fiabilidad y la claridad de su sintaxis.
🙈 Implementación
Descripción del proceso de implementación: Aquí puedes incluir cómo se llevó a cabo la construcción de la aplicación, mencionando herramientas y metodologías utilizadas.
🙊 ¿Cómo se hizo?
Detalles específicos: Describe los pasos concretos que seguiste en el desarrollo, incluyendo cualquier desafío encontrado y cómo los resolviste.



## Requisitos

1. **Pipeline de CI/CD:**
   - Configurar un pipeline utilizando una herramienta de CI/CD de tu elección (e.g., GitHub Actions, GitLab CI, Jenkins).
   - Asegurar que el pipeline se ejecute automáticamente al hacer push a la rama principal.

2. **Despliegue de Kubernetes:**
   - Crear un clúster de Kubernetes en cualquier proveedor de nube (e.g., AWS, GCP, Azure).
   - Utilizar alguna herramienta de IaC (e.g., Terraform, Pulumi, Ansible) para definir y aprovisionar el clúster.

3. **Despliegue de la aplicación:**
   - Seleccionar una aplicación de ejemplo y crear los archivos de configuración necesarios para desplegarla en Kubernetes (ideal si la aplicación está en Elixir), como los servicios dependientes de esta.
   - Asegurar que la aplicación sea accesible después del despliegue.

4. **Monitoreo/Notificación del despliegue de la aplicación:**
   - Al terminar el despliegue, se debe generar una notificación del estado del despliegue. Puede utilizar cualquier tipo de notificación (e.g., email, SMS, push).
   - La aplicación desplegada debe poder exportar métricas para ser consultadas por alguna herramienta (e.g., Grafana, Elasticsearch, Prometheus).

5. **Documentación:**
   - Proveer un `README` con instrucciones para reproducir el despliegue.
   - Incluir una explicación de las decisiones técnicas tomadas.

## Entregables

1. **Repositorio privado:**
   - Crear un repositorio privado en una plataforma de control de versiones (e.g., GitHub, GitLab).
   - Invitar a `sebas@cuentamono.com` y `mpardo@cuentamono.com` al repositorio.

2. **Código y archivos de configuración:**
   - Incluir los archivos de configuración del pipeline de CI/CD y de Kubernetes.
   - Incluir los archivos de configuración de IaC utilizados para aprovisionar el clúster.
   - Incluir cualquier script adicional utilizado para la automatización.

3. **Documentación:**
   - Proveer instrucciones detalladas en un archivo `README`.
   - Explicar las herramientas, tecnologías y decisiones técnicas empleadas.

## Criterios de evaluación

- Calidad y claridad del pipeline de CI/CD.
- Efectividad del despliegue del clúster de Kubernetes y recursos utilizando IaC.
- Implementación y despliegue correcto de la aplicación.
- Calidad y claridad de la documentación.
- Capacidad de explicar el proceso y las decisiones técnicas.

🙈	Mono con los ojos cerrados	:see_no_evil:
🙉	Mono con los oídos tapados	:hear_no_evil:
🙊	Mono con la boca cerrada	:speak_no_evil:
🐵	Cara de mono	:monkey_face:





# Azure_AKS_Labs
This repo contains Terraform code to create AKS and Specification file to deploy example application AKS.


#AKS Monitroing Accessibility
1. az login --use-device-code
2. az aks get-credentials --resource-group "<rg>" --name "<cluster name>"
3. kubectl --namespace monitoring get pods -l "release=prometheus"
4. kubectl port-forward --namespace monitoring svc/prometheus-kube-prometheus-prometheus 9090
5. kubectl port-forward --namespace monitoring svc/prometheus-grafana 8080:80
6. grafan user name : "admin" , password : "prom-operator"
