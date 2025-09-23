# Vortex API

<div style="text-align: left;">
  <img src="../assets/vortex.png" alt="Vortex" width="100" height="100">
</div>

O Vortex API é a interface de programação de aplicações que permite a configuração e gerenciamento de todos os componentes da Plataforma Vortex. Esta API RESTful oferece endpoints para configurar coletores, equipamentos, tags e gateways de comunicação.

## 📋 Visão Geral

A Vortex API serve como o ponto central de configuração da plataforma, permitindo que administradores e sistemas externos configurem e monitorem todos os aspectos da coleta de dados industriais. A API segue os padrões REST e oferece documentação interativa via Swagger.

### Principais Funcionalidades

- **Gerenciamento de Coletores**: CRUD completo para configuração de coletores
- **Configuração de Equipamentos**: Cadastro e gerenciamento de equipamentos industriais
- **Gestão de Tags**: Configuração de tags OPC para coleta de dados
- **Controle de Gateways**: Configuração de gateways de comunicação
- **Documentação Interativa**: Interface Swagger para teste e documentação
- **Validação de Dados**: Validação automática de dados de entrada
- **Persistência**: Armazenamento em PostgreSQL

### Arquitetura

A API utiliza uma arquitetura em camadas baseada em:

1. **Controllers**: Camada de apresentação que expõe os endpoints REST
2. **Services**: Camada de lógica de negócio que processa as requisições
3. **Data Access**: Camada de acesso a dados usando Entity Framework Core
4. **DTOs**: Objetos de transferência de dados para comunicação
5. **Models**: Modelos de domínio que representam as entidades

## 🛠️ Tecnologias Utilizadas

- **.NET 9**: Framework principal da aplicação
- **ASP.NET Core**: Framework web para criação da API REST
- **Entity Framework Core**: ORM para acesso ao banco de dados
- **PostgreSQL**: Banco de dados relacional
- **Swagger/OpenAPI**: Documentação interativa da API
- **Npgsql**: Driver PostgreSQL para .NET
- **Docker**: Containerização da aplicação

## 🚀 Execução

A Vortex API é executada como parte do ecossistema completo da Plataforma Vortex através do docker-compose do Vortex Server.

### Execução Integrada

A API é automaticamente iniciada junto com todos os outros componentes da plataforma:

```bash
# Executar toda a plataforma (incluindo a API)
docker-compose up -d

# Verificar status de todos os serviços
docker-compose ps

# Visualizar logs da API especificamente
docker-compose logs -f vortex-api
```

### Comandos de Gerenciamento

```bash
# Parar todos os serviços
docker-compose down

# Reiniciar apenas a API
docker-compose restart vortex-api

# Verificar logs em tempo real
docker-compose logs -f
```

### Acesso

- **API Base URL**: `http://localhost:5000`
- **Swagger UI**: `http://localhost:5000/swagger`
- **Swagger JSON**: `http://localhost:5000/swagger/v1/swagger.json`

### Configuração

A API é configurada automaticamente através do docker-compose do Vortex Server. As configurações de banco de dados são gerenciadas pelos serviços PostgreSQL containerizados, eliminando a necessidade de configuração manual de strings de conexão.

## 📚 Endpoints da API

### 🔧 Coletores (`/api/ConfigColetor`)

Gerenciamento de coletores de dados.

#### Endpoints Disponíveis

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `GET` | `/api/ConfigColetor` | Lista todos os coletores |
| `GET` | `/api/ConfigColetor/{id}` | Busca coletor por ID |
| `POST` | `/api/ConfigColetor` | Cria novo coletor |
| `PUT` | `/api/ConfigColetor/{id}` | Atualiza coletor existente |
| `DELETE` | `/api/ConfigColetor/{id}` | Remove coletor |

#### Exemplo de Payload

```json
{
  "id": 1,
  "name": "Coletor_Linha_01",
  "description": "Coletor da linha de produção 01",
  "serverIp": "192.168.1.100",
  "isActive": true,
  "createdAt": "2024-01-15T10:30:00Z"
}
```

### ⚙️ Equipamentos (`/api/ConfigEquipments`)

Configuração de equipamentos industriais.

#### Endpoints Disponíveis

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `GET` | `/api/ConfigEquipments` | Lista todos os equipamentos |
| `GET` | `/api/ConfigEquipments/{id}` | Busca equipamento por ID |
| `POST` | `/api/ConfigEquipments` | Cria novo equipamento |
| `PUT` | `/api/ConfigEquipments/{id}` | Atualiza equipamento |
| `DELETE` | `/api/ConfigEquipments/{id}` | Remove equipamento |

#### Exemplo de Payload

```json
{
  "id": 1,
  "name": "PLC_Principal",
  "type": "Siemens S7-1200",
  "location": "Linha de Produção A",
  "ipAddress": "192.168.1.50",
  "description": "PLC principal da linha A"
}
```

### 🏷️ Tags (`/api/ConfigTags`)

Gerenciamento de tags OPC para coleta de dados.

#### Endpoints Disponíveis

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `GET` | `/api/ConfigTags` | Lista todas as tags |
| `GET` | `/api/ConfigTags/{id}` | Busca tag por ID |
| `POST` | `/api/ConfigTags` | Cria nova tag |
| `PUT` | `/api/ConfigTags/{id}` | Atualiza tag existente |
| `DELETE` | `/api/ConfigTags/{id}` | Remove tag |

#### Exemplo de Payload

```json
{
  "id": 1,
  "name": "Temperature_Sensor_01",
  "opcTag": "NS=2;i=1001",
  "description": "Sensor de temperatura da caldeira",
  "dataType": "Float",
  "unit": "°C",
  "equipmentId": 1,
  "isActive": true,
  "scanRate": 1000
}
```

### 🌐 Gateways (`/api/ConfigGateways`)

Configuração de gateways de comunicação.

#### Endpoints Disponíveis

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `GET` | `/api/ConfigGateways` | Lista todos os gateways |
| `GET` | `/api/ConfigGateways/{id}` | Busca gateway por ID |
| `POST` | `/api/ConfigGateways` | Cria novo gateway |
| `PUT` | `/api/ConfigGateways/{id}` | Atualiza gateway |
| `DELETE` | `/api/ConfigGateways/{id}` | Remove gateway |

#### Exemplo de Payload

```json
{
  "id": 1,
  "name": "Gateway_Principal",
  "ipAddress": "192.168.1.1",
  "port": 502,
  "protocol": "Modbus TCP",
  "description": "Gateway principal da planta"
}
```

## 🔒 Autenticação e Autorização

Atualmente, a API opera sem autenticação para ambiente de desenvolvimento. Para produção, recomenda-se implementar:

- **JWT Bearer Tokens**
- **OAuth 2.0**
- **API Keys**
- **Rate Limiting**

## 📊 Códigos de Resposta HTTP

| Código | Descrição |
|--------|-----------|
| `200 OK` | Requisição bem-sucedida |
| `201 Created` | Recurso criado com sucesso |
| `204 No Content` | Atualização/exclusão bem-sucedida |
| `400 Bad Request` | Dados inválidos na requisição |
| `404 Not Found` | Recurso não encontrado |
| `500 Internal Server Error` | Erro interno do servidor |

## 🧪 Testando a API

### Via Swagger UI

1. Acesse `http://localhost:5000/swagger`
2. Expanda o endpoint desejado
3. Clique em "Try it out"
4. Preencha os parâmetros necessários
5. Execute a requisição

### Via CURL

```bash
# Listar todos os coletores
curl -X GET "http://localhost:5000/api/ConfigColetor" \
     -H "accept: application/json"

# Criar novo coletor
curl -X POST "http://localhost:5000/api/ConfigColetor" \
     -H "accept: application/json" \
     -H "Content-Type: application/json" \
     -d '{
       "name": "Novo_Coletor",
       "description": "Descrição do coletor",
       "serverIp": "192.168.1.200",
       "isActive": true
     }'

# Buscar coletor por ID
curl -X GET "http://localhost:5000/api/ConfigColetor/1" \
     -H "accept: application/json"
```

### Via Postman

1. Importe a collection Swagger: `http://localhost:5000/swagger/v1/swagger.json`
2. Configure o ambiente com a base URL
3. Execute as requisições

## 🏗️ Estrutura do Projeto

```
vortex_server_api/
├── API/
│   ├── Controllers/          # Controllers REST
│   │   ├── ConfigColetorController.cs
│   │   ├── ConfigEquipmentsController.cs
│   │   ├── ConfigGatewaysController.cs
│   │   └── ConfigTagsController.cs
│   ├── Data/                # Contexto do Entity Framework
│   ├── DTOs/                # Data Transfer Objects
│   ├── Models/              # Modelos de domínio
│   └── Services/            # Serviços de negócio
│       ├── Interfaces/      # Interfaces dos serviços
│       ├── ConfigColetorService.cs
│       ├── ConfigEquipmentsService.cs
│       ├── ConfigGatewaysService.cs
│       └── ConfigTagsService.cs
├── Program.cs               # Ponto de entrada
└── appsettings.json         # Configurações
```

## 🔧 Configurações Avançadas

### Ambiente Containerizado

A API é configurada automaticamente para funcionar no ambiente Docker do Vortex Server:

- **Banco de Dados**: Conecta automaticamente ao PostgreSQL containerizado
- **Rede**: Comunica-se com outros serviços através da rede Docker interna
- **Configurações**: Todas as configurações são gerenciadas via variáveis de ambiente do docker-compose

### Arquitetura Interna

A API utiliza uma arquitetura em camadas com:

- **Entity Framework Core**: Para acesso ao PostgreSQL containerizado
- **Injeção de Dependência**: Serviços registrados automaticamente
- **Swagger**: Documentação interativa disponível em `/swagger`
- **Validação**: DTOs com validação automática de dados

## 🐛 Troubleshooting

### Problemas Comuns

#### Erro de Conexão com Banco
```
Npgsql.NpgsqlException: Connection refused
```
**Solução**: Verificar se PostgreSQL está rodando e a string de conexão está correta.

#### Erro de Migration
```
Unable to create migration
```
**Solução**: Verificar se o Entity Framework está instalado e configurado.

#### Erro 404 no Swagger
**Solução**: Verificar se a aplicação está no modo Development.

### Logs

A API fornece logs detalhados:
```
🚀 API Iniciada!
Acesse a API: http://localhost:5000
Swagger UI: http://localhost:5000/swagger
```

## 🔄 Integração com Outros Componentes

### Vortex Collector
O Collector busca configurações da API para saber quais tags coletar:
```csharp
var configurationService = new ConfigurationService(httpClient, "http://localhost:5000");
var tags = await configurationService.GetConfiguredTagsAsync();
var collectors = await configurationService.GetConfiguredCollectorsAsync();
```

### Vortex Server
O Server utiliza a API para descobrir e gerenciar coletores automaticamente:
```csharp
// O Server consulta a API para descobrir coletores configurados
var existingCollectors = await apiService.GetConfiguredCollectorsAsync();
// E inicia os serviços de processamento para cada coletor encontrado
```

### Fluxo de Configuração
1. **Administrador** configura coletores, equipamentos e tags via API
2. **Vortex Server** descobre automaticamente as configurações via API
3. **Vortex Collector** busca suas configurações específicas via API
4. **Sistema** inicia a coleta de dados baseada nas configurações

## 📈 Performance e Escalabilidade

### Otimizações
- **Connection Pooling**: Pool de conexões do PostgreSQL
- **Async/Await**: Operações assíncronas para melhor throughput
- **Caching**: Cache de configurações frequentemente acessadas
- **Paginação**: Para listagens grandes

### Monitoramento
- **Health Checks**: Endpoints de saúde da aplicação
- **Logging**: Logs estruturados para monitoramento
- **Metrics**: Métricas de performance e uso