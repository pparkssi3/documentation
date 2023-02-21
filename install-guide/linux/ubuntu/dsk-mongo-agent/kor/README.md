# Ubuntu 환경에 DataSaker Mongo agent 설치하기

## DataSaker 설정파일을 생성하셨나요?
현재 Ubuntu 환경에서는 `DataSaker` agent를 설치하기 위해서는 기본 설정 파일이 생성되어 있어야 합니다. 만약 기본 설정 파일을 생성하지 않았다면 생성하여 주시기 바랍니다. [DataSaker 설정하기](../../README.md)


# Mongo agent 설치하기
## 1. 패키지 설치
`DataSaker`의 `Mongo agent`를 설치하기 위해서는 sudo 권한이 필요합니다.
<!-- 
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->
```bash
DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}

curl -fsSL -o installer.sh https://nextcloud.exem-oss.org/s/D6SFsRsQfYJHedd/download/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-mongo-agent

sudo DSK_GLOBAL_APIKEY=${DSK_GLOBAL_APIKEY} bash -c '/usr/bin/dsk-mongo-agent init "'${DSK_GLOBAL_APIKEY}'" && systemctl enable dsk-mongo-agent --now'
```

## 2. 패키지 실행 상태 확인
```bash
systemctl status dsk-mongo-agent
```
또는
```bash
service dsk-mongo-agent status
```

### 패키지 설정 방법
MongoDB를 모니터링 하기 위해서 agent 설정 변경해야 할 수 있습니다. \

### Args 인자 목록
`DataSaker` Mongo agent의 설정을 추가 할 수 있습니다.
다음 문서를 참고해주세요. [관련 문서](../../../../../settings/dsk-mongo-agent/settings.md)

---
# Mongo agent 제거하기
## 1. 패키지 중단
```bash
systemctl stop dsk-mongo-agent
```
또는
```bash
service dsk-mongo-agent stop
```

## 2. 패키지 제거
```bash
sudo apt remove dsk-mongo-agent
```