# JAVA

Java 애플리케이션은 수동, 혹은 자동으로 instrumentation 되어질 수 있습니다. 이 가이드에서는 **opentelemetry-javaagent**를 사용하여 자동 instrumentaion을 수행하는 방법을 설명합니다.

## 요구 사항

* **Java 8 이상**

## Javaagent 다운로드

javaagent는 [github 페이지](https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/latest)에서 다운로드할 수 있습니다.
다음 명령어로 최신 버전을 다운로드 받을 수 있습니다.

```bash
wget https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/latest/download/opentelemetry-javaagent.jar
```

## 설정하기

각 실행 환경에 맞게 설정을 진행 합니다.

### Non-Container 환경

어플리케이션을 실행할 때 다음과 같이 시스템 프로퍼티를 추가합니다.

``` bash
java -javaagent:path/to/opentelemetry-javaagent.jar \
     -Dotel.exporter.otlp.traces.endpoint=localhost:4317 \
     -Dotel.service.name=your-service-name \
     -Dotel.metrics.exporter=none \
     -Dotel.logs.exporter=none \
     -jar myapp.jar
```

### Docker 환경

Docker 네트워크 설정을 통해 Trace Agent로 접근할 수 있도록 설정 합니다.

``` bash
    docker network create <network-name>
```

``` bash
    # 자바 어플리케이션 실행 예시
    docker run my-java-application \
        -e OTEL_EXPORTER_OTLP_TRACES_ENDPOINT=localhost:4317 \
        -e OTEL_SERVICE_NAME=your-service-name \
        -e OTEL_METRICS_EXPORTER=none \
        -e OTEL_LOGS_EXPORTER=none \
        --network <network-name> \
        -d
```

혹은 다음과 같이 시스템 프로퍼티를 추가합니다.

``` dockerfile
    # Dockerfile 예시
    FROM openjdk:8-jre-alpine
    COPY opentelemetry-javaagent.jar /app/
    COPY myapp.jar /app/
    WORKDIR /app
    CMD ["java", "-javaagent:opentelemetry-javaagent.jar", \
    "-Dotel.service.name=your-service-name", \
    "-Dotel.metrics.exporter=none", \
    "-Dotel.logs.exporter=none", \
    "-Dotel.exporter.jaeger.endpoint=http://dsk-trace-agent-service:14250",\
    "-jar", "myapp.jar"]
```

### Kubernetes 환경

Dockerfile 생성 시 다음과 같이 opentelemetry javaagent를 추가합니다.

``` dockerfile
    # Dockerfile 예시
    FROM openjdk:8-jre-alpine
    COPY opentelemetry-javaagent.jar /app/
    COPY myapp.jar /app/
    WORKDIR /app
    CMD ["java", "-javaagent:opentelemetry-javaagent.jar", "-jar", "myapp.jar"]
```

배포 되는 yaml 파일에 아래와 같이 환경변수를 설정 합니다.

``` yaml
    spec:
      containers:
      - name: myapp
        image: myapp:latest
        env:
        # 아래와 같이 환경 변수들을 추가합니다.
        - name: DSK_NODE_NAME
          valueFrom:
            fieldRef:
              apiVersion: v1
              fieldPath: spec.nodeName
        - name: DSK_NAMESPACE
          valueFrom:
            fieldRef:
              apiVersion: v1
              fieldPath: metadata.namespace
        - name: DSK_POD_NAME
          valueFrom:
            fieldRef:
              apiVersion: v1
              fieldPath: metadata.name
        - name: OTEL_SERVICE_NAME
          valueFrom:
            fieldRef:
              apiVersion: v1
              fieldPath: metadata.labels['app.kubernetes.io/name'] # 혹은 서비스를 구분할 수 있는 다른 label
        - name: OTEL_EXPORTER_OTLP_ENDPOINT
          value: http://dsk-trace-agent.dsk-agent-dev:4317 # dsk trcace agent 서비스명
        - name: OTEL_METRICS_EXPORTER
          value: none
        - name: OTEL_LOGS_EXPORTER
          value: none
        - name: OTEL_RESOURCE_ATTRIBUTES
          value: dsk_node_name=$(DSK_NODE_NAME),dsk_namespace=$(DSK_NAMESPACE),dsk_pod_name=$(DSK_POD_NAME),dsk_app_name=$(DSK_NAMESPACE)_$(DSK_POD_NAME)
```
