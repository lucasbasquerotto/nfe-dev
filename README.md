# Sistema para obtenção de Documentos Fiscais (PHP)

Preencher arquivos de configurações com os dados e enviá-los para o S3. Os arquivos de exemplo estão no repositório `nfe-php`.

## 1) Configurações de ambiente

Informar em um arquivo `env.json` no diretório `/config` (no bucket do S3) os dados de configurações (ver `env.sample.json`):

`tpAmb`: 1 = produção; 2 = homologação

`clients`: Array com os ids dos clientes (os ids são as chaves no arquivo `clients.json`, ver `clients.sample.json`)

## 2) Configurações de clientes

Informar em um arquivo `clients.json` no diretório `/config` (no bucket do S3) os dados de configurações (ver `clients.sample.json`):

`razaosocial`: Nome completo do usuário (Ex: `RAZAO SOCIAL DO EMISSOR`)

`cnpj`: Número do CNPJ do usuário (Ex: `99999999999999`)

`ie`: Inscrição Estadual? (Ex: `999999999999`)

`siglaUF`: Unidade Federativa (Ex: `SP`)

`CSC`: Token de segurança fornecido pela SEFAZ para NFCe (Ex: `GPB0JBWLUR6HWFTVEAS6RJ69GPCROFPBBB8G`)

`CSCid`: Id do Token de segurança fornecido pela SEFAZ para NFCe (Ex: `000002`)

## 3. Restaurar os arquivos com as configurações e dados de estado do S3

```bash
cd /docker/main
docker-compose -f docker-compose.run.yml run --rm restore
```

## 4) Executar, salvando os arquivos dos documentos na máquina local

```bash
cd /docker/main
docker-compose -f docker-compose.run.yml run --rm main
```

Obs: Caso deseje executar em mais de uma máquina, alguns clientes em uma e outros em outra máquina, pode incluir novos campos em `env.json`, semelhantes ao campo já existente `clients` com os ids dos clientes, como `clients2`, `clients3`, etc..., e criar instruções para passar os nomes dos campos como variáveis de ambiente no arquivo `docker-compose.run.yml` (no mesmo estilo do serviço `main2`, que usa `clients2`). 

Por exemplo, para utilizar os clients do (suposto) campo `clients2` em `env.json`:

```bash
cd /docker/main
docker-compose -f docker-compose.run.yml run --rm main2
```

## 5. Enviar os arquivos de documentos gerados e estado salvo para o S3

```bash
cd /docker/main
docker-compose -f docker-compose.run.yml run --rm backup
```