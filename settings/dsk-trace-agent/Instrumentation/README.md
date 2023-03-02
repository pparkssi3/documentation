# Opentelemetry 연동 가이드

Opentelemetry는 오픈소스 프로젝트로, 다양한 언어와 프레임워크를 지원하는 분산 추적 시스템입니다.

Datasaker는 opentelemetry를 통한 분산 추적을 지원합니다. 분산 추적은 대상 애플리케이션의 실행을 추적하고, 이를 통해 애플리케이션의 성능을 분석하고 문제를 해결하는 데 사용할 수 있습니다. 분산추적은 어플리케이션이 실행되는 동안 생성된 데이터를 dsk-trace-agent에 전송함으로 이루어집니다. 현재 dsk-trace-agent는 opentelemetry, 또는 jaeger 포맷의 데이터를 지원합니다.

트레이스 데이터의 생산을 위해서는 대상 애플리케이션에 대한 instrumentation이 필요합니다. opentelemetry는 각 언어에 대한 SDK들을 지원하고 있습니다. 또한 각종 library나 에이전트를 통한 자동, 수동 instrumentaion을 지원하고 있습니다. 이 가이드에서는 opentelemetry를 사용하여 애플리케이션을 추적하는 방법을 설명합니다.

현재 다음 언어에 대한 설명을 제공하고 있습니다.

* [Java](./java.md)
* [Node.js](./nodejs.md)
* [Python](./python.md)

> Tip: opentelemtry는 다양한 언어와 프레임워크를 지원합니다. 자세한 내용은 [opentelemetry 문서](https://opentelemetry.io/docs/instrumentation/)를 참고하세요.

---
