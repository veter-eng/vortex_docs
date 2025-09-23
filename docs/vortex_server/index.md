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
- **Persistência de Dados**: Armazena dados em InfluxDB (time series) e PostgreSQL (configurações)
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
- **Grafana**: Observabilidade e dashboards para visualização de métricas
- **Docker**: Containerização da aplicação

## 🚀 Instalação e Configuração

### Pré-requisitos

A aplicação Vortex utiliza uma arquitetura híbrida onde alguns componentes são executados via Docker e outros diretamente no Windows:

#### Docker Desktop + WSL 2 (Para componentes containerizados)

1. **Instalar WSL 2:**
   ```powershell
   # Abrir PowerShell como administrador
   wsl --install
   wsl --set-default-version 2
   wsl --install -d Ubuntu
   ```

2. **Instalar Docker Desktop:**
   - Baixe o instalador do [Docker Desktop](https://www.docker.com/products/docker-desktop/)
   - Durante a instalação, marque "Use WSL 2 instead of Hyper-V (recommended)"
   - Reinicie o computador após a instalação

### Erlang (Pré-requisito do RabbitMQ 4.0.9)

**Download direto:**
- Baixe o Erlang/OTP 27.x para Windows: [https://www.erlang.org/downloads](https://www.erlang.org/downloads)
- Escolha a versão Windows 64-bit (.exe) mais recente da série 27.x
- Execute o instalador como administrador

**Ou via Chocolatey (PowerShell como administrador):**
```powershell
# Instalar Chocolatey (se não tiver)
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# Instalar Erlang (versão mais recente compatível com RabbitMQ 4.0.9)
choco install erlang
```

### RabbitMQ (Broker de Mensagens)

**Download direto:**
- Baixe o RabbitMQ 4.0.9 para Windows: [https://github.com/rabbitmq/rabbitmq-server/releases/tag/v4.0.9](https://github.com/rabbitmq/rabbitmq-server/releases/tag/v4.0.9)
- Escolha o arquivo `rabbitmq-server-4.0.9.exe`
- Execute o instalador como administrador

**Ou via Chocolatey (PowerShell como administrador):**
```powershell
# Instalar RabbitMQ
choco install rabbitmq --version=4.0.9

# Iniciar serviço RabbitMQ
Start-Service RabbitMQ
```


### Obtenção do Projeto

**Download direto:**
- Baixe o GitHub CLI para Windows: [https://cli.github.com/](https://cli.github.com/)
- Execute o instalador como administrador

**Ou via Chocolatey (PowerShell como administrador):**
```powershell
# Instalar GitHub CLI
choco install gh

# Configurar e clonar projeto
gh auth login

gh repo clone <repositorio>
```

### Configuração da Aplicação

#### 1. Configurar RabbitMQ no Windows

**Adicionar RabbitMQ ao PATH:**

1. Localize a pasta `sbin` do RabbitMQ:
   ```
   C:\Program Files\RabbitMQ Server\rabbitmq_server-4.0.9\sbin
   ```

2. Adicione ao PATH do sistema:
   - Abra "Editar variáveis de ambiente do sistema"
   - Clique em "Variáveis de Ambiente"
   - Em "Variáveis do sistema", selecione `Path` e clique em "Editar"
   - Clique em "Novo" e cole o caminho da pasta `sbin`
   - Confirme e feche todas as janelas

3. Teste no PowerShell:
   ```powershell
   rabbitmqctl status
   ```

**Ativar painel web de gerenciamento:**
```powershell
# Ativar plugin
rabbitmq-plugins enable rabbitmq_management

# Reiniciar serviço
Restart-Service rabbitmq

# Acessar: http://localhost:15672
# Usuário: guest | Senha: guest
```

**Criar usuário para Docker:**
```powershell
# Criar usuário para containers
rabbitmqctl add_user vortex vortex123
rabbitmqctl set_user_tags vortex administrator
rabbitmqctl set_permissions -p / vortex ".*" ".*" ".*"
```

#### 2. Configurar Variáveis de Ambiente

Crie arquivo `.env` com as configurações de variáveis de ambiente do projeto:

```env
INFLUXDB_HOST=influxdb
INFLUXDB_PORT=8086
INFLUXDB_DATABASE=
INFLUXDB_USERNAME=
INFLUXDB_PASSWORD=

RABBITMQ_HOST=host.docker.internal
RABBITMQ_PORT=5672
RABBITMQ_USERNAME=
RABBITMQ_PASSWORD=

POSTGRES_HOST=postgres
POSTGRES_PORT=5432
POSTGRES_DATABASE=
POSTGRES_USERNAME=
POSTGRES_PASSWORD=
```

### Execução da Aplicação

#### 1. Iniciar RabbitMQ (Windows)
```powershell
# Iniciar serviço RabbitMQ
Start-Service RabbitMQ

# Verificar status
Get-Service RabbitMQ
```

#### 2. Executar Componentes Containerizados
```bash
# Construir e executar todos os serviços containerizados
docker-compose up -d

# Verificar status dos containers
docker-compose ps

# Visualizar logs
docker-compose logs -f vortex-server
```

#### 3. Estrutura do Docker Compose

O projeto utiliza um arquivo `docker-compose.yml` que define todos os serviços necessários:

- **vortex-server**: Aplicação principal (.NET 9)
- **vortex-api**: API REST para configuração
- **vortex-interface**: Interface web do usuário
- **postgres**: Banco de dados PostgreSQL para configurações
- **influxDB**: Banco de dados time series para dados coletados
- **grafana**: Dashboard para visualização de métricas

#### 4. Portas e Acesso

Após a execução, os seguintes serviços estarão disponíveis:

| Serviço | Porta | URL de Acesso | Descrição |
|---------|-------|---------------|-----------|
| **Vortex API** | 5000 | http://localhost:5000 | API REST |
| **Swagger UI** | 5000 | http://localhost:5000/swagger | Documentação da API |
| **Vortex Interface** | 3000 | http://localhost:3000 | Interface web |
| **Grafana** | 3001 | http://localhost:3001 | Dashboards |
| **RabbitMQ Management** | 15672 | http://localhost:15672 | Gerenciamento do RabbitMQ |
| **InfluxDB Web Console** | 8086 | http://localhost:8086 | Console do InfluxDB |
| **PostgreSQL** | 5432 | localhost:5432 | Banco de dados |

#### 5. Verificação da Instalação

Após executar o `docker-compose up -d`, verifique se todos os serviços estão funcionando:

```bash
# Verificar status de todos os containers
docker-compose ps

# Verificar logs de inicialização
docker-compose logs --tail=50

# Testar conectividade da API
curl http://localhost:5000/swagger/v1/swagger.json

# Verificar RabbitMQ
curl http://localhost:15672
```

**Indicadores de Sucesso:**
- ✅ Todos os containers estão com status "Up"
- ✅ API responde em http://localhost:5000
- ✅ Swagger UI acessível em http://localhost:5000/swagger
- ✅ RabbitMQ Management acessível em http://localhost:15672
- ✅ Grafana acessível em http://localhost:3001

### Arquitetura de Execução

#### 🔧 Componentes Executados FORA do Container (Windows)
- **RabbitMQ**: Broker de mensagens (instalado diretamente no Windows)
- **Erlang**: Runtime necessário para o RabbitMQ

#### 🐳 Componentes Executados DENTRO do Container (Docker)
- **Vortex Server**: Aplicação principal (.NET 9)
- **PostgreSQL**: Banco de dados para configurações
- **InfluxDB**: Banco de dados time series
- **Grafana**: Observabilidade e dashboards
- **API**: Interface de comunicação
- **Interface**: Interface web do usuário

## 📊 Monitoramento

O Vortex Server fornece logs detalhados sobre:

- Status da inicialização dos bancos de dados
- Descoberta e configuração de coletores
- Processamento de mensagens
- Erros e exceções

### Acessos para Monitoramento

- **Logs em Tempo Real**: `docker-compose logs -f vortex-server`
- **Grafana Dashboard**: http://localhost:3001 (admin/admin)
- **RabbitMQ Management**: http://localhost:15672 (guest/guest)
- **InfluxDB Console**: http://localhost:8086
- **API Swagger**: http://localhost:5000/swagger

## 🔧 Manutenção

### Logs
Os logs da aplicação são exibidos no console e incluem:
- ✅ Eventos de sucesso
- ⚠️ Avisos e alertas
- ❌ Erros críticos
- 🔍 Informações de debug

### Backup
É recomendado fazer backup regular dos bancos de dados PostgreSQL e Influx.

### Atualizações
Para atualizar o servidor:
```bash
# 1. Parar a aplicação
docker-compose down

# 2. Fazer backup dos dados
# (usar comandos de backup acima)

# 3. Atualizar código fonte
git pull origin main

# 4. Reconstruir imagens
docker-compose build --no-cache

# 5. Reiniciar aplicação
docker-compose up -d
```

### Limpeza de Recursos
```bash
# Limpar containers parados
docker-compose down --remove-orphans

# Limpar volumes não utilizados
docker volume prune

# Limpar imagens antigas
docker image prune -a

# Limpar sistema completo
docker system prune -a --volumes
```

### Comandos de Manutenção Diária
```bash
# Verificar status geral
docker-compose ps

# Verificar logs recentes
docker-compose logs --tail=100

# Verificar uso de recursos
docker stats --no-stream

# Verificar espaço em disco
docker system df
```