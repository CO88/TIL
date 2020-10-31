# AWS RDS를 외부에서 접속하기

외부에서 접속하는 방법에는 여러가지가 존재하지만 <br>
이 글에는 **private subnet에 rds가 속해있을 때 접속하는 방법을 적겠습니다.**

aws rdb에 접속하기 위해선 <br>
RDS > 데이터베이스 > {접속할 데이터베이스} <br>
을 선택하여 해당 데이터베이스가 public access 속성을 enable해줘야합니다.

public access 속성을 enable하려면 속해있는 vpc에 DNS 호스트 이름을 설정해줘야합니다.
하지만 제가 사용하는 환경은 private subnet이며, DNS 호스트 이름은 비활성화 되었고, 외부에서 접속을 하려면
aws site-to-site VPN, aws direct connect등 을 이용해야합니다.

이전에 private subnet에 있는 ec2에 접속하려고 bastion 모듈을 이용하였습니다. <br>
만들었던 bastion 모듈을 이용해 ssh 터널링을 하여 aws rds에 접근을 하도록 설정을 해줘 rds에 접속을 할 수 있었습니다. 

기존에 bastion sg(security group)에 ingress와 egress는 22포트(ssh접속을 위해)만 열려있는 상태였기에 egress에 mariaDB에서 사용하는 3306포트를 추가하였습니다.

