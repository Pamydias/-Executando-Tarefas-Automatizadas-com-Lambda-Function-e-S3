# -Executando-Tarefas-Automatizadas-com-Lambda-Function-e-S3
# 🚀 Executando Tarefas Automatizadas com Lambda Function e S3  

## 📌 Descrição do Desafio  
Este projeto faz parte do bootcamp da **Digital Innovation One (DIO)** e tem como objetivo consolidar os conhecimentos em **AWS Lambda** e **Amazon S3** através da prática de automação de tarefas.  

Quando um arquivo é enviado ao bucket do S3, uma função Lambda é automaticamente acionada para processar o arquivo e movê-lo para a pasta `processed/` dentro do mesmo bucket.  

---

## 🎯 Objetivos de Aprendizagem  
- Criar e configurar um bucket no **Amazon S3**.  
- Desenvolver uma **função Lambda** para processar eventos.  
- Automatizar tarefas usando **gatilhos (triggers)** do S3.  
- Documentar o processo técnico no **GitHub** como portfólio.  

---

## 🛠️ Tecnologias Utilizadas  
- **Amazon S3** → Armazenamento de objetos  
- **AWS Lambda** → Execução de código sob demanda  
- **AWS IAM** → Permissões e controle de acesso  
- **Amazon CloudWatch** → Monitoramento e logs  

---

## 📂 Estrutura do Repositório  
```bash
📂 lambda-s3-automation
 ┣ 📂 images               # capturas de tela (prints da AWS)
 ┣ 📜 README.md            # documentação principal
 ┗ 📜 lambda_function.py   # código da Lambda em Python
1️⃣ Criando o bucket no S3

Nome do bucket: _BUCKET-da-Pamela

Configurado para receber arquivos de upload.

2️⃣ Criando a função Lambda

Runtime: Python 3.9

Criada role com permissões: s3:GetObject, s3:PutObject, s3:DeleteObject.

Código utilizado:
import json
import boto3
import urllib.parse

s3 = boto3.client('s3')

def lambda_handler(event, context):
    # Recupera informações do evento do S3
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = urllib.parse.unquote_plus(event['Records'][0]['s3']['object']['key'])
    
    print(f"Arquivo recebido: {key} no bucket: {bucket}")
    
    # Definir o novo caminho (exemplo: mover para uma pasta 'processed/')
    new_key = f"processed/{key}"
    
    try:
        # Copiar o arquivo para a nova pasta
        s3.copy_object(
            Bucket=bucket,
            CopySource={'Bucket': bucket, 'Key': key},
            Key=new_key
        )
        
        # Excluir o arquivo original (simulando "mover")
        s3.delete_object(Bucket=bucket, Key=key)
        
        print(f"Arquivo movido para: {new_key}")
        
        return {
            'statusCode': 200,
            'body': json.dumps(f"Arquivo {key} processado e movido para {new_key}")
        }
        
    except Exception as e:
        print(e)
        print(f"Erro ao processar o arquivo {key} do bucket {bucket}.")
        raise e
Entendi a importância de permissões corretas no IAM Role da Lambda.

O CloudWatch é essencial para debug e monitoramento.

Lambda + S3 é uma solução poderosa para automações sem servidor (serverless).

✅ Como Reproduzir

Criar bucket no S3.

Criar função Lambda com o código acima.

Configurar evento no bucket para acionar a Lambda no PUT.

Fazer upload de qualquer arquivo.

Verificar que o arquivo foi movido automaticamente para processed/.

✨ Autor

Projeto desenvolvido por Pamela Sama Dias Assis durante o bootcamp da Digital Innovation One (DIO)
.
