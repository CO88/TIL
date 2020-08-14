Terraform - elasticache
===

terraform을 이용해서 elasticache 서비스를 붙여봤습니다.

이를 위해서 아래 같은 모듈이 필요합니다.

```
1. vpc
2. subnet
3. security_group
```

>[Terraform-elasticache](https://registry.terraform.io/modules/cloudposse/elasticache-redis/aws/0.18.1) 를 참조하시면 어떤 값이 **required** 인지 **optional**인지 알 수 있습니다.

<br/>
위 링크에선 elasticache module 자체에서 sg를 만들기 때문에 security_group을 작성하진 않았습니다. 하지만, sg도 관리를 하기 위해서 아래와 같이 security_group 모듈을 생성해서 인바운드 규칙, 아웃바운드 규칙을 정하는 것이 좋습니다.

```go
module "sg" {
  source  = "terraform-aws-modules/security-group/aws"

  ...

  //인바운드 규칙
  ingress_with_cidr_blocks = [
    {
      ...
    }
  ]

  //아웃바운드 규칙
  egress_with_cidr_blocks = [
    {
      ...
    }
  ]
}
```