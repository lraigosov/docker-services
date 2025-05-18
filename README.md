# docker-services

Repositorio para gestionar y desplegar servicios utilizando Docker y Docker Compose.

## Descripción

Este proyecto contiene archivos de configuración y utilidades para levantar servicios en contenedores Docker, incluyendo n8n, PostgreSQL, Apache Hop, Apache NiFi y Apache Airflow.

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

1. Revisa y adapta `n8n-service.yaml` y `n8n-service_variables` según tu entorno.
2. Despliega el servicio con:
   ```sh
   docker compose -f n8n-service.yaml --env-file n8n-service_variables up -d
   ```
3. Accede a n8n desde tu navegador en [http://localhost:5678](http://localhost:5678) (puerto por defecto, revisa el archivo YAML para confirmar).

### PostgreSQL

1. Revisa y adapta `postgresql-service.yaml` según tu entorno.
2. Despliega el servicio con:
   ```sh
   docker compose -f postgresql-service.yaml up -d
   ```
3. Conéctate a PostgreSQL usando un cliente en `localhost:5432` (puerto por defecto, revisa el archivo YAML para confirmar).

### Apache Hop

1. Revisa y adapta `apache-hop-service.yaml` según tu entorno.
2. Despliega el servicio con:
   ```sh
   docker compose -f apache-hop-service.yaml up -d
   ```
3. Accede a Apache Hop en [http://localhost:8080](http://localhost:8080) (puerto por defecto, revisa el archivo YAML para confirmar).

### Apache NiFi

1. Revisa y adapta `apache-nifi.service.yaml` según tu entorno.
2. Despliega el servicio con:
   ```sh
   docker compose -f apache-nifi.service.yaml up -d
   ```
3. Accede a Apache NiFi en [http://localhost:8081](http://localhost:8081) (puerto por defecto, revisa el archivo YAML para confirmar).

### Apache Airflow

1. Revisa y adapta `airflow-service.yaml` y `airflow-service_variables` según tu entorno.
2. Despliega el servicio con:
   ```sh
   docker compose -f airflow-service.yaml --env-file airflow-service_variables up -d
   ```
3. Accede a la interfaz web de Airflow en [http://localhost:8080](http://localhost:8080) (puerto por defecto, revisa el archivo YAML para confirmar).

## Estructura del proyecto

- `.gitignore`
- `airflow-service.yaml`
- `airflow-service_variables`
- `apache-hop-service.yaml`
- `apache-nifi.service.yaml`
- `estructura.txt`
- `n8n-service.yaml`
- `n8n-service_variables`
- `postgresql-service.yaml`
- `README.md`
- `.vscode/settings.json`

## Personalización

Modifica los archivos de configuración y variables de entorno según tus necesidades.

## Licencia

MIT