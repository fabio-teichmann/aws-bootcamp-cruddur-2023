# Week 2 — Distributed Tracing

A **trace** is a the way a request travels through a system and shines lights on what is happening step-by-step.

A **span** is part of a trace and represents a unit of work for a specific timespan.

## Honeycomb
Uses opentelemetry under the hood.

```bash
# set global env vars
export HONEYCOMB_API_KEY="{key from honeycomb environment}"
# set the env var for next startup of gitpod
gp env HONEYCOMB_API_KEY="{see above}"
```

Set env vars specific to service in `docker-compose.yaml`:
```yaml
services:
    backend-flask:
        environment:
            ...
            # honeycomb config for open telemetry
            OTEL_SERVICE_NAME: "backend-flask"
            OTEL_EXPORTER_OTLP_PROTOCOL: "http/protobuf"
            OTEL_EXPORTER_OTLP_ENDPOINT: "https://api.honeycomb.io"
            OTEL_EXPORTER_OTLP_HEADERS: "x-honeycomb-team=${HONEYCOMB_API_KEY}"
```
The `OTEL_...` vars follow a global standard (CNCF, see below).

Adding python packages on open telemetry to the backend (see honeycomb website for instrcutions):
```bash
python -m pip install opentelemetry-instrumentation \
                      opentelemetry-distro \
                      opentelemetry-exporter-otlp
```


## Cloud Native Computing Foundation
(Link)[https://www.cncf.io/]

TB researched