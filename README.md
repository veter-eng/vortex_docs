# 📚 Documentação da Plataforma Vortex

<div style="text-align: center;">
  <img src="docs/assets/vortex.png" alt="Vortex Platform" width="200" height="200">
</div>

## 🚀 Sobre a Plataforma Vortex

A Plataforma Vortex é uma solução completa para coleta, processamento e armazenamento de dados industriais em tempo real. Desenvolvida para ambientes de automação industrial, a plataforma oferece uma arquitetura de microsserviços robusta e escalável.

### 🎯 Objetivo

Esta documentação fornece informações completas sobre todos os componentes da Plataforma Vortex, incluindo instalação, configuração, operação e manutenção de cada microsserviço.

## 🏗️ Arquitetura da Plataforma

A Plataforma Vortex é composta por três componentes principais:

### 🖥️ **Vortex Server**
O núcleo orquestrador da plataforma, responsável por:
- Coordenação de todos os microsserviços
- Descoberta automática de coletores
- Processamento de filas de mensagens
- Persistência de dados em bancos time series e relacionais
- Gerenciamento de configurações centralizadas

### 📡 **Vortex Collector**
O componente de coleta de dados industriais, que oferece:
- Comunicação com equipamentos via protocolos OPC DA
- Coleta de dados em tempo real de PLCs e sensores
- Transmissão de dados via RabbitMQ
- Suporte a múltiplos coletores simultâneos
- Recuperação automática de falhas

### 🌐 **Vortex API**
A interface de programação para configuração e gerenciamento:
- API RESTful para configuração de todos os componentes
- Gerenciamento de coletores, equipamentos e tags
- Interface Swagger para documentação interativa
- Validação automática de dados
- Persistência em PostgreSQL

## 🛠️ Tecnologias Utilizadas

| Componente | Tecnologias Principais |
|------------|----------------------|
| **Server** | .NET 9, PostgreSQL, QuestDB, RabbitMQ, Erlang |
| **Collector** | .NET 9, OPC DA, RabbitMQ, HttpClient |
| **API** | ASP.NET Core, Entity Framework, PostgreSQL, Swagger |

## 📖 Como Usar Esta Documentação

### 🎯 Para Administradores
- Consulte os guias de instalação de cada componente
- Siga os procedimentos de configuração e manutenção
- Utilize os exemplos de troubleshooting

### 👨‍💻 Para Desenvolvedores
- Explore a documentação da API para integração
- Consulte os diagramas de arquitetura
- Utilize os exemplos de código e configuração

### 🔧 Para Operadores
- Consulte os guias de operação diária
- Utilize os logs e monitoramento
- Siga os procedimentos de recuperação de falhas

## 🚀 Início Rápido

1. **Instalação do Vortex Server**
   - Configure PostgreSQL, QuestDB e RabbitMQ
   - Instale .NET 9 e dependências
   - Execute o servidor principal

2. **Configuração via Vortex API**
   - Configure equipamentos e tags
   - Defina coletores e gateways
   - Teste via interface Swagger

3. **Deploy dos Vortex Collectors**
   - Configure conexões OPC DA
   - Inicie os coletores nos equipamentos
   - Monitore a coleta de dados

## 📞 Suporte

Para suporte técnico e questões sobre a Plataforma Vortex:

- **Documentação**: Esta documentação completa
- **Logs**: Verificar logs detalhados de cada componente
- **Monitoramento**: Utilizar ferramentas de monitoramento integradas

## 🔄 Atualizações

Esta documentação é atualizada regularmente conforme novas versões e funcionalidades são adicionadas à Plataforma Vortex.

---

**Desenvolvido por**: Henrique Morais
**Copyright**: © Veter Engineering
**Versão da Documentação**: 1.0