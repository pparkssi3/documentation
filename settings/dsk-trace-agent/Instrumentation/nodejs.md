# Node.js

이 가이드에서는 Node.js 애플리케이션을 추적하기 위해 opentelemetry-js를 사용하는 방법을 설명합니다.

## 요구 사항

* **Node.js**
* **TypeScript** (옵션)

## 의존성 설치

opentelemetry-js를 사용하기 위해 다음 의존성이 필요합니다.

``` bash
npm install @opentelemetry/sdk-node @opentelemetry/api
```

또한 여러 instrumentation 모듈을 한번에 설치하기 위해 다음 의존성을 설치 할 수 있습니다.
이 가이드에선 http에 대한 instrumentation을 설치합니다.

``` bash
npm install --save @opentelemetry/exporter-trace-otlp-http
```

> Tip: 개별 instrumentation 모듈을 설치하거나, 지원 목록을 확인하기 위해선 다음 [opentelemetry registry](https://opentelemetry.io/registry/?language=js&component=instrumentation) 페이지를 참고하세요.

## 1. 코드 추가

tracing모듈은 대상 어플리케이션이 실행되기 전에 초기화되어야 합니다. 이를 위해 `-r --require module` 플래그를 사용합니다.
다음 코드로 exporter를 설정하고, tracing모듈을 초기화 할 수 있습니다.

> <span style="color:red">**주의:**</span> url은 dsk-trace-agent의 url로 변경해야 합니다.

### `tracing.js|ts`

``` javascript
const opentelemetry = require("@opentelemetry/sdk-node");
const {
  getNodeAutoInstrumentations,
} = require("@opentelemetry/auto-instrumentations-node");
const {
  OTLPTraceExporter,
} = require("@opentelemetry/exporter-trace-otlp-http");
// 통신 예정인 dsk-trace-agent의 otlp-http 주소를 넣어주세요. (혹은 TARCE_URL 환경변수를 사용해주세요.)
const url = (process.env.TRACE_URL || "http://localhost:4318");

const sdk = new opentelemetry.NodeSDK({
  traceExporter: new OTLPTraceExporter({
    url: "<your-dsk-trace-agent>/v1/traces",
    headers: {},
  }),
  instrumentations: [getNodeAutoInstrumentations()],
});

// 초기화
sdk.start()
  .then(() => console.log('Tracing initialized'))
  .catch((error) => console.log('Error initializing tracing', error));

// graceful shutdown
process.on('SIGTERM', () => {
  sdk.shutdown()
    .then(() => console.log('Tracing terminated'))
    .catch((error) => console.log('Error terminating tracing', error))
    .finally(() => process.exit(0));
})
```

디버그 로그를 출력하고 싶다면 아래 코드를 추가해주세요.

``` javascript
const { diag, DiagConsoleLogger, DiagLogLevel } = require('@opentelemetry/api');
diag.setLogger(new DiagConsoleLogger(), DiagLogLevel.DEBUG);
```

## 2. 실행

Node.js 애플리케이션을 실행하기 전에, `tracing.js` 또는 `tracing.ts`를 불러오기 위해 `-r --require` 플래그를 사용합니다.

``` bash
# javascript의 경우
node --require './tracing.js' app.js

# typescript의 경우
ts-node --require './tracing.ts' app.ts
```

## 3. 쿠버네티스 환경 변수 설정

Node.js 애플리케이션을 쿠버네티스 환경에서 실행할 경우, 환경 변수를 설정하여 트레이스에 대한 추가 정보를 제공할 수 있습니다.
아래 내용을 yaml에 추가해주세요.

``` yaml
    spec:
      containers:
      - name: myapp
        image: myapp:latest
        env:
          - name: DSK_NODE_NAME
            valueFrom:
              fieldRef:
                fieldPath: spec.nodeName
          - name: DSK_NAMESPACE
            valueFrom:
              fieldRef:
                fieldPath: metadata.namespace
          - name: DSK_POD_NAME
            valueFrom:
              fieldRef:
                fieldPath: metadata.name
          - name: OTEL_RESOURCE_ATTRIBUTES
            value: dsk_node_name=$(DSK_NODE_NAME),dsk_namespace=$(DSK_NAMESPACE),dsk_pod_name=$(DSK_POD_NAME)
```
