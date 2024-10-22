# Paso 2: Despliegue de Kubernetes 🐵🙊🙉🙈

Creación de un clúster de Kubernetes en Azure utilizando Terraform como herramienta de infraestructura como código (IaC). Este enfoque permite definir y aprovisionar el clúster de manera eficiente y reproducible, facilitando la gestión de la infraestructura en la nube. 
# Azure_AKS_Labs
This repo contains Terraform code to create AKS and Specification file to deploy example application AKS.
https://youtu.be/hQEI9WRtkIU



#AKS Monitroing Accessibility
1. az login --use-device-code
2. az aks get-credentials --resource-group "<rg>" --name "<cluster name>"
3. kubectl --namespace monitoring get pods -l "release=prometheus"
4. kubectl port-forward --namespace monitoring svc/prometheus-kube-prometheus-prometheus 9090
5. kubectl port-forward --namespace monitoring svc/prometheus-grafana 8080:80
6. grafan user name : "admin" , password : "prom-operator"

🐵🙊🙉🙈