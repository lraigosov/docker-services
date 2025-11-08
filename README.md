# docker-services

Repositorio para gestionar y desplegar servicios utilizando Docker y Docker Compose.

## Instalación de Docker y Portainer en Ubuntu

### Instalar Docker

```sh
sudo apt update
sudo apt install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo usermod -aG docker $USER
```
> **Nota:** Cierra la sesión y vuelve a entrar para que los cambios de grupo tengan efecto.

### Instalar Portainer

```sh
docker volume create portainer_data
docker run -d -p 9443:9443 --name portainer --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest
```
Accede a Portainer en [https://localhost:9443](https://localhost:9443) y sigue las instrucciones para crear el usuario administrador.

## Descripción

Este proyecto contiene archivos de configuración para desplegar servicios mediante Docker Compose, incluyendo n8n, PostgreSQL, Apache Hop, Apache NiFi, Airflow y Grafana.

## Requisitos

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/)
- Red Docker externa llamada `red-principal` (crear con: `docker network create red-principal`)

## Instalación

Clona este repositorio:

```sh
git clone https://github.com/lraigosov/docker-services.git
cd docker-services
```

## Orden de despliegue recomendado

1. **Crear la red Docker**:
   ```sh
   docker network create red-principal
   ```

2. **PostgreSQL** (base de datos compartida):
   ```sh
   docker compose -f postgresql-service.yaml up -d
   ```

3. **Servicios dependientes** (en cualquier orden):
   - n8n
   - Airflow (incluye Redis)
   - Grafana

4. **Servicios independientes**:
   - Apache Hop
   - Apache NiFi

## Guía de acceso a los servicios

### n8n

- **¿Para qué sirve?**: Automatización de flujos de trabajo (workflows) y orquestación de tareas.
- **Despliegue**:
  1. Asegúrate de que el servicio PostgreSQL esté ejecutándose.
  2. Revisa y adapta las variables en `n8n-service_variables`:
     - `POSTGRES_HOST`: postgres_server
     - `POSTGRES_USER`: postgres
     - `POSTGRES_PASSWORD`: postgrespw
     - `POSTGRES_DB`: n8n
  3. Ejecuta:
     ```sh
     docker compose -f n8n-service.yaml --env-file n8n-service_variables up -d
     ```
  4. Accede en [http://localhost:5678](http://localhost:5678).
- **Notas**: 
  - El contenedor `n8n-init` crea automáticamente la base de datos `n8n` en PostgreSQL si no existe.
  - Utiliza PostgreSQL como backend de base de datos (configurado con `DB_TYPE=postgresdb`).
  - Zona horaria configurada: America/Bogota.
  - Incluye runners habilitados (`N8N_RUNNERS_ENABLED=true`).

### PostgreSQL

- **¿Para qué sirve?**: Base de datos relacional utilizada por otros servicios (n8n, Airflow, Grafana).
- **Despliegue**:
  1. Ejecuta:
     ```sh
     docker compose -f postgresql-service.yaml up -d
     ```
  2. Conéctate usando un cliente en `localhost:5432`.
- **Credenciales por defecto**:
  - Usuario: `postgres`
  - Contraseña: `postgrespw`
  - Base de datos: `postgres_db`
  - Host: `postgres_server` (nombre del contenedor)
- **Notas**: 
  - Configurado con `max_connections=100` y `shared_buffers=128MB`.
  - Encoding UTF8 y locale en_US.UTF-8.
  - Los demás servicios se conectan usando el nombre del contenedor `postgres_server` en la red `red-principal`.

### Apache Hop

- **¿Para qué sirve?**: Plataforma para desarrollo y ejecución de pipelines de datos (ETL).
- **Despliegue**:
  1. Ejecuta:
     ```sh
     docker compose -f apache-hop-service.yaml up -d
     ```
  2. Accede a Hop Web en [http://localhost:8080](http://localhost:8080).
  3. Hop Server ejecuta en el puerto 8181.
- **Credenciales Hop Server**:
  - Usuario: `admin`
  - Contraseña: `admin123456`
- **Notas**: 
  - Incluye tres contenedores: `hop-init` (inicialización), `hop-server` y `hop-web`.
  - Crea automáticamente la estructura de proyecto `my_project` con ambiente `dev`.
  - Los volúmenes montados incluyen: `./projects`, `./config`, `./logs`, `./files` y `./audit`.
  - Validación de integridad mediante checksums SHA256.

### Apache NiFi

- **¿Para qué sirve?**: Automatización y gestión de flujos de datos entre sistemas.
- **Despliegue**:
  1. Ejecuta:
     ```sh
     docker compose -f apache-nifi.service.yaml up -d
     ```
  2. Accede en [https://localhost:8443/nifi](https://localhost:8443/nifi) (HTTPS).
- **Credenciales**:
  - Usuario: `admin`
  - Contraseña: `admin123456789`
- **Notas**: 
  - Utiliza HTTPS en el puerto 8443.
  - Zona horaria configurada: America/Bogota.
  - Incluye volúmenes persistentes para configuración, estado, logs, y repositorios de contenido.

### Apache Airflow

- **¿Para qué sirve?**: Orquestación de flujos de trabajo programados mediante DAGs.
- **Despliegue**:
  1. Asegúrate de que el servicio PostgreSQL esté ejecutándose.
  2. Revisa y adapta las variables en `airflow-service_variables`:
     - `POSTGRES_USER`: postgres (debe agregarse manualmente)
     - `POSTGRES_PASSWORD`: postgrespw
     - `POSTGRES_HOST`: postgres_server
     - `REDIS_PASSWORD`: contraseña para Redis (debe agregarse manualmente)
     - `AIRFLOW_FERNET_KEY`: clave de encriptación
     - `AIRFLOW_VERSION`: latest
     - `AIRFLOW_UID`: 5000
     - `AIRFLOW_GID`: 5000
  3. Ejecuta:
     ```sh
     docker compose -f airflow-service.yaml --env-file airflow-service_variables up -d
     ```
  4. Accede en [http://localhost:8080](http://localhost:8080).
- **Credenciales por defecto**:
  - Usuario: `admin`
  - Contraseña: `admin`
- **Notas**: 
  - Incluye cinco contenedores: `redis`, `airflow-init`, `airflow-webserver`, `airflow-scheduler` y `airflow-worker`.
  - Utiliza PostgreSQL como base de datos y Redis como broker para Celery Executor.
  - El contenedor `airflow-init` configura automáticamente la base de datos `airflow` y crea el usuario admin.
  - Los volúmenes incluyen: `airflow_logs`, `airflow_dags` y `airflow_plugins`.
  - **Importante**: Limpia las variables duplicadas en `airflow-service_variables` antes del primer despliegue.

### Grafana

- **¿Para qué sirve?**: Visualización y monitoreo de datos mediante dashboards.
- **Despliegue**:
  1. Asegúrate de que el servicio PostgreSQL esté ejecutándose.
  2. Revisa y adapta las variables en `grafana-service_variables`:
     - `GRAFANA_PG_HOST`: postgres_server
     - `GRAFANA_PG_PORT`: 5432 (debe agregarse manualmente al archivo)
     - `GRAFANA_PG_DATABASE`: grafana
     - `GRAFANA_PG_USER`: postgres
     - `GRAFANA_PG_PASSWORD`: postgrespw
     - `GRAFANA_ADMIN_USER`: admin
     - `GRAFANA_ADMIN_PASSWORD`: admin123
  3. Ejecuta:
     ```sh
     docker compose -f grafana-service.yaml --env-file grafana-service_variables up -d
     ```
  4. Accede en [http://localhost:3000](http://localhost:3000).
- **Credenciales**:
  - Usuario: `admin`
  - Contraseña: `admin123`
- **Notas**: 
  - El contenedor `grafana-init` crea automáticamente la base de datos `grafana` en PostgreSQL si no existe.
  - Plugins preinstalados: `grafana-piechart-panel`, `grafana-worldmap-panel`, `grafana-clock-panel`.
  - Idioma por defecto: Español.
  - Zona horaria: America/Bogota.

## Estructura del proyecto

```
docker-services/
├── airflow-service.yaml              # Configuración de Apache Airflow con Celery y Redis
├── airflow-service_variables         # Variables de entorno para Airflow
├── apache-hop-service.yaml           # Configuración de Apache Hop (Web y Server)
├── apache-nifi.service.yaml          # Configuración de Apache NiFi
├── grafana-service.yaml              # Configuración de Grafana con PostgreSQL
├── grafana-service_variables         # Variables de entorno para Grafana
├── n8n-service.yaml                  # Configuración de n8n con PostgreSQL
├── n8n-service_variables             # Variables de entorno para n8n
├── postgresql-service.yaml           # Configuración de PostgreSQL
├── estructura.txt                    # Documentación de la estructura
└── README.md                         # Este archivo
```

## Notas importantes

- Todos los servicios requieren la red Docker `red-principal`. Créala antes de desplegar:
  ```sh
  docker network create red-principal
  ```
- Los servicios n8n, Airflow y Grafana dependen de PostgreSQL. Despliega PostgreSQL primero.
- Revisa y personaliza los archivos `*_variables` antes del despliegue para ajustar credenciales y configuraciones.
- **Importante**: El archivo `airflow-service_variables` requiere configuración manual de las variables `POSTGRES_USER`, `REDIS_PASSWORD` antes del primer despliegue.
- El archivo `grafana-service_variables` requiere definir `GRAFANA_PG_PORT` (por defecto: 5432).

### Puertos utilizados

| Servicio | Puerto | Protocolo | URL de acceso |
|----------|--------|-----------|---------------|
| n8n | 5678 | HTTP | http://localhost:5678 |
| PostgreSQL | 5432 | TCP | localhost:5432 |
| Apache Hop Web | 8080 | HTTP | http://localhost:8080 |
| Apache Hop Server | 8181 | HTTP | http://localhost:8181 |
| Apache NiFi | 8443 | HTTPS | https://localhost:8443/nifi |
| Airflow | 8080 | HTTP | http://localhost:8080 |
| Airflow Redis | 6379 | TCP | localhost:6379 |
| Grafana | 3000 | HTTP | http://localhost:3000 |
| Portainer | 9443 | HTTPS | https://localhost:9443 |

**Advertencia**: Apache Hop Web y Airflow comparten el puerto 8080. No pueden ejecutarse simultáneamente sin modificar la configuración.

## Licencia

MIT