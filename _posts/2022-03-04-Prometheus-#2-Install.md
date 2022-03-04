---
categories: [Infra, Monitoring]
tags: [설치, 모니터링, 프로메테우스]
---


<aside>
💡 docker 환경 기반으로 구성할 것이다. 추후에는 kubernetes로 전환 할 것이다.

</aside>

# 설치

1. docker 설치 및 실행
    
    ```bash
    yum -y install docker
    
    systemtcl start docker
    ```
    
2. prometheus.yml 작성
    
    ```bash
    vi /<config-file-path>/prometheus.yml
    ```
    
    ```yaml
    global:
        # 전체 수집 주기 default 1m
        scrape_interval: 5s
        # 전체 수집 제한 시간 default 10s
        scrape_timeout: 1m
    # 수집 대상 configs
    scrape_configs:
        - job_name: 'prometheus'
          static_configs:
            - targets: ['IP:9090']
    ```
    
    - 참고
        
        [Prometheus Configuration](https://prometheus.io/docs/prometheus/latest/configuration/configuration/)
        
3. docker volume 생성
    
    ```bash
    docker volume create "prometheus-data"
    ```
    
4. docker run
    
    ```bash
    docker run -d -p 9090:9090 --name prometheus \
    -v /<config-file-path>/prometheus.yml:/opt/bitnami/prometheus/conf/prometheus.yml \
    -v "prometheus-data:/opt/bitnami/prometheus/data" \
    bitnami/prometheus:latest
    ```
    
5. IP:9090 접속
    
    ![접속 화면](/assets/prometheus/캡처.png)
