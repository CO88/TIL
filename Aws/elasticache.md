# Elasticache
---
Aws에서 제공해주는 2가지 캐시 엔진이 있습니다.

* **Redis** 
* **Memcached**

두 캐시 엔진은 유명한 In-Memory Data Storage이며, 많이 비교되는 시스템입니다.

관련 글을 찾다가 Redis의 개발자 Salvatore Sanfilippo가 Redis와 Memcached를 
다음과 같은 포인트로 비교한 글을 봤습니다.

* 지원하는 데이터 구조
* 메모리 사용 효율성 비교
* 성능 비교 

여기서 redis를 사용한 이유는 **성능 비교**에서 Memcached와 redis의 차이를 보고 끄덕이게 되었습니다.

> Redis는 단일 코어, Memcached는 멀티 코어를 사용하고, 평균적으로 Redis는 코어 측면에서 측정했을 때 소규모 데이터 스토리지에서 Memecahced보다 높은 성능을 자랑합니다.

> Memcached는 100k 이상의 데이터를 저장할 때 Redis보다 성능이 뛰어납니다.

<br/>

## aws의 elasticache redis의 클러스터를 생성하면서 주의점
---

1. elasticache redis 클러스터의 sg를 따로 생성해서 6379 port를 열어주어야 제공되는 엔드포인트 주소로 접속이 됩니다.

2. RDS와 다르게 Elastic Cahce의 캐시 엔진은 AWS 외부에서 접속을 할 수 없습니다.
그래서 Security Group에서 모든 IP로 inbound로 설정해줘도 동일한 VPC에 속한 EC2 인스턴스만 접속할 수 있습니다.