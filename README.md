# boavista-project

Projeto de criação de Pipeline na GCP, utilizando 2 formas diferentes - Job Data Transfer, Processo com programas Python para execução local

 Instalação do gsutil para realizar a transferencia do dado para o Storage

Utilizado o SO Linux:
curl https://sdk.cloud.google.com | bash

Reinicie o Shell:
exec -l $SHELL

Executando:
gcloud init


##Informações utilizadas para capturar o arquivo via JOB Data Transfer
######## Bucket
Nome do Bucket: test_boavista
repositorio origem: source_data

######## Criação do Job de Transferencia do arquivo "comp_boss" para Bucket
-Nome do Job: 8084082977519456458  
-Nome do arquivo: comp_boss
-Opção de Sincronização: Não ativada
-Periodicidade: Não se repete
-Diretório de origem: /home/leonardo/Entrevista/TesteEngenheiroDados/data/comp_boss
-Bucket destino: test_boavista/source_data
-Descrição do Job:Mover o arquivo comp_boss.csv para o bucket test_boavista

######## Criação do Job de Transferencia do arquivo "comp_boss" para Bucket
-Nome do Job: 8153838024236554941  
-Nome do arquivo: bill_of_materials.csv
-Opção de Sincronização: Não ativada
-Periodicidade: Não se repete
-Diretório de origem: /home/leonardo/Entrevista/TesteEngenheiroDados/data/bill_of_materials
-Bucket destino: test_boavista/source_data
-Descrição do Job: Mover o arquivo bill_of_materials.csv para o bucket test_boavista

######## Criação do Job de Transferencia do arquivo "comp_boss" para Bucket
-Nome do Job: 13324659531313661249 
-Nome do arquivo: price_quote.cs
-Opção de Sincronização: Não ativada
-Periodicidade: Não se repete
-Diretório de origem: /home/leonardo/Entrevista/TesteEngenheiroDados/data/price_quote
-Bucket destino: test_boavista/source_data
-Descrição do Job:Mover o arquivo price_quote.csv para o bucket test_boavista

######## Instalação do Agente Docker para transferir dados Locais para o Cloud Storage (recursos do Cloud Pub/Sub)

--Configuração do Agente
sudo docker run -d --ulimit memlock=64000000 --rm \
--volumes-from gcloud-configg \
-v /:/transfer_root \
gcr.io/cloud-ingest/tsop-agent:latest \
--enable-mount-directory \
--project-id=apt-entropy-289618 \
--creds-file=/home/leonardo/Entrevista/key_access/key.json \
--hostname=$(hostname) \
--agent-id-prefix=agente_transferencia_2


#Configurando credential do google para execução do programa copia_arq_bucket.py
set GOOGLE_APPLICATION_CREDENTIALS="/home/leonardo/Entrevista/key_access/key_2.json"

# Execução da captura via Programas Python

- Se faz necessário importar os programas do Git e observar aos Paths colocados, que estão hardcoded.
- Após alterar os PATH's rodar o main.py, ele orquestrará todo o processo.


###### Job Dataflow
Nome Job: visualization_data_bigquery


python -m apache_beam.examples.wordcount_minimal --input YOUR_INPUT_FILE --output counts

#Extraindo relatório dos arquivos carregados
python -m apache_beam.examples.wordcount --input gs://teste_boavista/source_data/ \
                                         --output gs://apt-entropy-289618/counts \
                                         --runner DataflowRunner \
                                         --project apt-entropy-289618 \
                                         --region us-central1 \
                                         --temp_location gs://apt-entropy-289618/temp
