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
```
