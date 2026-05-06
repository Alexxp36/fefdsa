# TechRetail - Proyecto de Orquestación con Docker Swarm

**Autores:** Jamir Venturo Perez, Kevin Flores Trejo  
**Fecha:** 05 de mayo de 2026  
**Entorno:** Killercoda Cloud Engine / Docker v29.1.3

##  Resumen del Proyecto
Este repositorio contiene la solución de infraestructura para la empresa **TechRetail**. Se ha implementado un clúster de alta disponibilidad utilizando **Docker Swarm**, garantizando que los servicios críticos (Frontend, Backend, DB, Cache) sean tolerantes a fallos y escalables dinámicamente.

##  Guía de Ejecución

### 1. Preparación del Clúster
Inicializamos el nodo Manager:
```bash
docker swarm init --advertise-addr $(hostname -i)
```

### 2. Capa de Seguridad (Secrets)
Para proteger la integridad de los datos, implementamos **Docker Secrets**. Esto evita que las contraseñas de la base de datos estén expuestas en el código fuente, garantizando que solo los servicios autorizados tengan acceso:
```bash
echo "AdminTechRetail2026" | docker secret create db_password - 
```

### 3. Despliegue de la Arquitectura (Stack)
Lanzamos el stack completo que orquestará los servicios de frontend, backend, base de datos y caché:
```bash
docker stack deploy -c docker-compose.yml techretail
```

### 4. Monitoreo y Gestión Visual
Implementamos **Portainer CE** para supervisar la salud de los contenedores y el estado del clúster desde una interfaz gráfica profesional:
```bash
docker volume create portainer_data
docker service create \
--name techretail_portainer \
--publish 9000:9000 \
--constraint node.role==manager \
--mount type=bind,source=/var/run/docker.sock,target=/var/run/docker.sock \
--mount type=volume,source=portainer_data,target=/data \
portainer/portainer-ce:latest
```



## Acceso a los Servicios
Una vez desplegado el stack, se puede acceder a las distintas interfaces a través de los siguientes puertos:

* **Frontend Web:** `http://localhost:80` (Muestra la página de bienvenida de Nginx configurada).
* **Portainer CE:** `http://localhost:9000` (Panel de administración y monitoreo).
* **Visualizer:** `http://localhost:8080` (Mapa gráfico del clúster Swarm).



## 🛠️ Comandos Útiles de Gestión
Para administrar el ciclo de vida del stack, se pueden utilizar los siguientes comandos:

* **Ver logs de un servicio:** `docker service logs techretail_backend`
* **Eliminar el stack completo:** `docker stack rm techretail`
* **Eliminar el secreto:** `docker secret rm db_password`
* **Salir del modo Swarm:** `docker swarm leave --force`

## 👥 Autores
Proyecto desarrollado para el curso de **Arquitectura de Microservicios**:

* **Jamir Venturo Perez** - [GitHub Jamir](https://github.com/jamirventuro)
* **Kevin Flores Trejo** - [GitHub Kevin](https://github.com/kevinflores)

