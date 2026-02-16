# Weaviate Dockerfile README

This README documents how to build and run a Weaviate container image from the Dockerfile in this repository.

## What this setup provides

- A reproducible way to package Weaviate in a Docker image
- Local development and testing with persistent storage
- Environment-driven configuration for auth, modules, and query defaults

## Prerequisites

- Docker 24+ (or compatible)
- At least 4 GB of RAM available to Docker
- A `Dockerfile` in the project root

## Build the image

From the repository root:

```bash
docker build -t weaviate-custom:latest -f Dockerfile .
```

## Run Weaviate

This command starts Weaviate, maps HTTP and gRPC ports, and persists data in a named Docker volume:

```bash
docker run --name weaviate \
  -p 8080:8080 \
  -p 50051:50051 \
  -v weaviate_data:/var/lib/weaviate \
  -e QUERY_DEFAULTS_LIMIT=25 \
  -e AUTHENTICATION_ANONYMOUS_ACCESS_ENABLED=true \
  -e PERSISTENCE_DATA_PATH=/var/lib/weaviate \
  -e DEFAULT_VECTORIZER_MODULE=none \
  -e ENABLE_MODULES=text2vec-openai,generative-openai \
  weaviate-custom:latest
```

## Verify the container is healthy

```bash
curl -s http://localhost:8080/v1/.well-known/ready
```

Expected output:

```text
OK
```

## Common configuration variables

Set these with `-e KEY=value` when starting the container.

| Variable | Example | Purpose |
| --- | --- | --- |
| `AUTHENTICATION_ANONYMOUS_ACCESS_ENABLED` | `true` | Allows unauthenticated local access |
| `QUERY_DEFAULTS_LIMIT` | `25` | Default query result limit |
| `PERSISTENCE_DATA_PATH` | `/var/lib/weaviate` | Filesystem path for persisted data |
| `DEFAULT_VECTORIZER_MODULE` | `none` | Default vectorizer for new classes |
| `ENABLE_MODULES` | `text2vec-openai,generative-openai` | Enables optional modules |

> Note: Some modules require additional API keys (for example `OPENAI_APIKEY`) and outbound network access.

## Stop and clean up

Stop and remove the running container:

```bash
docker rm -f weaviate
```

Remove persisted data as well:

```bash
docker volume rm weaviate_data
```

## Troubleshooting

- **Container exits immediately:** inspect logs with `docker logs weaviate`
- **Port already in use:** change published ports, for example `-p 18080:8080`
- **Module errors at startup:** confirm all required module API keys are provided
- **Data not persisted:** make sure the volume mount and `PERSISTENCE_DATA_PATH` match
