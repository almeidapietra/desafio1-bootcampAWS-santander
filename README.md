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

**Feito por**
**Pietra Almeida**

### Contatos
<div> 
    <a href = "mailto:costapietra@gmail.com"><img loading="lazy" src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white" target="_blank"></a>
    <a href="https://www.linkedin.com/in/almeidapietra" target="_blank"><img loading="lazy" src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a>   
</div>
