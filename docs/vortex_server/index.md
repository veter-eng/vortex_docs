# Vortex Server

<div style="text-align: left;">
  <img src="../assets/vortex.png" alt="Vortex" width="100" height="100">
</div>

O Vortex Server é o núcleo orquestrador dos microsserviços da Plataforma Vortex. Este componente é responsável por gerenciar e coordenar toda a infraestrutura de coleta, processamento e armazenamento de dados industriais.

## 📋 Visão Geral

O Vortex Server atua como o maestro de uma orquestra, coordenando múltiplos coletores de dados e garantindo que todas as informações sejam processadas e armazenadas adequadamente nos bancos de dados apropriados.

### Principais Funcionalidades

- **Orquestração de Microsserviços**: Gerencia e coordena todos os componentes da plataforma
- **Descoberta Automática de Coletores**: Identifica e configura automaticamente os coletores disponíveis
- **Gerenciamento de Filas**: Processa mensagens dos coletores através do RabbitMQ
- **Persistência de Dados**: Armazena dados em QuestDB (time series) e PostgreSQL (configurações)
- **Configuração Centralizada**: Gerencia configurações de toda a plataforma

### Arquitetura

O servidor utiliza uma arquitetura baseada em filas de mensagens, onde:

1. **Inicialização**: Verifica e inicializa as conexões com os bancos de dados
2. **Descoberta**: Busca coletores configurados no banco de dados
3. **Orquestração**: Inicia serviços de fila para cada coletor descoberto
4. **Processamento**: Processa continuamente as mensagens recebidas dos coletores

## 🛠️ Tecnologias Utilizadas

- **.NET 9**: Framework principal da aplicação
- **PostgreSQL**: Banco de dados para configurações
- **InfluxDB**: Banco de dados time series para dados coletados
- **RabbitMQ**: Broker de mensagens
- **Erlang**: Runtime necessário para o RabbitMQ
- **Docker**: Containerização da aplicação

## 🚀 Instalação e Configuração

### Pré-requisitos

Antes de instalar o Vortex Server, é necessário configurar as seguintes dependências:

#### QuestDB (Banco Time Series)
```bash
# Baixar QuestDB
wget https://github.com/questdb/questdb/releases/download/8.2.2/questdb-8.2.2-rt-linux-x86-64.tar.gz

# Descompactar
tar -xvzf questdb-8.2.2-rt-linux-x86-64.tar.gz

# Mover para diretório de aplicações
sudo mkdir -p /opt/questdb
sudo mv questdb-8.2.2-rt-linux-x86-64/* /opt/questdb

# Configurar PATH
echo 'export PATH=$PATH:/opt/questdb/bin' >> ~/.bashrc
source ~/.bashrc

# Iniciar QuestDB
cd /opt/questdb/bin
./questdb.sh start
```

#### Erlang (Pré-requisito do RabbitMQ)
```bash
# Atualizar sistema
sudo apt-get update

# Instalar dependências
sudo apt install -y curl gnupg apt-transport-https

# Adicionar chave do repositório
curl -fsSL https://packages.erlang-solutions.com/ubuntu/erlang_solutions.asc | sudo apt-key add -

# Adicionar repositório
echo "deb https://packages.erlang-solutions.com/ubuntu $(lsb_release -cs) contrib" | sudo tee /etc/apt/sources.list.d/erlang.list

# Instalar Erlang
sudo apt-get update
sudo apt install -y erlang
```

#### RabbitMQ (Broker de Mensagens)
```bash
# Baixar RabbitMQ
wget https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.11.16/rabbitmq-server-generic-unix-3.11.16.tar.xz

# Extrair pacote
tar -xvf rabbitmq-server-generic-unix-3.11.16.tar.xz

# Mover para pasta de aplicações
sudo mkdir -p /opt/rabbitmq
sudo mv rabbitmq_server-3.11.16/* /opt/rabbitmq

# Configurar PATH
echo 'export PATH=$PATH:/opt/rabbitmq/sbin' >> ~/.bashrc
source ~/.bashrc

# Criar diretórios necessários
sudo mkdir -p /opt/rabbitmq/var/lib/rabbitmq/mnesia
sudo mkdir -p /opt/rabbitmq/var/log/rabbitmq
sudo chown -R ubuntu:ubuntu /opt/rabbitmq/var

# Iniciar RabbitMQ
cd /opt/rabbitmq/sbin
./rabbitmq-server
```

#### PostgreSQL (Banco de Configurações)
```bash
# Atualizar sistema
sudo apt update && sudo apt upgrade -y

# Instalar dependências
sudo apt install -y wget gnupg2 lsb-release

# Adicionar repositório PostgreSQL 17
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo gpg --dearmor -o /usr/share/keyrings/postgresql-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/postgresql-archive-keyring.gpg] http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" | sudo tee /etc/apt/sources.list.d/pgdg.list

# Instalar PostgreSQL 17
sudo apt update
sudo apt install -y postgresql-17

# Iniciar serviços
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

#### .NET 9 (Runtime da Aplicação)
```bash
# Instalar pré-requisitos
sudo apt update && sudo apt upgrade -y
sudo apt install -y wget curl

# Script de instalação do .NET 9
wget https://dot.net/v1/dotnet-install.sh -O dotnet-install.sh
chmod +x dotnet-install.sh
./dotnet-install.sh --channel 9.0

# Configurar variáveis de ambiente
echo 'export DOTNET_ROOT=$HOME/.dotnet' >> ~/.bashrc
echo 'export PATH=$PATH:$HOME/.dotnet' >> ~/.bashrc
source ~/.bashrc
```

### Instalação via GitHub CLI

```bash
# Instalar GitHub CLI
sudo apt update && sudo apt upgrade -y
sudo apt install curl -y

# Adicionar repositório oficial
type -p curl >/dev/null || sudo apt install curl -y
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | \
sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg

sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) \
signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] \
https://cli.github.com/packages stable main" | \
sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null

# Instalar GitHub CLI
sudo apt update
sudo apt install gh -y

# Configurar e clonar projeto
gh auth login
sudo mkdir /opt/github
sudo chown -R ubuntu:ubuntu /opt/github
cd /opt/github
gh repo clone armatore/vortex_server
```

### Configuração da Aplicação

1. **Configurar variáveis de ambiente** no arquivo `.env`
2. **Configurar string de conexão** do PostgreSQL
3. **Configurar endereços** do RabbitMQ e QuestDB
4. **Executar migrations** do banco de dados

### Execução

```bash
# Compilar projeto
dotnet build

# Executar aplicação
dotnet run
```

## 📊 Monitoramento

O Vortex Server fornece logs detalhados sobre:

- Status da inicialização dos bancos de dados
- Descoberta e configuração de coletores
- Processamento de mensagens
- Erros e exceções

## 🔧 Manutenção

### Logs
Os logs da aplicação são exibidos no console e incluem:
- ✅ Eventos de sucesso
- ⚠️ Avisos e alertas
- ❌ Erros críticos
- 🔍 Informações de debug

### Backup
É recomendado fazer backup regular dos bancos de dados PostgreSQL e QuestDB.

### Atualizações
Para atualizar o servidor:
1. Pare a aplicação
2. Faça backup dos dados
3. Atualize o código fonte
4. Execute as migrations necessárias
5. Reinicie a aplicação