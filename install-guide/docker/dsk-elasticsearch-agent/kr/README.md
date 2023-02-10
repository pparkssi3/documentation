# 도커 환경에 에이전트 설치하기

**데이터세이커 엘라스틱서치 에이전트**를 도커 환경에 설치하는 방법을 설명합니다.

1. 데이터세이커가 사용할 로컬 디렉터리 생성

   ```shell
    mkdir -p /var/datasaker
    chmod 777 /var/datasaker/ 
   ```

2. 수집하고자 하는 엘라스틱서치 서버의 호스트, 포트 주소를 설정한다.

   ```shell
    export DSK_ES_URI=<protocol>://<user>:<password>@<host>:<port>
   ```

   예를 들어, localhost 9200 포트에 서비스중인 엘라스틱서치 서버를 수집하기 위해서는 다음과 같이 설정합니다.

   ```shell
    export DSK_ES_URI=http://localhost:9200
   ```

3. 도커 명령어를 서버에 입력

   ```shell
    docker run -d --name saker-elasticsearch-agent\
    -v /var/datasaker/:/var/datasaker/\
    -e DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}\
    -e DSK_GLOBAL_GATEWAY_URL=${VAR_GLOBAL_GATEWAY_URL}\
    -e DSK_GLOBAL_AGENTMANAGER_URL=${VAR_GLOBAL_AGENTMANAGER_URL}\
    --restart=always\
    nexus2.exem-oss.org/saas/dsk-elasticsearch-agent\
    --es.uri=${DSK_ES_URI}\
    --es.all\
    --es.indices\
    --es.indices_settings\
    --es.indices_mappings\
    --es.cluster_settings\
    --es.shards\
    --es.snapshots
   ```
