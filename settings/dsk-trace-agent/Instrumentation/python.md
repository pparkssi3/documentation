# Python

Python Application은 자동 또는 수동으로 instrumentation 되어질 수 있습니다. 
이 가이드에서는 **opentelemetry-python**을 통해 자동으로 수행하는 방법을 설명합니다.

## 요구 사항

* **Python 3.6 이상**

## 패키지 설치

opentelemetry-python을 사용하기 위해선 다음 패키지가 필요합니다.

```bash
pip install opentelemetry-distro
pip install opentelemetry-exporter-otlp
```

설치한 opentelemetry-distro은 설치된 site-package의 패키지 목록을 읽고 필요한 라이브러리를 추가적으로 설치를 지원합니다.

```bash
opentelemetry-bootstrap -a install
```

## 설정하기

각 실행 환경에 맞게 설정을 진행 합니다.

### Non-Container 환경

#### Args로 추가해서 실행

Application을 실행할 때, 다음과 같이 시스템 프로퍼티를 추가합니다.

```bash
opentelemetry-instrument \
    --traces_exporter otlp \
    --metrics_exporter none \
    --logs_exporter none \
    --service_name your-service-name \
    --exporter_otlp_endpoint localhost:4317 \
    python myapp.py
```

#### 환경변수로 추가해서 실행

환경변수를 이용하여 실행할 수 있습니다.

```bash
OTEL_SERVICE_NAME=your-service-name \
OTEL_TRACES_EXPORTER=otlp \
OTEL_METRICS_EXPORTER=none \
OTEL_LOGS_EXPORTER=none \
OTEL_EXPORTER_OTLP_TRACES_ENDPOINT=localhost:4317
opentelemetry-instrument \
    python myapp.py
```

디버그용으로 콘솔에 출력되는 것을 보고 싶다면 traces_exporter에 console을 추가하면 됩니다.

```bash
# args
--traces_exporter console,otlp
# envs
OTEL_TRACES_EXPORTER=console,otlp
```

### Docker 환경

Docker 설정을 통해 접근할 수 있도록 설정합니다.

#### Args로 추가해서 실행

```dockerfile
FROM python:3.9
RUN pip install --upgrade pip

RUN mkdir /app
COPY . /app/
WORKDIR /app

ENV PYTHONPATH /app

RUN pip install -r requirements.txt
RUN opentelemetry-bootstrap -a install

ENTRYPOINT ["opentelemetry-instrument", "--traces_exporter=otlp", "--metrics_exporter=none", "--logs_exporter=none", "--service_name=your-service-name", "--exporter_otlp_endpoint=http://dsk-trace-agent-service:4317", "python", "myapp.py"]
```

#### 환경변수로 추가해서 실행

```bash
docker run my-python-application \
  -e OTEL_SERVICE_NAME=your-service-name \
  -e OTEL_TRACES_EXPORTER=otlp \
  -e OTEL_METRICS_EXPORTER=none \ 
  -e OTEL_LOGS_EXPORTER=none \ 
  -e OTEL_EXPORTER_OTLP_TRACES_ENDPOINT=localhost:4317 \
  -d
```

### Kubernetes 환경

Dockerfile 생성 시 다음과 같이 opentelemetry-bootstrap을 실행하여 필요한 패키지를 설치할 수 있도록 합니다.

```dockerfile
FROM python:3.9
RUN pip install --upgrade pip

RUN mkdir /app
COPY . /app/
WORKDIR /app

ENV PYTHONPATH /app

RUN pip install -r requirements.txt
RUN opentelemetry-bootstrap -a install

ENTRYPOINT ["opentelemetry-instrument", "python", "myapp.py"]
```

#### 환경변수로 추가해서 실행

배포되는 yaml 파일에 아래와 같이 환경변수를 설정합니다.

```yaml
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