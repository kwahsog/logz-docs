```yaml
receivers:
  jaeger:
    protocols:
      thrift_compact:
        endpoint: "0.0.0.0:6831"
      thrift_binary:
        endpoint: "0.0.0.0:6832"
      grpc:
        endpoint: "0.0.0.0:14250"
      thrift_http:
        endpoint: "0.0.0.0:14268"
  opencensus:
    endpoint: "0.0.0.0:55678"
  otlp:
    protocols:
      grpc:
        endpoint: "0.0.0.0:4317"
      http:
        endpoint: "0.0.0.0:4318"
  zipkin:
    endpoint: "0.0.0.0:9411"


exporters:
  logzio:
    account_token: "<<TRACING-SHIPPING-TOKEN>>"
    region: "<<LOGZIO_ACCOUNT_REGION_CODE>>"

  logging:

processors:
  batch:

extensions:
  pprof:
    endpoint: :1777
  zpages:
    endpoint: :55679
  health_check:

service:
  extensions: [health_check, pprof, zpages]
  pipelines:
    traces:
      receivers: [opencensus, jaeger, zipkin, otlp]
      processors: [batch]
      exporters: [logging, logzio]
```
