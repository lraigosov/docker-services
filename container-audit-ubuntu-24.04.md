## Auditoria de Contenedores - Ubuntu-24.04

Fecha: 2026-05-22

### Inventario Detectado

| Contenedor | Imagen | Estado | YAML/Compose asociado en este repo | Resultado de prueba |
|---|---|---|---|---|
| postgres_server | postgres:latest | Up (healthy) | compose/postgresql-service.yaml | OK (healthcheck=healthy) |
| n8n_init | postgres:latest | Exited (0) | compose/n8n-service.yaml | OK (init finalizado) |
| n8n-server | n8nio/n8n:latest | Up | compose/n8n-service.yaml | OK (HTTP 200 en /) |
| grafana_init | postgres:latest | Exited (0) | compose/grafana-service.yaml | OK (init finalizado) |
| grafana-server | grafana/grafana:latest | Up | compose/grafana-service.yaml | OK (HTTP 200 en /api/health) |
| redis_server | redis:latest | Up (healthy) | compose/airflow-service.yaml | OK (healthy) |
| airflow_init | apache/airflow:latest-python3.12 | Exited (0) | compose/airflow-service.yaml | OK (init finalizado) |
| airflow_apiserver | apache/airflow:latest-python3.12 | Up | compose/airflow-service.yaml | OK (health interno 200 en /api/v2/monitor/health) |
| airflow_scheduler | apache/airflow:latest-python3.12 | Up | compose/airflow-service.yaml | OK (proceso activo) |
| airflow_worker | apache/airflow:latest-python3.12 | Up | compose/airflow-service.yaml | OK (proceso activo) |
| hop-init | debian:bullseye-slim | Exited (0) | compose/apache-hop-service.yaml | OK (init finalizado) |
| hop-server | apache/hop:2.13.0 | Up (healthy) | compose/apache-hop-service.yaml | OK (HTTP 401 esperado por autenticacion) |
| hop-web | apache/hop-web:2.13.0 | Up (healthy) | compose/apache-hop-service.yaml | OK (HTTP 302 esperado) |
| nifi | apache/nifi:latest | Up | compose/apache-nifi.service.yaml | OK (HTTPS 200 en /nifi/) |
| portainer | portainer/portainer-ce:latest | Up | No gestionado en este repo | Excluido por instruccion |
| nodered | nodered/node-red:latest | Eliminado | compose/nodered-service.yaml | YAML personalizado creado |
| mysql-server | mysql:latest | Eliminado | compose/mysql-service.yaml | YAML personalizado creado |
| cassandra-server | cassandra:latest | Eliminado | compose/cassandra-service.yaml | YAML personalizado creado |

### Observaciones

- La validacion y pruebas se ejecutaron sobre la distro Ubuntu-24.04.
- Se eliminaron contenedores legacy no esenciales y se reemplazaron por YAML personalizados en este repositorio.
- Portainer se excluyo de cambios, respetando la instruccion.
