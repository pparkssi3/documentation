# 쿠버네티스에 헬름 차트로 기본 에이전트 설치하기

## 유저 테넌트 정보 등록

**데이터세이커**는 쿠버네티스에 **기본 에이전트를**를 설치할 수 있는 헬름 차트를 제공합니다. 차트를 설치하기 전에 유저 테넌트 정보를 등록해야 합니다.

> **데이터세이커** 디렉토리 생성 및 권한 설정

```shell
mkdir -p ~/datasaker
chmod 755 ~/datasaker/
```

> **데이터세이커** 유저 정보 등록

```shell
cat << EOF > ~/datasaker/config.yaml
userInfo:
  clusterName: ${VAR_CLUSTER_NAME}
  apiKey: ${VAR_GLOBAL_APIKEY}
  runtimeType: ${VAR_RUNTIME_TYPE}
  runtimeSock: ${VAR_RUNTIME_SOCK}
EOF
```

## helm을 이용한 에이전트 설치

> 데이터세이커 헬름 레포지토리 추가

  ```shell
  helm repo add datasaker https://datasaker.github.io/agent-helm/
  ```

> 데이터세이커 헬름 pull

  ```shell
  helm pull datasaker/agent-helm
  ```

> 압축 해제

  ```shell
  tar -zxvf agent-helm-0.1.0.tgz -C ~/datasaker
  ```

> 기본 에이전트 설치

  ```shell
  helm install datasaker ~/datasaker/agent-helm -n datasaker --create-namespace \
    -f ~/datasaker/config.yaml
  ```
