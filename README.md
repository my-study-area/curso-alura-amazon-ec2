# curso-alura-amazon-ec2
Curso de Deploy no Amazon EC2: Alta disponibilidade e escalabilidade de uma aplicação

## Anotações
```bash
# inicia o localstack
docker-compose up

# lista as imagens
aws --endpoint-url=http://localhost:4566 ec2 describe-images --owners amazon

# lista exibindo somente o ImageId das imagens
aws --endpoint-url=http://localhost:4566 ec2 describe-images --owners amazon --query 'Images[*].[ImageId]'

# lista exibindo somente ImageId, ImageLocation, Platform das imagens
aws --endpoint-url=http://localhost:4566 ec2 describe-images --owners amazon --query 'Images[*].[ImageId,ImageLocation,Platform]'

# cria chave para acesso remoto via ssh
aws --endpoint-url=http://localhost:4566 ec2 create-key-pair --key-name chave-aws

# cria uma instância com chave
aws --endpoint-url=http://localhost:4566 ec2 run-instances --image-id ami-a1bf2ecd\
 --instance-type t2.micro --key-name chave-aws

# autoriza grupo de segurança acessar instância
aws --endpoint-url=http://localhost:4566 ec2 authorize-security-group-ingress\
 --group-name "default" --protocol tcp --port 22 --cidr 0.0.0.0/0

# cria uma instância sem chave
aws --endpoint-url=http://localhost:4566 ec2 run-instances --image-id ami-a1bf2ecd --instance-type t2.micro

# lista as instâncias em execução
aws --endpoint-url=http://localhost:4566 ec2 describe-instances

# para uma instância
aws ec2 --endpoint-url=http://localhost:4566 stop-instances\
 --instance-ids <INSTANCEID>

# lista as instâncias exibindo o InstanceId  e o State
aws --endpoint-url=http://localhost:4566 ec2 describe-instances\
 --query='Reservations[*].Instances[*].[InstanceId,State]'

# inicia uma instância parada
aws --endpoint-url=http://localhost:4566 ec2 start-instances\
 --instance-ids <INSTANCEID>

# exclui uma instância
aws --endpoint-url=http://localhost:4566 ec2 terminate-instances\
 --instance-ids <INSTANCEID>
```
## Create an IPv4-enabled VPC and subnets using the AWS CLI
```bash
aws ec2 create-vpc \
--endpoint-url http://localhost:4566 \
--cidr-block 10.0.0.0/16 --query Vpc.VpcId --output text

aws ec2 create-subnet \
--endpoint-url http://localhost:4566 \
--vpc-id vpc-6674dfc5 --cidr-block 10.0.1.0/24

aws ec2 create-subnet \
--endpoint-url http://localhost:4566 \
--vpc-id vpc-6674dfc5 --cidr-block 10.0.0.0/24

aws ec2 create-internet-gateway \
--endpoint-url http://localhost:4566 \
--query InternetGateway.InternetGatewayId --output text

aws ec2 attach-internet-gateway \
--endpoint-url http://localhost:4566 \
--vpc-id vpc-6674dfc5 --internet-gateway-id igw-c245b8e3

aws ec2 create-route-table \
--endpoint-url http://localhost:4566 \
--vpc-id vpc-6674dfc5 --query RouteTable.RouteTableId --output text

aws ec2 create-route \
--endpoint-url http://localhost:4566 \
--route-table-id rtb-e316b2b4 \
--destination-cidr-block 0.0.0.0/0 \
--gateway-id igw-c245b8e3

aws ec2 describe-route-tables \
--endpoint-url http://localhost:4566 \
--route-table-id rtb-e316b2b4

aws ec2 describe-subnets \
--endpoint-url http://localhost:4566 \
--filters "Name=vpc-id,Values=vpc-6674dfc5" \
--query "Subnets[*].{ID:SubnetId,CIDR:CidrBlock}"

aws ec2 associate-route-table \
--endpoint-url http://localhost:4566 \
--subnet-id subnet-a385aede \
--route-table-id rtb-e316b2b4

aws ec2 modify-subnet-attribute \
--endpoint-url http://localhost:4566 \
--subnet-id subnet-a385aede \
--map-public-ip-on-launch

aws ec2 create-security-group \
--endpoint-url http://localhost:4566 \
--group-name SSHAccess \
--description "Security group for SSH access" \
--vpc-id vpc-6674dfc5

aws ec2 authorize-security-group-ingress \
--endpoint-url http://localhost:4566 \
--group-id sg-b8a32889d3008994d \
--protocol tcp \
--port 22 \
--cidr 0.0.0.0/0

aws ec2 run-instances \
--endpoint-url http://localhost:4566 \
--image-id ami-a4827dc9 \
--count 1 \
--instance-type t2.micro \
--security-group-ids sg-b8a32889d3008994d \
--subnet-id subnet-a385aede

aws ec2 describe-instances \
--endpoint-url http://localhost:4566 \
--instance-id i-577ba6a3656d885ee \
--query "Reservations[*].Instances[*].{State:State.Name,Address:PrivateIpAddress}"

aws ec2 describe-instances \
--endpoint-url http://localhost:4566 \
--query "Reservations[*].Instances[*].\
{State:State.Name,Address:PublicIpAddress,AddressPPP:PrivateIpAddress}"
```
## Links
- [Create an IPv4-enabled VPC and subnets using the AWS CLI](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-subnets-commands-example.html)
