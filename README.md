# Tienda de Alimentos para Perritos 🐶 - Proyecto DevOps

Este repositorio contiene el código fuente y la configuración de infraestructura para una aplicación web Full-Stack (CRUD de productos) orientada a una tienda de alimentos para mascotas. 

El proyecto está diseñado con un enfoque **DevOps**, separando sus componentes en microservicios, containerizando las aplicaciones mediante **Docker**, e incluyendo manifiestos de orquestación para **Kubernetes (K8s)**.

## 🚀 Arquitectura del Proyecto

El proyecto está dividido en tres capas principales (y un directorio de infraestructura):

*   **`frontend/`**: Interfaz de usuario (UI) construida con HTML, CSS y JavaScript Vanilla. Consume la API REST del backend. Está preparado para ser servido con Nginx usando un contenedor de Docker.
*   **`backend/`**: API RESTful construida con **Node.js** y **Express**. Maneja la lógica de negocio y se conecta a la base de datos para realizar las operaciones CRUD (Crear, Leer, Actualizar, Eliminar).
*   **`db/`**: Base de datos **MySQL**. Contiene el script de inicialización (`init.sql`) que crea la base de datos `tienda_perritos`, la tabla `productos` y la puebla con datos iniciales (semilla).
*   **`k8s/`**: Manifiestos de **Kubernetes** para desplegar la aplicación en un cluster. Incluye configuraciones de *Deployments*, *Services*, *Secrets* y *Horizontal Pod Autoscalers (HPA)* para escalabilidad automática.

## 🛠️ Tecnologías Utilizadas

*   **Frontend:** HTML5, CSS3, JavaScript.
*   **Backend:** Node.js, Express, Jest (para pruebas).
*   **Base de Datos:** MySQL.
*   **Contenedores:** Docker, Docker Compose (implícito en la arquitectura).
*   **Orquestación:** Kubernetes (Manifiestos YAML).
*   **CI/CD:** Preparado para integrarse mediante flujos de trabajo (contiene el directorio `.github/` para GitHub Actions).

## 📂 Estructura de Directorios

```text
ev3_devops/
├── backend/            # API de Node.js + Express
│   ├── tests/          # Pruebas unitarias
│   ├── server.js       # Punto de entrada del backend
│   ├── package.json    # Dependencias de Node
│   └── Dockerfile      # Configuración de imagen Docker para el backend
├── db/                 # Configuración de Base de Datos
│   ├── init.sql        # Script de creación e inserción de datos iniciales
│   └── Dockerfile      # Configuración de imagen Docker para MySQL
├── frontend/           # Aplicación cliente
│   ├── app.js          # Lógica JavaScript del frontend
│   ├── index.html      # Estructura principal
│   ├── default.conf    # Configuración de Nginx
│   └── Dockerfile      # Configuración de imagen Docker para el frontend (Nginx)
└── k8s/                # Configuración de Kubernetes
    ├── namespace.yaml
    ├── mysql-deployment.yaml, mysql-service.yaml, mysql-secret.yaml
    ├── backend-deployment.yaml, backend-service.yaml, backend-hpa.yaml
    └── frontend-deployment.yaml, frontend-service.yaml, frontend-hpa.yaml
```

## ⚙️ Cómo empezar

### Requisitos Previos
- Docker y Docker Compose instalados.
- Un clúster de Kubernetes (como Minikube, kind o un cluster en la nube) y `kubectl` configurado si deseas desplegar en K8s.

### Despliegue en Kubernetes
1. Aplica el namespace:
   ```bash
   kubectl apply -f k8s/namespace.yaml
   ```
2. Aplica los secretos y configuraciones de BD:
   ```bash
   kubectl apply -f k8s/mysql-secret.yaml
   kubectl apply -f k8s/mysql-deployment.yaml
   kubectl apply -f k8s/mysql-service.yaml
   ```
3. Despliega el backend y su servicio:
   ```bash
   kubectl apply -f k8s/backend-deployment.yaml
   kubectl apply -f k8s/backend-service.yaml
   kubectl apply -f k8s/backend-hpa.yaml
   ```
4. Despliega el frontend y su servicio:
   ```bash
   kubectl apply -f k8s/frontend-deployment.yaml
   kubectl apply -f k8s/frontend-service.yaml
   kubectl apply -f k8s/frontend-hpa.yaml
   ```

Una vez que todos los pods estén corriendo, podrás acceder a la tienda virtual a través del puerto expuesto por el servicio del Frontend.

## 🧪 Pruebas (Testing)
El backend incluye un conjunto de pruebas configurado con **Jest**. Para ejecutar los tests localmente, navega al directorio del backend y ejecuta:
```bash
npm install
npm run test
```
