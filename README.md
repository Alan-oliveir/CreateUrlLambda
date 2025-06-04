# URL Shortener - AWS Lambda

Um encurtador de URLs serverless desenvolvido com Java e AWS Lambda, criado durante evento de programação. O projeto demonstra a implementação de uma arquitetura serverless moderna utilizando serviços AWS.

## Tecnologias Utilizadas

- **Java 17** - Linguagem de programação principal
- **AWS Lambda** - Computação serverless
- **Amazon S3** - Armazenamento de dados
- **Maven** - Gerenciamento de dependências e build
- **Jackson** - Serialização/deserialização JSON
- **Lombok** - Redução de boilerplate code
- **AWS SDK v2** - Integração com serviços AWS

## Funcionalidades

- ✅ Criação de URLs encurtadas com código único (8 caracteres)
- ✅ Armazenamento seguro no Amazon S3
- ✅ Configuração de tempo de expiração para URLs
- ✅ API REST serverless com AWS Lambda
- ✅ Processamento de requisições JSON

## Arquitetura

```
Cliente → API Gateway → AWS Lambda → Amazon S3
```

1. **Recebimento**: Lambda recebe requisição com URL original e tempo de expiração
2. **Processamento**: Gera código único de 8 caracteres usando UUID
3. **Armazenamento**: Salva dados no S3 em formato JSON
4. **Resposta**: Retorna código da URL encurtada

## Estrutura do Projeto

```
src/main/java/com/rocketseat/createUrlLambda/
├── Main.java          # Handler principal do Lambda
└── UrlData.java       # Modelo de dados da URL
```

## Como Executar

### Pré-requisitos

- Java 17+
- Maven 3.6+
- Conta AWS configurada
- AWS CLI instalado e configurado

### Build e Deploy

1. **Clone o repositório**
```bash
git clone <url do repositorio>
cd CreateUrlLambda
```

2. **Compile o projeto**
```bash
mvn clean compile
```

3. **Gere o JAR para deploy**
```bash
mvn package
```

4. **Configure o bucket S3**
```bash
aws s3 mb s3://aws-url-shortner-app
```

5. **Deploy no AWS Lambda**
- Faça upload do JAR gerado (`target/CreateUrlLambda-1.0-SNAPSHOT.jar`)
- Configure o handler: `com.rocketseat.createUrlShortner.Main::handleRequest`
- Adicione as permissões necessárias para S3

## Uso da API

### Endpoint
```
POST /create-short-url
```

### Payload da Requisição
```json
{
  "originalUrl": "https://exemplo.com/url-muito-longa",
  "expirationTime": "1640995200"
}
```

### Resposta
```json
{
  "code": "a1b2c3d4"
}
```

## Permissões AWS Necessárias

O Lambda precisa das seguintes permissões IAM:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::aws-url-shortner-app/*"
    }
  ]
}
```

## Exemplo de Dados Armazenados

```json
{
  "originalUrl": "https://exemplo.com/url-muito-longa",
  "expirationTime": 1640995200
}
```

## Melhorias Futuras

- [ ] Validação de URLs de entrada
- [ ] Sistema de cache com Redis
- [ ] Métricas e monitoramento
- [ ] Testes unitários e de integração
- [ ] Rate limiting
- [ ] Interface web para gerenciamento

## Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

---

**Nota**: Este projeto foi desenvolvido para fins educacionais e demonstração de habilidades em arquitetura serverless com AWS.
