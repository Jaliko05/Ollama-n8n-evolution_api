# 🔧 Entorno de Orquestación IA + Automatización

Este entorno contiene una arquitectura compuesta por múltiples servicios para habilitar procesamiento de lenguaje natural con **Ollama**, automatización de flujos con **n8n**, persistencia con **PostgreSQL**, motor vectorial **Qdrant**, almacenamiento temporal con **Redis** y backend especializado **Evolution API**. 

## 🧱 Servicios Incluidos

| Servicio        | Propósito |
|----------------|----------|
| `postgres`     | Base de datos relacional para n8n y Evolution API |
| `evolution_api`| API de backend desarrollada en Go para gestión de flujos Evolution |
| `redis`        | Almacenamiento temporal tipo clave/valor usado por Evolution |
| `n8n`          | Plataforma de automatización de flujos |
| `qdrant`       | Base de datos vectorial para búsquedas semánticas |
| `ollama`       | Motor local de modelos de lenguaje (Llama 3.1, Nomic Embed) |

## 📦 Pre-requisitos

Antes de ejecutar el entorno, asegurese de contar con:

1. [Docker](https://docs.docker.com/get-docker/) 
2. [Docker Compose](https://docs.docker.com/compose/install/) 
3. Soporte opcional para GPU con [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) si desea usar el perfil `gpu-nvidia`  
4. Archivo `.env` con las siguientes variables definidas:

```env
POSTGRES_USER=usuario
POSTGRES_PASSWORD=clave
POSTGRES_DB=n8n
N8N_ENCRYPTION_KEY=clave_secreta
N8N_USER_MANAGEMENT_JWT_SECRET=otra_clave
```

## 🚀 Instalación

1. Clonar el repositorio:

```bash
git clone https://github.com/Jaliko05/Ollama-n8n-evolution_api.git
cd mi-repo
```

2. Crear archivo `.env` con las variables requeridas (ver sección anterior).

3. Iniciar servicios (modo CPU por defecto):

```bash
docker compose --profile cpu up -d
```

Para usar la versión con aceleración por GPU:

```bash
docker compose --profile gpu-nvidia up -d
```

> ⚠️ Recuerde que el modelo Llama3.1 y el embedder `nomic-embed-text` se descargan automáticamente al iniciar el contenedor `ollama-pull-llama-*`.

## 🕸️ Red y Volúmenes

Todos los servicios están conectados a una red llamada `evo_n8n_net`. Los datos persistentes se almacenan en volúmenes Docker:

- `n8n_storage`
- `postgres_data`
- `qdrant_storage`
- `evolution_instances`
- `evolution_redis`
- `ollama_storage`

## 📍 Puertos Expuestos

| Servicio     | Puerto Local | Descripción |
|--------------|--------------|-------------|
| PostgreSQL   | `5432`       | Base de datos |
| Evolution API| `8080`       | Backend API |
| Redis        | `6379`       | Almacenamiento en memoria |
| n8n          | `5678`       | Interfaz web de automatizaci贸n |
| Qdrant       | `6333`       | API REST vectorial |
| Ollama       | `11434`      | Servicio LLM local |

## 🧪 Verificación

Puede verificar que los servicios están corriendo:

```bash
docker compose ps
```

Y acceder a:

- n8n: http://localhost:5678
- Evolution API: http://localhost:8080
- Qdrant: http://localhost:6333
- Ollama: http://localhost:11434

## 🧹 Apagar servicios

```bash
docker compose down
```

## 🛠️Notas adicionales

- `ollama-pull-llama-*` son contenedores de inicialización diseñados para descargar modelos. Se auto detienen tras finalizar.
- El entorno está pensado para ser extensible. Puede agregar nuevos modelos Ollama simplemente modificando la línea de `pull`.

## 📝 Licencia

Este proyecto compone y orquesta servicios de terceros a través de imágenes oficiales disponibles públicamente. Cada servicio (Ollama, Qdrant, n8n, Redis, PostgreSQL, etc.) está sujeto a su propia licencia de uso, la cual puede revisarse en los respectivos repositorios oficiales:

- [Ollama](https://github.com/ollama/ollama) – Licencia específica de Ollama
- [Qdrant](https://github.com/qdrant/qdrant) – Apache 2.0
- [n8n](https://github.com/n8n-io/n8n) – Sustainable Use License (SUL)
- [Redis](https://github.com/redis/redis) – BSD 3-Clause
- [PostgreSQL](https://www.postgresql.org/about/licence/) – PostgreSQL License
- [Evolution API](https://github.com/atendai/evolution-api) – Licencia según proveedor

Este `docker-compose` no modifica, redistribuye ni empaqueta estos servicios, solo automatiza su despliegue. Se recomienda revisar cada repositorio antes de su uso en producción.

