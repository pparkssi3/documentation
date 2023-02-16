# 도커 환경에 에이전트 설치하기

**데이터세이커 몽고 에이전트**를 도커 환경에 설치하는 방법을 설명합니다.

1. **데이터세이커**가 사용할 로컬 디렉터리 생성.

   ```shell
    mkdir -p /var/datasaker
    chmod 777 /var/datasaker/ 
   ```

2. 수집하고자 하는 몽고DB 서버의 호스트, 포트 주소를 설정한다.

   ```shell
    export DSK_MONGO_URI=mongodb://<user>:<password>@<host>:<port>
   ```

   예를 들어, localhost 27017 포트에 서비스중인 몽고DB를 수집하기 위해서는 다음과 같이 설정합니다.

   ```shell
    export DSK_MONGO_URI=mongodb://localhost:27017
   ```

3. 도커 명령어를 서버에 입력.

   ```shell
    docker run -d --name saker-mongo-agent\
    -v /var/datasaker/:/var/datasaker/\
    -e DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}\
    -e DSK_GLOBAL_GATEWAY_URL=${VAR_GLOBAL_GATEWAY_URL}\
    -e DSK_GLOBAL_AGENTMANAGER_URL=${VAR_GLOBAL_AGENTMANAGER_URL}\
    --restart=always\
    nexus2.exem-oss.org/saas/dsk-mongo-agent\
    --mongodb.uri=${DSK_MONGO_URI}\
    --collect-all\
    --compatible-mode
   ```
