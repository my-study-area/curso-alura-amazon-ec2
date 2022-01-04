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
```
