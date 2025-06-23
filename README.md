# Kong Plugin Examples

This is a sample configuration for experiencing Kong's plugins.

## Architecture

Observability platform have been build to see how plugins work.

```mermaid
flowchart LR

    subgraph Kong Gateway
        A1[OpenTelemetry plugin]
        A2[Prometheus plugin]
        A3[HTTP Log plugin]
    end

    A1 --> |OTLP Receiver - Log/Trace| B1[OpenTelemetry Collector]
    A2 --> |Prometheus Receiver| B1
    A3 --> |HTTP Log Input| B3[Fluent Bit] --> |OTLP Receiver| B1

    B1 --> |OTLP Exporter| C1[Grafana Tempo]
    B1 --> |Prometheus Exporter| C2[Prometheus]
    B1 --> |OTLP Exporter| C3[Grafana Loki]

    C1 --> D1[Grafana]
    C2 --> D1
    C3 --> D1
```

## How to use?

Run this script.

```sh
docker compose up -d
```

You can access as follows.

| service      | endpoint                                                         |
| ------------ | ---------------------------------------------------------------- |
| Kong Manager | [http://localhost:8002](http://localhost:8002)                   |
| API          | [http://localhost:8000/v1/greet](http://localhost:8000/v1/greet) |
| Grafana      | [http://localhost:3000](http://localhost:3000)                   |

After that, Please configure the desired plugin settings using decK. For example,

```sh
deck gateway sync plugins/canary/fixed-percentage/kong.yml
```
