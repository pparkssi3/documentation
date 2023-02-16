# Ubuntu 환경에 엘라스틱서치 에이전트 설치 방법

## 1. Installing the package

> **sudo** 권한이 필요합니다.

<!-- 
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->
```bash
DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}

curl -fsSL -o installer.sh https://nextcloud.exem-oss.org/s/D6SFsRsQfYJHedd/download/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-elasticsearch-agent

sudo DSK_GLOBAL_APIKEY=${DSK_GLOBAL_APIKEY} bash -c '/usr/bin/dsk-elasticsearch-agent init "'${DSK_GLOBAL_APIKEY}'" && systemctl enable dsk-elasticsearch-agent --now'
```

## 2. 패키지 실행 상태 확인

```bash
systemctl status dsk-elasticsearch-agent
```

또는

```bash
service dsk-elasticsearch-agent status
```

## 3. 패키지 중단

```bash
systemctl stop dsk-elasticsearch-agent
```

또는

```bash
service dsk-elasticsearch-agent stop
```

## 4. 패키지 제거

```bash
sudo apt remove dsk-elasticsearch-agent
```

## 패키지 설정 방법

엘라스틱서치를 모니터링 하기 위해서 에이전트 설정 변경해야 할 수 있습니다.

## Args 인자 목록

**데이터세이커 엘라스틱서치 에이전트**의 설정을 추가 할 수 있습니다.

다음 문서를 참고해주세요. [관련 문서](../../../../settings/dsk-elasticsearch-agent/settings.md)
