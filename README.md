# docker-services

Repositorio para gestionar y desplegar servicios utilizando Docker y Docker Compose.

## Descripción

Este proyecto contiene archivos de configuración y utilidades para levantar servicios en contenedores Docker, facilitando el desarrollo y pruebas de herramientas como n8n, PostgreSQL, Apache Hop, Apache NiFi, Airflow y Grafana.

## Requisitos

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/)

## Instalación

Clona este repositorio:

```sh
git clone https://github.com/tu-usuario/docker-services.git
cd docker-services
```

## Uso

El repositorio incluye archivos de configuración para servicios individuales (`.yaml`).  
Puedes utilizar los archivos `.yaml` para desplegar servicios específicos según tus necesidades.

## Guía de acceso a los servicios

### n8n

- **¿Para qué sirve?**: Automatización de flujos de trabajo (workflows) y orquestación de tareas.
- **Despliegue**:
  1. Revisa y adapta `n8n-service.yaml` y `n8n-service_variables` según tu entorno.
  2. Ejecuta:
     ```sh
     docker compose -f n8n-service.yaml --env-file n8n-service_variables up -d
     ```
  3. Accede desde tu navegador en [http://localhost:5678](http://localhost:5678) (puerto por defecto).

### PostgreSQL

- **¿Para qué sirve?**: Base de datos relacional de código abierto.
- **Despliegue**:
  1. Revisa y adapta `postgresql-service.yaml` según tu entorno.
  2. Ejecuta:
     ```sh
     docker compose -f postgresql-service.yaml up -d
     ```
  3. Conéctate usando un cliente en `localhost:5432` (puerto por defecto).

### Apache Hop

- **¿Para qué sirve?**: Plataforma para orquestación y desarrollo de pipelines de datos (ETL).
- **Despliegue**:
  1. Revisa y adapta `apache-hop-service.yaml` según tu entorno.
  2. Ejecuta:
     ```sh
     docker compose -f apache-hop-service.yaml up -d
     ```
  3. Accede en [http://localhost:8080](http://localhost:8080) (puerto por defecto).

### Apache NiFi

- **¿Para qué sirve?**: Automatización y gestión de flujos de datos entre sistemas.
- **Despliegue**:
  1. Revisa y adapta `apache-nifi.service.yaml` según tu entorno.
  2. Ejecuta:
     ```sh
     docker compose -f apache-nifi.service.yaml up -d
     ```
  3. Accede en [http://localhost:8081](http://localhost:8081) (puerto por defecto).

### Apache Airflow

- **¿Para qué sirve?**: Orquestación de flujos de trabajo programados (workflows) mediante DAGs.
- **Despliegue**:
  1. Revisa y adapta `airflow-service.yaml` y `airflow-service_variables` según tu entorno.
  2. Ejecuta:
     ```sh
     docker compose -f airflow-service.yaml --env-file airflow-service_variables up -d
     ```
  3. Accede en [http://localhost:8080](http://localhost:8080) (puerto por defecto).

### Grafana

- **¿Para qué sirve?**: Visualización y monitoreo de datos mediante dashboards.
- **Despliegue**:
  1. Revisa y adapta `grafana-service.yaml` y `grafana-service_variables` según tu entorno.
  2. Ejecuta:
     ```sh
     docker compose -f grafana-service.yaml --env-file grafana-service_variables up -d
     ```
  3. Accede en [http://localhost:3000](http://localhost:3000) (puerto por defecto).

## Estructura del proyecto

- `.gitignore`
- `airflow-service.yaml`
- `airflow-service_variables`
- `apache-hop-service.yaml`
- `apache-nifi.service.yaml`
- `estructura.txt`
- `grafana-service.yaml`
- `grafana-service_variables`
- `n8n-service.yaml`
- `n8n-service_variables`
- `postgresql-service.yaml`
- `README.md`
- `.vscode/settings.json`

## Personalización

Modifica los archivos de configuración y variables de entorno según tus necesidades.

## Licencia

MIT