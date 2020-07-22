# terraform data.ft

Rdb, Acm 같은 고정정보, 자주 변하지 않는 정보는 remote state에서 정보를 가져올 수 있습니다. 

```
data "terraform _remote_state" "NAME" {
    ...
}
```
data로 정의해서 s3에서 가져와서 사용하고, 이러한 정의는 data.ft에 정의합니다.