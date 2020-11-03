# aws rds에 dump하여 export 또는 import

개발용 데이터를 넣거나 다른 서버, 클라우드(ncp,gcp등..)에서 aws rds로 옮길때, <br/>
aws rds로 import해야 할 때, export해야할 때가 있습니다. 

저는 >[aws rds로 외부접속](https://github.com/blanccobb/TIL/blob/master/Aws/rds-external-connection.md) 방법을 이용하여 aws rds로 import 또는 export를 진행하였습니다.

기본적으로 aws rds는 private subnet에 있다고 하고 외부에서 접속을 해야합니다. 그러려면 다양한 방법이 필요하겠지만 ssh 터널링을 이용해서 접속하겠습니다. 

<br>

기본적인 흐름은 다음과 같겠습니다.<br/>


> local ->(1) bastion ec2(public) -->(2) aws rds(private) 

(1)에 해당하는 작업은 private key를 이용하여 다음 command를 통하여 bastion ec2에 접속합니다.

```sh
ssh -i ${private_key} ${bastion ec2 username}@${bastion eip}
```
* bastion에는 mysqldump 커맨드를 사용할 수 있게 mysql이 설치되어있어야합니다.

(1)후엔 bastion에 접속이 되었는데 여기서 또 따로 명령어를 날리는 것보단 (1)과 (2)의 내용을 포함하는 command를 작성하는게 좋습니다.

```sh
ssh -i ${private_key} ${bastion_ec2_username}@${bastion_eip} \
    mysqldump \
    -h${rds_host} -u${rds_username} --password \
    --database ${database_name} > dump.sql
```
위 커맨드를 실행하면 bastion에 ${database name}에 dump를 얻을 수 있습니다.

또 아래는 반대로 외부 dump파일을 aws rds로 import 하는 부분입니다.

```sh
ssh -i ${private_key} ${bastion_ec2_username}@${bastion_eip} \
    mysql \
    -h${target_rds_host} -u${rds_username} --password \
    --database ${target_database_name} < dump.sql