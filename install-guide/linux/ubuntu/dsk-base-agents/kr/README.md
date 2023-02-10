# Ubuntu 환경에 기본 에이전트 설치 방법

## 1. 패키지 설치

> **sudo** 권한이 필요합니다.

<!-- 
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->
```bash
DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}

curl -fsSL -o installer.sh https://nextcloud.exem-oss.org/s/eKiRRnym7KRW98Y/download/dsk-node-agent-install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-node-agent

sudo DSK_GLOBAL_APIKEY=${DSK_GLOBAL_APIKEY} bash -c '/usr/bin/dsk-node-agent init "'${DSK_GLOBAL_APIKEY}'" && systemctl enable dsk-node-agent --now'
```

## 2. 패키지 실행 상태 확인

```bash
systemctl status dsk-node-agent
```

또는 

```bash
service dsk-node-agent status
```

## 3. 패키지 중단

```bash
systemctl stop dsk-node-agent
```

또는 

```bash
service dsk-node-agent stop
```

## 4. 패키지 제거

```bash
sudo apt remove dsk-node-agent
```
