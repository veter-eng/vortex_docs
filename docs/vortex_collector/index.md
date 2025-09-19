# Vortex Collector

<div style="text-align: left;">
  <img src="../assets/vortex.png" alt="Vortex" width="100" height="100">
</div>

O Vortex Collector é o componente responsável pela coleta de dados de equipamentos industriais através de protocolos de comunicação como OPC DA. Este microsserviço atua como uma ponte entre os sistemas industriais e a Plataforma Vortex.

## 📋 Visão Geral

O Vortex Collector é projetado para operar em ambientes industriais, coletando dados de PLCs, sensores e outros equipamentos de automação. Ele utiliza uma arquitetura robusta e escalável para garantir a coleta confiável de dados em tempo real.

### Principais Funcionalidades

- **Coleta de Dados OPC DA**: Conecta-se a servidores OPC DA para coletar dados de equipamentos
- **Configuração Dinâmica**: Busca configurações de tags e equipamentos via API
- **Envio de Dados**: Transmite dados coletados via RabbitMQ para processamento
- **Multi-Coletor**: Suporta múltiplos coletores executando simultaneamente
- **Recuperação Automática**: Reconecta automaticamente em caso de falhas
- **Monitoramento**: Fornece logs detalhados sobre o status da coleta

### Arquitetura

O coletor segue uma arquitetura baseada em serviços distribuídos:

1. **Inicialização**: Conecta-se ao serviço de configuração
2. **Descoberta**: Busca informações dos coletores no banco de dados
3. **Configuração**: Obtém tags configuradas para cada coletor
4. **Coleta**: Executa leitura contínua dos dados OPC DA
5. **Transmissão**: Envia dados via RabbitMQ para o Vortex Server

## 🛠️ Tecnologias Utilizadas

- **.NET 9**: Framework principal da aplicação
- **OPC DA**: Protocolo de comunicação industrial
- **RabbitMQ**: Broker de mensagens para transmissão de dados
- **PostgreSQL**: Banco de dados para configurações de coletores
- **HttpClient**: Comunicação com API de configuração
- **Docker**: Containerização da aplicação

## 🚀 Instalação e Configuração

### Pré-requisitos

#### Dependências do Sistema
```bash
# .NET 9 Runtime
wget https://dot.net/v1/dotnet-install.sh -O dotnet-install.sh
chmod +x dotnet-install.sh
./dotnet-install.sh --channel 9.0

# Configurar variáveis de ambiente
echo 'export DOTNET_ROOT=$HOME/.dotnet' >> ~/.bashrc
echo 'export PATH=$PATH:$HOME/.dotnet' >> ~/.bashrc
source ~/.bashrc
```

#### Servidor OPC DA
O coletor requer um servidor OPC DA funcionando na rede. Configurações típicas:
- **Host**: localhost ou IP do servidor OPC
- **Porta**: Porta padrão do OPC DA (geralmente 135)
- **CLSID**: Identificador do servidor OPC

### Configuração

#### Variáveis de Ambiente
```csharp
// Configurações padrão no Program.cs
private const string OpcdaTag = "localhost";
private const string RabbitMqHost = "localhost";
private const string ConfigurationServiceUrl = "http://localhost:5000";
private const string PostgresConnectionString = "Host=localhost;Port=5432;Username=postgres;Password=admin;Database=vortex_server_config";
```

#### Configuração de Coletores
Os coletores devem ser configurados no banco de dados PostgreSQL através da API do Vortex. Cada coletor possui:

- **Nome**: Identificador único do coletor
- **IP do Servidor**: Endereço do servidor de configuração (opcional)
- **Tags**: Lista de tags OPC para coleta
- **Gateway**: Informações do gateway de comunicação

### Instalação

```bash
# Clonar repositório
git clone [repository-url]
cd vortex_collector

# Restaurar dependências
dotnet restore

# Compilar projeto
dotnet build

# Executar aplicação
dotnet run
```

## 🔧 Configuração Avançada

### Estrutura do Projeto

```
vortex_collector/
├── Application/          # Lógica de aplicação
├── Domain/              # Modelos de domínio
├── Infrastructure/      # Infraestrutura (OPC, RabbitMQ, DB)
│   ├── OPC/DA/         # Implementação OPC DA
│   ├── RabbitMQ/       # Serviços de mensageria
│   └── Services/       # Serviços de infraestrutura
├── Presentation/       # Camada de apresentação
└── Program.cs          # Ponto de entrada da aplicação
```

### Serviços Principais

#### ConfigurationService
Responsável por buscar configurações da API:
```csharp
// Busca tags configuradas
var tags = await configurationService.GetConfiguredTagsAsync();

// Atualiza URL base do serviço
configurationService.UpdateBaseUrl(serverUrl);
```

#### OpcDaTagReader
Realiza a leitura dos dados OPC DA:
```csharp
var tagReader = new OpcDaTagReader(OpcdaTag, configurationService);
```

#### RabbitMqService
Gerencia o envio de dados:
```csharp
var messageQueue = new RabbitMqService(RabbitMqHost, collectorInfo.Name);
```

#### CollectorService
Orquestra a coleta de dados:
```csharp
var service = new CollectorService(tagReader, messageQueue);
await service.RunAsync();
```

## 📊 Monitoramento e Logs

### Logs da Aplicação
O coletor fornece logs detalhados sobre:

- **🚀 Inicialização**: Status do start da aplicação
- **📦 Serviços**: Criação e configuração dos serviços
- **🔍 Descoberta**: Busca de coletores no banco de dados
- **✅ Conexões**: Status das conexões com APIs e bancos
- **⚠️ Avisos**: Alertas sobre problemas de configuração
- **❌ Erros**: Falhas de comunicação ou processamento

### Exemplo de Log de Inicialização
```
🚀 Program Starting...
🚀 Starting vortex_collector...
📦 Creating HttpClient...
📦 Creating ConfigurationService...
Testando Conexão com ConfigurationService...
✅ Conectado com sucesso!
📦 Creating CollectorDbService...
🔍 Querying Collectors From Database...
📋 Found 3 Collectors
Iniciando Coletor: Coletor_01
Usando servidor: http://192.168.1.100:5000
⏳ All collectors started. Press Ctrl+C to stop...
```

## 🔄 Fluxo de Operação

### 1. Inicialização
- Verifica conectividade com o serviço de configuração
- Aguarda até que a API esteja disponível
- Conecta-se ao banco de dados PostgreSQL

### 2. Descoberta de Coletores
- Busca lista de coletores configurados no banco
- Valida configurações de cada coletor
- Prepara serviços para cada coletor encontrado

### 3. Configuração Dinâmica
- Para cada coletor, busca as tags configuradas
- Configura URL do servidor (se especificado)
- Inicializa serviços OPC DA e RabbitMQ

### 4. Coleta Contínua
- Executa leitura das tags OPC DA em intervalos regulares
- Processa e formata os dados coletados
- Envia dados para o RabbitMQ

### 5. Recuperação de Falhas
- Detecta falhas de comunicação
- Implementa retry automático
- Reconecta-se aos serviços quando necessário

## 🛡️ Tratamento de Erros

### Tipos de Erro

#### Falhas de Conexão
- **Serviço de Configuração**: Retry com delay de 1 minuto
- **Banco de Dados**: Falha crítica, aplicação para
- **OPC DA**: Reconexão automática
- **RabbitMQ**: Retry com backoff exponencial

#### Configurações Ausentes
- **Nenhum Coletor**: Aplicação para com aviso
- **Tags Inválidas**: Log de erro, continua com outras tags
- **Servidor Indisponível**: Fallback para servidor padrão

### Mensagens de Erro Comuns

```
❌ Falha na conexão: [erro específico]
🔁 Tentando novamente em 1 minuto...
⚠️ No Collectors Found In Database!!! Please Add Collectors Via API First!!!
```

## 🔧 Manutenção

### Atualização de Configurações
As configurações são atualizadas dinamicamente via API. Não é necessário reiniciar o coletor para aplicar mudanças nas tags.

### Backup e Recuperação
- **Configurações**: Armazenadas no PostgreSQL (gerenciado pelo Vortex Server)
- **Logs**: Logs de aplicação são mantidos em memória/console
- **Estado**: O coletor é stateless, recuperação é automática

### Performance
- **CPU**: Baixo uso, otimizado para coleta contínua
- **Memória**: Uso eficiente, garbage collection automático
- **Rede**: Tráfego otimizado via RabbitMQ

### Escalabilidade
- **Horizontal**: Múltiplos coletores podem executar simultaneamente
- **Vertical**: Suporta grande quantidade de tags por coletor
- **Distribuída**: Coletores podem estar em diferentes redes/locais