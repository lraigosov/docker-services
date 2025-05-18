# docker-services

Repositorio para gestionar y desplegar servicios utilizando Docker y Docker Compose.

## Descripción

Este proyecto contiene archivos de configuración y utilidades para levantar servicios en contenedores Docker, como n8n y PostgreSQL.

## Requisitos

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/) (opcional, según los servicios que se agreguen)

## Instalación

Clona este repositorio:

```sh
git clone https://github.com/tu-usuario/docker-services.git
cd docker-services
```

## Uso

Actualmente, el repositorio incluye archivos de configuración para servicios individuales (YAML), pero no un archivo `docker-compose.yml` general.  
Puedes utilizar los archivos `.yaml` para desplegar servicios específicos según tus necesidades.

### Archivos principales

- `n8n-service.yaml`: Configuración para el servicio n8n.
- `n8n-service_variables`: Variables de entorno para n8n.
- `postgresql-service.yaml`: Configuración para el servicio PostgreSQL.

Consulta y adapta estos archivos según tu entorno y requerimientos.

## Estructura del proyecto

- `.gitignore`: Exclusiones recomendadas para proyectos Docker.
- `estructura.txt`: Listado de archivos y carpetas del proyecto.
- `n8n-service.yaml`: Configuración de n8n.
- `n8n-service_variables`: Variables de entorno para n8n.
- `postgresql-service.yaml`: Configuración de PostgreSQL.
- `README.md`: Documentación del proyecto.
- `.vscode/settings.json`: Configuración recomendada para el entorno de desarrollo.

## Personalización

Modifica los archivos de configuración y variables de entorno según tus necesidades.

## Licencia

MIT