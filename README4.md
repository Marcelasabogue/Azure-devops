# Paso 3: Despliegue de la aplicación 🐵🙊🙉🙈

Implementación de un sistema de monitoreo y notificación para el despliegue de la aplicación. Al finalizar el despliegue, se genera una notificación por correo electrónico utilizando Terraform. Además, la aplicación desplegada está configurada para exportar métricas, que son recolectadas y visualizadas mediante Grafana y Prometheus, asegurando un monitoreo efectivo del rendimiento y la disponibilidad de la aplicación. 



Para acceder al monitoreo de AKS, sigue estos pasos:

```bash
# 1. Inicia sesión en Azure
az login --use-device-code

# 2. Obtener las credenciales de tu clúster AKS
az aks get-credentials --resource-group "<rg>" --name "<nombre del clúster>"

# 3. Verificar los pods en el namespace de monitoreo
kubectl --namespace monitoring get pods -l "release=prometheus"

# 4. Redirigir el puerto para acceder a Prometheus
kubectl port-forward --namespace monitoring svc/prometheus-kube-prometheus-prometheus 9090

# 5. Redirigir el puerto para acceder a Grafana
kubectl port-forward --namespace monitoring svc/prometheus-grafana 8080:80

# 6. Credenciales de Grafana
# - Nombre de usuario: admin
# - Contraseña: password
```


[![Despliegue](\media\videografana.png)](https://youtu.be/rYGsFI3o6AY)

*(Haz clic en la imagen de arriba para ver un video ilustrativo del proceso)*