# 쿠버네티스 환경에서 데이터세이터 Mongo agent 설치하기

## DataSaker를 설치하셨나요?
현재 Kubernetes 환경에 `DataSaker`가 설치되어 있지 않다면 `DataSaker` 설치를 먼저 진행하여 주시기 바랍니다. [DataSaker 설치하기](../../README.md)

# Mongo agent 설치하기
## 1. Mongo agent 설정값 등록
```shell
cat << EOF >> ~/datasaker/config.yaml

mongoAgent:
  list: []
EOF
```
### Mongo agent 설정 값
Mongo agent의 설정 값의 의미와 default값은 다음과 같습니다. 사용자마다 에이전트 설정에 대해 다른 요구사항이 있습니다. 따라서 에이전트 설정을 사용자 설정에 맞게 조정해야 합니다. 최적의 결과를 위해 에이전트 설정을 조정하세요.
"~/datasaker/config.yaml"에서 해당 값을 추가하거나 수정하세요.
```yaml
```

## 2. Mongo agent 동작
```shell
helm upgrade datasaker ~/datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```
