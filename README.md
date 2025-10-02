# Desafio AWS: Arquitetura Serverless com S3 e Lambda

## Descrição da Arquitetura
Minha solução implementa uma arquitetura serverless na AWS que processa automaticamente imagens enviadas para um bucket S3. Quando um usuário faz upload de uma imagem, uma função Lambda é acionada para criar uma miniatura (thumbnail) da imagem, que é então armazenada em um bucket S3 separado.

![Arquitetura](images/Arquitetura.svg)

## Componentes da Arquitetura
- **Amazon S3 (Bucket de Entrada):** Armazena as imagens originais enviadas pelos usuários  
- **AWS Lambda:** Função serverless que processa as imagens e gera thumbnails  
- **Amazon S3 (Bucket de Saída):** Armazena as miniaturas processadas  
- **Amazon CloudWatch:** Serviço de monitoramento e logging para rastrear a execução  
- **AWS IAM:** Gerenciamento de permissões e políticas de acesso entre os serviços  

## Fluxo de Processamento
1. O usuário faz upload de uma imagem para o bucket S3 de entrada  
2. O S3 detecta o novo arquivo e dispara automaticamente a função Lambda  
3. A Lambda baixa a imagem, redimensiona para criar uma miniatura (thumbnail)  
4. A miniatura processada é salva no bucket S3 de saída  
5. Logs de execução são registrados no CloudWatch para monitoramento  

## Conceitos Fundamentais
- **Amazon S3 (Simple Storage Service):** Serviço de armazenamento de objetos escalável e durável. Oferece alta disponibilidade e segurança para dados.  
- **AWS Lambda:** Serviço de computação sem servidor que executa código em resposta a eventos sem provisionar ou gerenciar servidores.  
- **CloudWatch:** Serviço de monitoramento e observabilidade para recursos e aplicações em nuvem.  
- **Processamento de Imagens:** Redimensionamento e criação de thumbnails diretamente na função Lambda, usando Node.js  

## Vantagens da Arquitetura Escolhida
- Escalabilidade automática: Adapta-se automaticamente à carga de trabalho  
- Custo-efetividade: Paga apenas pelo tempo de execução e recursos utilizados  
- Baixa manutenção: Infraestrutura totalmente gerenciada pela AWS  
- Alta disponibilidade: Recursos distribuídos em múltiplas zonas de disponibilidade  
- Processamento especializado: Foco em um caso de uso específico (thumbnails de imagens)  

## Implementação
O processo de implementação incluiu:  
- Criação de dois buckets S3 (entrada e saída) com configurações apropriadas  
- Desenvolvimento da função Lambda em Node.js para redimensionar as imagens 
- Configuração do trigger do S3 para a função Lambda  
- Definição de políticas IAM para permissões entre serviços  

## Insights e Aprendizados
Criar esse desenho de arquitetura me ajudou a entender como soluções serverless podem facilitar o processamento de mídia de forma prática. Para casos como criar thumbnails, percebi que processar imagens direto no Lambda seria muito mais simples do que usar um banco de dados. A integração entre S3 e Lambda é uma opção rápida, confiável e adequada para esse tipo de tarefa. 

-----------------------------------------------------------------------------------

# Complementando para o desafio "executando tarefas automatizadas com Lambda Function e S3" - estudos sobre LocalStack.

# LocalStack - Desenvolvendo e Testando AWS Localmente

##  Meus Primeiros Passos com LocalStack

Antes de mergulharmos nos detalhes do LocalStack, quero compartilhar minha experiência prática com essa ferramenta incrível. Recentemente, implementei minhas primeiras soluções usando LocalStack para criar e testar filas SQS localmente.

**Confira meu repositório com a implementação completa:**
[**sqs-localstack-solution**](https://github.com/almeidapietra/sqs-localstack-solution)

Neste projeto, explorei como:
- Configurar o LocalStack para simular serviços AWS
- Criar e gerenciar filas SQS localmente
- Desenvolver e testar aplicações sem custos com a AWS real
- Implementar um ambiente de desenvolvimento totalmente offline

Essa experiência me mostrou na prática os benefícios que vou detalhar a seguir!

---

## O que é LocalStack?

LocalStack é uma ferramenta de código aberto que fornece um ambiente local de nuvem AWS totalmente funcional. Ele permite desenvolver e testar aplicações AWS sem precisar conectar-se à nuvem real, executando serviços AWS localmente em sua máquina.


## Para que serve o LocalStack?

### Desenvolvimento Local
- **Testar aplicações** que usam serviços AWS sem custos
- **Desenvolver offline** quando não há conexão com internet
- **Simular ambientes AWS** completos localmente

### Testes e CI/CD
- **Testes automatizados** sem depender da AWS real
- **Ambientes isolados** para cada desenvolvedor
- **Integração contínua** sem custos de serviços cloud

### Aprendizado e Experimentação
- **Estudar serviços AWS** sem gastar créditos
- **Testar configurações** arriscadas sem impacto
- **Prototipar rapidamente** novas ideias

## Benefícios de Usar LocalStack

### Econômico
- **Completamente gratuito** para uso básico
- **Elimina custos** de serviços AWS durante desenvolvimento
- **Não consome créditos** da conta AWS real

### Produtividade
- **Desenvolvimento mais rápido** sem latência de rede
- **Iteração rápida** com reinício instantâneo de serviços
- **Debug mais fácil** com logs locais detalhados

### Segurança
- **Ambiente isolado** sem risco de afetar produção
- **Dados locais** sem preocupação com vazamento
- **Teste de falhas** sem impacto em ambientes reais

## Como o LocalStack Funciona?

### Arquitetura
LocalStack cria containers Docker que emulam serviços AWS específicos. Cada serviço (S3, Lambda, DynamoDB, etc.) roda em um container separado, simulando o comportamento real da AWS.

### Serviços Suportados
- **Computação**: Lambda, ECS
- **Armazenamento**: S3, EBS
- **Banco de Dados**: DynamoDB, RDS
- **Mensageria**: SQS, SNS
- **API**: API Gateway
- **E muitos outros**

## Como Usar LocalStack

### Instalação

**Via Docker (Recomendado):**
```bash
# Instalar Docker
# Baixar imagem do LocalStack
docker pull localstack/localstack

# Executar container
docker run -d -p 4566:4566 -p 4571:4571 localstack/localstack
```

**Via pip:**
```bash
pip install localstack
localstack start
```

### Configuração Básica

**Configurar AWS CLI para LocalStack:**
```bash
aws configure set aws_access_key_id test
aws configure set aws_secret_access_key test
aws configure set default.region us-east-1
aws configure set default.output json
```

**Endpoint para LocalStack:**
```bash
export AWS_ENDPOINT_URL=http://localhost:4566
```

### Exemplo Prático - Lambda + S3

**Criar bucket S3 local:**
```bash
aws --endpoint-url=http://localhost:4566 s3 mb s3://meu-bucket-local
```

**Criar função Lambda local:**
```python
# lambda_function.py
import json
import boto3

def lambda_handler(event, context):
    s3 = boto3.client('s3', endpoint_url='http://localhost:4566')
    
    # Listar buckets
    buckets = s3.list_buckets()
    
    return {
        'statusCode': 200,
        'body': json.dumps({
            'buckets': [bucket['Name'] for bucket in buckets['Buckets']]
        })
    }
```

**Configurar Lambda local:**
```bash
# Zip da função
zip function.zip lambda_function.py

# Criar função Lambda
aws --endpoint-url=http://localhost:4566 lambda create-function \
    --function-name minha-funcao-local \
    --runtime python3.8 \
    --handler lambda_function.lambda_handler \
    --zip-file fileb://function.zip \
    --role arn:aws:iam::123456789012:role/lambda-role
```

## Integração com Desenvolvimento

### Ambiente de Desenvolvimento
```python
# config.py
import os

def get_aws_config():
    if os.getenv('ENVIRONMENT') == 'local':
        return {
            'endpoint_url': 'http://localhost:4566',
            'aws_access_key_id': 'test',
            'aws_secret_access_key': 'test',
            'region_name': 'us-east-1'
        }
    else:
        return {
            'region_name': 'us-east-1'
        }
```

### Testes Automatizados
```python
# test_my_app.py
import boto3
import pytest
from my_app import process_s3_file

@pytest.fixture
def s3_client():
    return boto3.client('s3', endpoint_url='http://localhost:4566')

def test_process_s3_file(s3_client):
    # Setup
    s3_client.create_bucket(Bucket='test-bucket')
    s3_client.put_object(Bucket='test-bucket', Key='test.txt', Body='test content')
    
    # Test
    result = process_s3_file('test-bucket', 'test.txt')
    
    # Assert
    assert result == 'TEST CONTENT'
```

## Casos de Uso Comuns

### 1. Desenvolvimento de Aplicações Serverless
- Testar funções Lambda localmente
- Simular triggers de S3, DynamoDB, etc.
- Desenvolver aplicações completas offline

### 2. Testes de Integração
- Testar interações entre serviços AWS
- Validar arquiteturas complexas
- Garantir qualidade antes do deploy

### 3. Demonstrações e Workshops
- Mostrar funcionalidades AWS sem custos
- Workshops em locais sem internet
- Treinamentos corporativos

## Limitações e Considerações

### O que funciona bem:
- Serviços principais (S3, Lambda, DynamoDB)
- Operações básicas de CRUD
- Desenvolvimento e testes locais

### Limitações:
- Alguns serviços avançados podem não estar totalmente implementados
- Performance diferente da AWS real
- Recursos específicos de regiões podem variar

## Melhores Práticas

### 1. Configuração de Ambiente
```bash
# .env
ENVIRONMENT=local
AWS_ENDPOINT_URL=http://localhost:4566
AWS_ACCESS_KEY_ID=test
AWS_SECRET_ACCESS_KEY=test
```

### 2. Scripts de Desenvolvimento
```bash
#!/bin/bash
# start-localstack.sh
docker-compose up -d localstack
sleep 10
./scripts/setup-localstack.sh
```

### 3. Separação de Ambientes
```python
# Em seu código
def get_client(service):
    if config.ENVIRONMENT == 'local':
        return boto3.client(service, endpoint_url=config.AWS_ENDPOINT_URL)
    else:
        return boto3.client(service)
```

## Conclusão

LocalStack é uma ferramenta essencial para desenvolvedores que trabalham com AWS, proporcionando:
- **Economia** significativa durante desenvolvimento
- **Produtividade** aumentada com testes locais
- **Qualidade** melhorada através de testes mais abrangentes
- **Flexibilidade** para desenvolver em qualquer lugar

Integrar LocalStack no fluxo de desenvolvimento permite criar aplicações AWS mais robustas e confiáveis, enquanto reduz custos e dependências externas.






**Feito por**
**Pietra Almeida**

### Contatos
<div> 
    <a href = "mailto:costapietra@gmail.com"><img loading="lazy" src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white" target="_blank"></a>
    <a href="https://www.linkedin.com/in/almeidapietra" target="_blank"><img loading="lazy" src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a>   
</div>
