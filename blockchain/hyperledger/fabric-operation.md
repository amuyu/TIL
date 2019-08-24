
# 패브릭 운영을 위한 서비스 
- 익스플로러
- 프로메테우스
- 그라파나

# 운영을 위한 API
## listenAddress
confing/core.yaml 의 Operations 영역을 보면 listenAddress 라는 항목이 있다.
기본값으로 127.0.0.1:9443 으로 지정되어 있다. 필요에 따라 이 영역에서 TLS 적용을 설정할 수 있다.

> core.yaml? 아마 fabric 에서 볼수 있을 듯
> 오더러에도 동일하게 설정할 수 있다.

listenAddress 로 설정한 호스트와 포트는 운영 서비스에서 해당 노드에 접근하기 위한 엔드포인트이다.

이제 각 노드에 해당 설정을 적용해서 살펴보자
오더러 노드는 ORDERER_OPERATIONS_LISTENADDRESS 를 추가하고
피어 노드는 CORE_OPERATIONS_LISTENADDRESS 를 추가한다.
실습의 경우, 내부 포트sms 9443으로 하고 외부에서 각 노드로 접근하는 포트는 9443, 9100, 9200, 9300, 9400 포트로 한다.

## 로그 레벨 관리
로그 레벨 관리를 위한 API 는 /logspec 이다.
curl 로 peer 에 get/put api 를 호출할 수 있다.
```bash
# bash 로 접속한 후,
docker exec -it cli bash

# curl 을 받을 수 있도록 설치하고
wget 127.0.0.1:9443/logspec

# logspec 을 호출한다.
curl peer0.org1.example.com:9443/logspec
{"spec":"info"}
```

## 헬스 체크
health checks api 는 피어와 오더러를 비롯한 노드들이 잘 동작하는지 확인할 수 있다.
노드 상태 체크를 위한 API 엔드포인트는 /healthz 입니다.
```bash
curl peer0.org1.example.com:9443/healthz
{"status":"OK", "time":"2019-05-05T09:01:42.72"}
```

## 메트릭
Metrics API 는 시스템 로그와 같은 정보를 시간 경과에 따라 지속적으로 노출시킴으로써 운영자에게
블록체인 네트워크가 어떻게 동작하고 있는지를 보여준다.
api 엔드포인트는 /metrics 이다.

```bash
curl peer0.org1.example.com:9443/metrics
```

메트릭 정보를 수집해서 모니터링 서비스를 만들 수 있다.
ref : https://hyperledger-fabric.readthedocs.io/en/release-1.4/metrics_reference.html?highlight=metrics


# 프로메테우스 
프로메테우스는 사운드클라우드에서 만든 오픈소스 모니터링 솔루션으로 Go 언어로 개발됐습니다.
https://prometheus.io/docs/introduction/overview/

하이퍼레저 패브릭에서 제공하는 메트릭 데이터를 다양한 promQL 질의를 통해 결과를 집계하고 그래프로 시각화할 수 있다.


프로메테우스를 다른 모니터링 툴과 비교했을 때 가장 큰 차별점 중 하나는 메트릭 데이터를 타깃으로부터 가져온다는(pull) 것이라고 한다.
일반적으로 대부분의 모니터링 도구는 클라언트가 메트릭 데이터를 수집해서 서버로 보내는(push) 방식이다.

## How to run
docker 실행 방법은 다음과 같다
```bash
docker run --name prometheus -d -p 127.0.0.1:9090:9090 prom/prometheus
```

hylerledger 의 Metrics 정보와 연동하려면 prometheus.yml 을 설정해야 하고,
```bash
docker run -d --name prometheus-server -p 9090:9090 --restart always -v ./prometheus.yml:/prometheus.yml \
prom/prometheus \
--config.file=/prometheus.yml
```

ref: https://github.com/alerta/prometheus-config/blob/master/docker-compose.yml

## 프로메테우스 연동
core.yaml, orderer.yaml, docker-compose-base.yaml 파일을 수정할 필요가 있다.

이중에서 docker-compose-base.yaml 수정하는 부분을 보면
```yaml
# orderer.example.com#environment
- ORDERER_OPERATIONS_LISTENADDRESS=orderer.example.com:9443
- ORDERER_METRICS_PROVIDER=prometheus

# orderer.example.com#ports
- 9443:9443

# peer.org.example.com
- CORE_OPERATIONS_LISTENADDRESS=peer.org.example.com:9443
- CORE_METRIS_PROVIDER=prometheus

# peer.org.example.com#ports
# 외부 port 는 설정하기 나름
- 9100:9443
```

네트워크 설정한 후, 네트워크가 구동되면 `localhost:9443/metrics` 로 접속해 메트릭 데이터를 확인할 수 있다.
orderer 의 Metrics 정보를 가져온다

프로메테우스도 설정을 변경하고 실행해서 `localhost:9090/targets` 에 접속해 프로메테우스에 연결된 타킷이 제대로 연동되고 있는지 확인할 수 있다.
실행해보면 hyperledger_metrics 잡에 등록된 노드가 모두 비활성 상태라서 그렇다.
프로메테우스와 블록체인 네트워크를 연결해주는 작업이 필요하다.
```bash
docker network conenct net_byfn 39237bab7fe1
```
net_byfn 은 블록체인 네트워크 노드들의 네트워크 모드를 의미하고
39237bab7fe1 은 prometheus 의 컨테이너 아이디를 의미한다.
블록체인 docker container 들이 net_byfn 으로 묶여있어야 한다.
묶이게 되는 네트워크 이름을 확인하고 prometheus 의 container id 확인하고 실행
네트워크가 연결되면 state 가 up 으로 바뀝니다.

## promQL
샘플로 scrape_samples_scraped 질의를 통해 console 과 graph 를 확인할 수 있다.
fabric_version : 노드의 Version
endorser_successful_proposals : 엔도서에서 성공적으로 수행된 트랜잭션 확인


# 그라파나
그라파나는 프로메테우스와 함께 사용된다. 
커스터마이징과 더 전문적인 모니터링 서비스를 위해 그라파나를 사용한다고 한다.

## how to run
```bash
docker run -d -p 3000:3000 grafana/grafana
```
실행 한 후, localhost:3000 에 접속해서 확인할 수 있다.
기본 계정은 admin/admin 이다.

## 그라파나와 프로메테우스 연동
처음 로그인 하면 Home Dashboard 화면이 나타나고 데이터 소스를 추가해야 한다.
Add data source 버튼을 클릭하고 prometheus 를 클릭한다.

url 을 http://localhost:9090 으로 입력하고 HTTP Method 가 Get 으로 되어있는지 확인한 후, 저장한다.
프로메테우스와 블록치엔 노드 연결할 때 처럼 그라파나를 동일한 네트워크에 두고
http://prometheus_prometheus_1:9090 로 설정했다.

설정이 완료되면 메인화면에서 New dashboard 를 선택한다. 커스터마이징 한다고 함

## grafana dashboard
https://grafana.com/grafana/dashboards/10716


# 프로메테우스와 그라파나를 이용한 메트릭 모니터링


# ref
[blockchain-explorer](https://github.com/hyperledger/blockchain-explorer)
[operations guides](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html)
[Hyperledger Fabric Real-time Monitoring with 1.41](https://medium.com/@vkrishnan.ny/hyperledger-fabric-real-time-monitoring-with-1-41-9babf233f44e)