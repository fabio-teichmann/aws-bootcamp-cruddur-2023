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

**Issue**: trying to connect to HoneyComb, I ran into two errors:
1. not finding `opentelemtry` when trying yo use `trace` within the code
2. HoneyComb not receiving any data when app is running and APIs are triggered

In the OpenTelemetry docs I found the following instruction to run the app:
```bash
export OTEL_PYTHON_LOGGING_AUTO_INSTRUMENTATION_ENABLED=true
opentelemetry-instrument \
    --traces_exporter console \
    --metrics_exporter console \
    --logs_exporter console \
    --service_name dice-server \
    flask run -p 8080
```

I tried to include the env var into the `docker-compose.yaml` and re-run the app in docker. This didn't trigger HoenyComb, unfortunately, but it showed traces in the console.

> Upon further investigation, the BE container was not set up properly:
> - missing OTEL dependencies
> - honeycomb api key not set

Even after the key was set properly and after I exchanged the `CMD` in the Dockerfile to what HoneyComb instructs, the HoneyComb platform still does not receive any data. I will revisit this topic at a later time since currently it is not critical to move forward.


## Cloud Native Computing Foundation
(Link)[https://www.cncf.io/]

TB researched