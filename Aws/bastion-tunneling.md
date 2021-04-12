
# SSH 터널링을 통해 private subnet에 있는 instance 접속

VPC를 만들땐 public subnet / private subnet을 생성할 수 있습니다.
public subnet은 이름에서 알 수 있듯이 public이여서 외부에서 접근을 할 수 있습니다.
하지만, private subnet은 외부에서 직접 액세스를 할 수 없습니다.

이는 SSM이라는 Session Manager로 aws console에서 바로 접속이 가능합니다. -> [SSM](Aws/ssm.md)
>이외의 방법인 bastions host를 통한 SSH 터널링 방법을 여기서 소개하려고 합니다.

public subnet에 bastion에 해당하는 ec2를 생성합니다. 
생성할땐, keypair를 지정해서 생성해줍니다.
이 ec2에 sg(security group)에는 inbound / outbound를 TCP, 22port로 각각 열어줍니다.
보안을 위해 inbound에 allowed cidr block으로 본인 또는 회사 ip로 접근할 수 있도록 수정해줍니다

private subnet에 존재하는 ec2에 sg에 TCP, 22port를 열어주고 allowed를 위에서 생성한 public에 있는 ec2 sg로 지정해줍니다 (보안을 위함)

준비는 끝났습니다.

생성된 keypair를 준비물로

ssh -i ${keypair} -L 22:${private_subnet_ec2_ip}:22 ${ec2-user}@{bastion_eip} 의 command를 실행한 후에 (창을 닫지 않습니다)

> ssh -i ${keypair} -L ${local_port}:${private_subnet_ec2_ip}:${remote_port} ${ec2-user}@{bastion_eip}

다른 cmd창을 띄어 ssh -i ${keypair} ${ec2-user}@localhost 의 command를 실행하면 private subnet에 존재하는 ec2에 접속을 할 수 있습니다!!

