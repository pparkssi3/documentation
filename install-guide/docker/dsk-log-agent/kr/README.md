# 도커 환경에 로그 에이전트 설치하기

**데이터세이커 로그 에이전트**를 도커 환경에 설치하는 방법을 설명합니다.

1. 데이터세이커가 사용할 로컬 디렉터리 생성

   ```shell
    mkdir -p /var/datasaker
    chmod 777 /var/datasaker/ 
   ```

2. 수집하고자 하는 로그를 로그 에이전트 컨테이너에 볼륨을 마운트한다.

3. 도커 명령어를 서버에 입력

   ```shell
   docker run -d --name saker-log-agent \
    -v /var/datasaker/:/var/datasaker/ \
    -v /var/lib/docker/containers/:/etc/datasaker/dsk-log-agent/log/containers/ \
    -e DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}\
    -e DSK_GLOBAL_GATEWAY_URL=${VAR_GLOBAL_GATEWAY_URL}\
    -e DSK_GLOBAL_AGENTMANAGER_URL=${VAR_GLOBAL_AGENTMANAGER_URL}\
    --restart=always\
    nexus2.exem-oss.org/saas/dsk-log-agent \
    --global.config=/etc/datasaker/global-config.yml \
    --agent.config=/etc/datasaker/dsk-log-agent/agent-config.yml \
    --transport.tls.ca=/etc/datasaker/dsk-log-agent/cert/ca-cert.pem \
    --transport.tls.cert=/etc/datasaker/dsk-log-agent/cert/client-cert.pem \
    --transport.tls.key=/etc/datasaker/dsk-log-agent/cert/client-key.pem
   ```
