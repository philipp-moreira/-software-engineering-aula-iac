# Exemplo 3 - SQS (Simple Queue Service)

Este exemplo cria três filas SQS no LocalStack:
- Topico1
- Topico2  
- Topico3

## Recursos Criados

- 3 filas SQS com configurações padrão
- Tags para identificação
- Outputs com URLs e ARNs das filas

## Como Executar

1. Certifique-se que o LocalStack está rodando:
```bash
docker-compose up -d
```

2. Execute o Terraform:
```bash
cd exemplo3-sqs
terraform init
terraform plan
terraform apply
```

3. Veja os outputs:
```bash
terraform output
```

## Como Testar

### Listar filas criadas:

>>> O comando abaixo (item 04), só é possível a partir do container que possui a imagem do localstack e emula a cloud da aws.

- 01 - Abra um novo terminal (ou nova aba no terminal)
- 02 - Rode o comando abaixo para se conectar ao shell do container
```bash
docker compose exec localstack sh
```
- 03 - Você deve ter se conectado ao shell do container, e deve estar vendo algo similar a:
```bash
#
```
- 04 - Agora, já conectado ao shell do container que contém a emulação do serviço/cloud AWS, rode o comando abaixo no shell
```bash
aws --endpoint-url=http://localhost:4566 sqs list-queues
```

### Enviar mensagem para uma fila:
```bash
aws                                                               \
  --endpoint-url=http://localhost:4566 sqs send-message           \
  --queue-url http://localhost:4566/000000000000/Topico1          \
  --message-body "Mensagem de teste para Topico1"
```

### Receber mensagens de uma fila:
```bash
aws                                                               \
  --endpoint-url=http://localhost:4566 sqs receive-message        \
  --queue-url http://localhost:4566/000000000000/Topico1
```

### Ver atributos de uma fila:
```bash
aws                                                               \
  --endpoint-url=http://localhost:4566 sqs get-queue-attributes   \
  --queue-url http://localhost:4566/000000000000/Topico1          \
  --attribute-names All
```

## Limpeza

Para destruir os recursos:
```bash
terraform destroy -auto-approve
```
