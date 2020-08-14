Elasticache - phpredis 
===

laravel에서는 redis를 phpredis, predis 두 가지 패키지를 사용하여 개발할 수 있습니다.

라라벨 공식 홈페이지에서는 predis가 제거될 예정이였기에 phpredis로 바꿔서 elasticache를 연결하였습니다.

>{note} Predis는 패키지 원작자에 의해 버려졌습니다. 이후 라라벨 release에서 제거될 수 있습니다.

<br/>
predis는 composer로 간단히 추가를 할 수 있지만, **phpredis**는 php에서 redis extension을 설치해야하기 때문에 아래 코드 블록과 같은 작업을 해줘야 합니다.

```docker

# install phpRedis
RUN pecl install -o -f redis && \
    rm -rf /tmp/pear && \
    docker-php-ext-enable redis

```
<br/>

## 주의사항
---
predis와 달리 phpredis는 TLS통신을 제공하고 있지 않기에 elasticache에 endpoint를 그대로 사용하면 
redis와는 달리 aws에선 elasticache에 TLS을 지원하고 있기에 다음과 같은 에러를 뱉어냅니다.

```
read error on connection
```

그래서 endpoint에 tls://${REDIS_HOSTNAME} 를 붙여서 TLS을 사용하도록 해줘야합니다.

