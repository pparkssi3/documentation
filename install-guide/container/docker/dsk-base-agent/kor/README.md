# Coming soon

<!--
# 도커 환경에 기본 에이전트 설치 하기

도커 환경에 **기본 에이전트**를 설치하는 방법을 설명합니다.

1. 데이터세이커가 사용할 로컬 디렉터리 생성

   ```shell
    mkdir -p /var/datasaker
    chmod 755 /var/datasaker/ 
   ```

2. 도커 명령어를 서버에 입력

   ```shell
    docker run -d --name saker-node-agent\
     -v /var/datasaker/:/var/datasaker/\
     -v /proc/:/host/proc/\
     -v /sys/:/host/sys/\
     -v /:/host/root/:ro,rslave\
     -e DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}\
     -e DSK_GLOBAL_GATEWAY_URL=${VAR_GLOBAL_GATEWAY_URL}\
     -e DSK_GLOBAL_AGENTMANAGER_URL=${VAR_GLOBAL_AGENTMANAGER_URL}\
     --network=host\
     --pid=host\
     --restart=always\
     nexus2.exem-oss.org/saas/dsk-node-agent\
     --path.procfs=/host/proc\
     --path.sysfs=/host/sys\
     --path.rootfs=/host/root\
     --collector.filesystem.ignored-mount-points="^/(dev|proc|sys|run|var/lib/docker/.+|var/lib/kubelet/pods/.+)($|/)"\
     --collector.tcpstat\
     --web.listen-address=:9110 & \
    docker run -d --name saker-container-agent\
     -v /var/datasaker/:/var/datasaker/\
     -v /:/rootfs/:ro\
     -v /var/run/:/var/run/:ro\
     -v /sys/:/sys/:ro\
     -v /dev/disk/:/dev/disk/:ro\
     -e DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}\
     -e DSK_GLOBAL_GATEWAY_URL=${VAR_GLOBAL_GATEWAY_URL}\
     -e DSK_GLOBAL_AGENTMANAGER_URL=${VAR_GLOBAL_AGENTMANAGER_URL}\
     --privileged\
     --restart=always\
     nexus2.exem-oss.org/saas/dsk-container-agent\
     --logtostderr=true\
     --port=4194\
     --disable_metrics=accelerator,cpu_topology,disk,memory_numa,tcp,udp,percpu,sched,process,hugetlb,referenced_memory,resctrl,cpuset,advtcp\
     --disable_root_cgroup_stats=true\
     --store_container_labels=false\
     --docker_only=true 
   ```
-->