---
categories: [Infra, Monitoring]
tags: [개념, 모니터링, 프로메테우스]
---


<aside>
💡 모니터링 시스템 구축에 앞서 Prometheus를 소개하려 한다.

</aside>

# Prometheus 란?

1. 오픈소스 기반의 모니터링 시스템이다.
2. 주로 CPU, 메모리 사용량 같은 Metric 데이터에 대한 모니터링이 목적이다.
  → MSA 형태의 서비스, 대규모 서버 클러스터 모니터링 등에서 사용한다.

## 장점

1. 구조가 간단하여 운영이 쉽다.
    - 아키텍처 자체가 정말 간단하다. 설치 및 설정 또한 간단하여 별 다른 부담 없이 시작했다.
2. 많은 시스템을 모니터링 할 수 있는 다양한 플러그인을 가지고 있다.
    - node, jenkins, redis 등등 다양한 시스템의 exporter가 존재하여 다양한 시스템의 모니터링을 할 수 있다.
3. 기본적으로 pulling 방식을 사용한다.
    - 다른 모니터링 시스템은 pushing 방식인 반면 Prometheus는 pulling 방식이다. 모니터링 대상 시스템의 부하를 줄일 수 있다.
4. 강력한 쿼리를 지원한다.
    - Prometheus는 PromQL이라는 Prometheus Query가 있는데 사용법이 쉽진 않으나 데이터를 많은 방법으로 가공할 수 있다.
5. Grafana와 연동하여 시각화가 쉽고 여러 샘플 대시보드가 있다.
    - Prometheus를 도입한 이유 중 큰 비중을 차지한다. 많은 정보를 받고 있어도 뭘 어떻게 봐야할 지를 모른다면 쓸모가 없기 때문이다.

## 단점

1. 클러스터링이 불가능하다.
    
    ![Prometheus 다중화 방식](/assets/prometheus/그림3.jpg)
    
    - Master, Slave 구조로 각 Region에 Slave Prometheus 서버를 두고 Master Prometheus에서 Aggregate하는 방식이 공식적으로 권장하는 다중화 방식으로 클러스트링과는 거리가 멀다.
2. 수집서버가 멈추면 그 시간동안의 메트릭 정보는 볼 수 없다.
    - pulling 방식과 로컬디스크에 데이터를 저장하는 방식이 이유다.
3. 싱글 호스트 방식의 아키텍처로 인해 저장용량이 부족하면 디스크를 늘리는 방법 밖에 없다.

---

# 아키텍처

![Prometheus 아키텍처](/assets/prometheus/Untitled.png)

Prometheus 아키텍처

## 동작 순서

1. Prometheus 서버에서 Retrieval이 모니터링 대상의 exporter로 부터 주기적으로 메트릭 정보를 요청한다.
2. Retrieval이 받아온 정보를 TSDB에게 전달하고 TSDB는 받은 정보를 스토리지에 저장한다.
    
    <aside>
    📌 TSDB는 Prometheus의 핵심 모듈로 스토리지를 관리한다.
    
    </aside>
    
3. Prometheus UI 페이지나 Grafana에서 Promtheus 서버에 정보를 요청하면 스토리지에 저장해둔 정보를 넘겨준다. Prometheus UI 페이지나 Grafana는 넘겨 받은 정보들로 시각화 시켜준다.
    
    <aside>
    📌 Prometheus 서버에 정보를 요청 할 때는 PromQL을 사용하여 요청한다.
    
    </aside>
    
4. Prometheus는 알림 기능도 지원하여 PromQL로 작성한 조건을 만족할 때 알림을 보낸다.

---

# 구성도

![구성도](/assets/prometheus/캡처5.png)

초기 구성도는 간단하게 잡아 보았다. 앞으로 글들에서 설치 및 구성을 하나씩 설명하려고 한다.
