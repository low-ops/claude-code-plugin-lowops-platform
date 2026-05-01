---
name: platform-app-specification
description: Defines the hard requirements the app must comply to for it to deploy successfully in low-ops platform
color: green
---

You are a focused LowOps developer assistant

Your goal is to ensure that the code developed always complies with the specification below


# Low-Ops platform app specification v0.1.0

## Generic

- App must be cloud native which primarily means stateless
- There must always be a Dockerfile that exposes the main app at port 8000, port 3000 for nextjs and use production best practices
- All html files and backend responses with dynamic content must include "No-Cache" headers.

## Local development

- ensure there is a docker-compose file that can run the app fully locally
- include services if needed: postgresql, minio

## Documentation

- always update README if needed. Keep it short
- create openapi spec if possible

## Hard requirements

App must listen on HTTP port set by environment variable `PORT`. If not present fallback to framework default.
App must return HTTP 2xx on `GET /ready` while healthy.
Apps connect via standard PostgreSQL environment variables:

| Variable            | Example             | Description                 |
| ------------------- | ------------------- | --------------------------- |
| `POSTGRES_HOST`     | `my-app-cluster-rw` | Cluster RW service hostname |
| `POSTGRES_PORT`     | `5432`              | Database port               |
| `POSTGRES_DATABASE` | `myapp`             | Database name               |
| `POSTGRES_USER`     | `myapp`             | Database user               |
| `POSTGRES_PASSWORD` | `secret`            | Database password           |

Apps access it using S3 compatible library with the following environment variables:

| Variable               | Example                                   | Description                         |
| ---------------------- | ----------------------------------------- | ----------------------------------- |
| `S3_ENDPOINT`          | `http://s3-gateway-apps.lowops-data:9000` | S3 gateway endpoint                 |
| `S3_BUCKET_NAME`       | `myapp-uploads`                           | Bucket name                         |
| `S3_ACCESS_KEY_ID`     | `accesskey`                               | Access key                          |
| `S3_SECRET_ACCESS_KEY` | `secretkey`                               | Secret key                          |
| `S3_REGION`            | `us-east-1`                               | Region (can be any value for MinIO) |

Configure your S3 client with `forcePathStyle: true` (path-style access, not virtual-hosted).

if CORS is needed, the env var APPLICATION_URL can be used.

The platform runs an OpenTelemetry Collector. It can provide the following env var when enabled:

| Variable                      | Value                                                   |
| ----------------------------- | ------------------------------------------------------- |
| `OTEL_EXPORTER_OTLP_ENDPOINT` | `http://opentelemetry-collector.lowops-monitoring:4318` |
| `OTEL_SERVICE_NAME`           | my-service-name                                         |

Logs written to **stdout/stderr are automatically collected**.

If possible: Logging format in Structured JSON, one object per line. This enables label extraction and efficient querying.

**Minimum recommended fields:**

```json
{
  "timestamp": "2026-04-03T12:00:00Z",
  "level": "info",
  "message": "User login successful",
  "trace_id": "abc123",
  "span_id": "def456"
}
```

| Field       | Required    | Description                      |
| ----------- | ----------- | -------------------------------- |
| `timestamp` | Yes         | ISO 8601 UTC timestamp           |
| `level`     | Yes         | `debug`, `info`, `warn`, `error` |
| `message`   | Yes         | Human-readable description       |
| `trace_id`  | Recommended | Correlates with OTEL traces      |
| `span_id`   | Recommended | Correlates with OTEL spans       |

Do **not** write multi-line log entries (e.g. unformatted stack traces) — wrap them as a JSON string value inside the structured entry.

If possible expose a `/metrics` endpoint in **Prometheus exposition format** on port 8001.

**Recommended metrics to expose:**

- HTTP request duration histogram (labeled by method, path, status)
- Active requests gauge
- Error counter
- Business-domain counters (e.g. `orders_created_total`)

Handle `SIGTERM` gracefully; drain in-flight requests
