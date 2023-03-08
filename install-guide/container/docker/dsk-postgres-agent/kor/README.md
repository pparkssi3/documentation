# Coming soon

<!--
# 도커 환경에 데이터세이커 PostgreSQL 에이전트 설치하기

**데이터세이커 PostgreSQL 에이전트**를 도커 환경에 설치하는 방법을 설명합니다.

1. **데이터세이커**가 사용할 로컬 디렉터리 생성한다.

   ```shell
    mkdir -p /var/datasaker
    chmod 777 /var/datasaker/ 
   ```

2. 수집하고자 하는 PostgreSQL의 호스트, 포트 주소를 설정한다.

   ```shell
    export DSK_PG_URI=postgrseql://<user>:<password>@<host>:<port>/postgres?sslmode=[disable|require|verify-ca|verify-full]
   ```

   예를 들어, localhost 5432 포트에 서비스중인 PostgreSQL을 수집하기 위해서는 다음과 같이 설정합니다.

   ```shell
    export DSK_PG_URI=postgrseql://localhost:5432/postgres?sslmode=disable
   ```

3. 도커 명령어를 서버에 입력한다.

   ```shell
    docker run -d --name saker-postgres-agent\
     -v /var/datasaker/:/var/datasaker/\
     -e DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}\
     -e DSK_GLOBAL_GATEWAY_URL=${VAR_GLOBAL_GATEWAY_URL}\
     -e DSK_GLOBAL_AGENTMANAGER_URL=${VAR_GLOBAL_AGENTMANAGER_URL}\
     -e DATA_SOURCE_NAME=${DSK_PG_URI}\
     --network=host\
     --restart=always\
     nexus2.exem-oss.org/saas/dsk-postgres-agent
   ```
-->
