# 💼 Proposta Comercial - Vortex Server

<div style="text-align: center; padding: 2rem 1rem;">
  <img src="../assets/vortex.png" alt="Vortex" width="120" height="120" style="max-width: 100%; height: auto;">
  <h2 style="margin: 1rem 0 0.5rem 0; font-size: clamp(1.5rem, 4vw, 2.5rem);">Transforme sua Indústria com Inteligência de Dados</h2>
  <p style="margin: 0; color: #666; font-size: clamp(1rem, 2.5vw, 1.2rem);">Solução completa para coleta, processamento e análise de dados industriais em tempo real</p>
</div>

## 🎯 Visão Geral da Solução

O **Vortex Server** é a solução completa para **coleta, processamento e análise de dados industriais** em tempo real. Nossa plataforma oferece uma arquitetura robusta e escalável que permite às empresas industriais extrair valor máximo de seus equipamentos e processos.

### Por que Escolher o Vortex Server?

<div class="grid cards" markdown>

-   :material-lightning-bolt:{ .lg .middle } **Tempo Real**

    ---

    Coleta e processamento de dados em tempo real com latência ultra-baixa, garantindo decisões rápidas e precisas.

-   :material-shield-check:{ .lg .middle } **Confiabilidade**

    ---

    Arquitetura robusta com alta disponibilidade e recuperação de falhas em segundos.

-   :material-chart-line:{ .lg .middle } **Escalabilidade**

    ---

    Cresce com seu negócio. Suporte a milhares de pontos de dados e múltiplos sites industriais.

-   :material-cog:{ .lg .middle } **Facilidade de Uso**

    ---

    Interface intuitiva e configuração simplificada. Operação sem necessidade de especialistas técnicos.

</div>

## 🏭 Fluxo Completo do Produto

### 1. Coleta de Dados Industriais

```mermaid
graph TD
    A[Equipamentos Industriais] --> B[PLC/Sensores]
    B --> C[Protocolo OPC DA]
    C --> D[Vortex Collector]
    D --> E[RabbitMQ]
    E --> F[Vortex Server]
    F --> G[Processamento]
    G --> H[Armazenamento]
```

**Características da Coleta:**
- ✅ Suporte a protocolos industriais padrão (OPC DA)
- ✅ Descoberta automática de equipamentos
- ✅ Coleta contínua 24/7 sem interrupções
- ✅ Tolerância a falhas de rede e equipamentos

## 🎬 Demonstração da Arquitetura

<div class="video-container" style="text-align: center; margin: 2rem 0;">

<video width="800" height="450" controls style="border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <source src="../assets/arquitetura.mp4" type="video/mp4">
  Seu navegador não suporta vídeos HTML5.
</video>

<p><em>Demonstração da arquitetura Vortex Server</em></p>

</div>

### 2. Processamento e Orquestração

O **Vortex Server** atua como o cérebro da operação:

<div class="grid cards" markdown>

-   :material-magnify:{ .lg .middle } **Descoberta Automática**

    ---

    Identifica automaticamente novos coletores e equipamentos na rede industrial.

-   :material-sync:{ .lg .middle } **Orquestração Inteligente**

    ---

    Coordena múltiplos coletores e otimiza o fluxo de dados em tempo real.

-   :material-filter:{ .lg .middle } **Processamento Avançado**

    ---

    Filtra, valida e transforma dados.

-   :material-alert:{ .lg .middle } **Alertas Inteligentes**

    ---

    Sistema de alertas configurável baseado em regras de negócio.

</div>

### 3. Armazenamento Otimizado

**Arquitetura Híbrida de Dados:**

| Tipo de Dados | Banco de Dados | Benefícios |
|---------------|----------------|------------|
| **Configurações** | PostgreSQL | Transações ACID, integridade referencial |
| **Dados Temporais** | InfluxDB | Alta performance, análise de séries temporais |
| **Métricas** | Grafana | Visualização e análise de dados em tempo real |

### 4. Visualização e Análise

```mermaid
graph LR
    A[Dados Armazenados] --> B[Grafana Dashboard]
    A --> C[Vortex API]
    A --> D[Relatórios Customizados]
    
    B --> E[Visualização em Tempo Real]
    C --> F[Integração com Sistemas]
    D --> G[Análise de Tendências]
```

## 🚀 Casos de Uso Principais

<div class="grid cards" markdown>

-   :material-monitor:{ .lg .middle } **Monitoramento de Equipamentos Críticos**

    ---

    **Desafio:** Falhas inesperadas em equipamentos críticos causam paradas custosas.

    **Solução Vortex:**
    - Monitoramento contínuo de temperatura, vibração, pressão
    - Alertas baseados em padrões históricos
    - Manutenção preventiva programada

    **Resultado:** Redução nas falhas críticas.

-   :material-cog:{ .lg .middle } **Otimização de Processos de Produção**

    ---

    **Desafio:** Processos industriais com baixa eficiência e desperdício de recursos.

    **Solução Vortex:**
    - Análise em tempo real de parâmetros de processo
    - Identificação de gargalos e oportunidades de melhoria
    - Controle automático de variáveis críticas

    **Resultado:** Aumento na eficiência de produção.

-   :material-leaf:{ .lg .middle } **Gestão de Energia e Sustentabilidade**

    ---

    **Desafio:** Alto consumo energético e necessidade de redução de pegada de carbono.

    **Solução Vortex:**
    - Monitoramento detalhado do consumo energético
    - Identificação de picos de consumo e desperdícios
    - Otimização automática de cargas

    **Resultado:** Redução no consumo energético.

</div>



## 💡 Próximos Passos

<div class="grid cards" markdown>

-   :material-presentation:{ .lg .middle } **Demonstração Personalizada**

    ---

    Agende uma demonstração personalizada para sua indústria:
    - Análise de seus equipamentos atuais
    - Simulação com seus dados reais
    - Apresentação de casos similares ao seu

-   :material-test-tube:{ .lg .middle } **Prova de Conceito (POC)**

    ---

    Implementação piloto em ambiente controlado:
    - Duração: 30 dias
    - Equipamentos: Até 10 pontos de dados
    - Suporte técnico completo incluído

-   :material-file-document:{ .lg .middle } **Proposta Comercial Detalhada**

    ---

    Após a POC, apresentaremos:
    - Proposta comercial personalizada
    - Cronograma de implementação
    - Plano de treinamento e suporte

</div>

## 📞 Contato Comercial

<div class="contact-section" style="text-align: center; padding: 2rem; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); border-radius: 12px; margin: 2rem 0; color: white;">
  
  <h3 style="margin: 0 0 1rem 0; color: white;">🚀 Pronto para transformar sua indústria?</h3>
  
  <div style="margin: 1.5rem 0;">
    <a href="mailto:comercial@veter.com.br" style="display: inline-block; padding: 1rem 2rem; background: rgba(255,255,255,0.2); color: white; text-decoration: none; border-radius: 8px; font-weight: bold; transition: all 0.3s ease;">
      📞 Fale Conosco - Contato Comercial
    </a>
  </div>
  
  <div style="margin-top: 1.5rem;">
    <strong>Veter Engineering</strong><br>
    <em>Especialistas em Automação Industrial</em>
  </div>
  
</div>

---

<div class="footer-section" style="text-align: center; margin-top: 3rem; padding: 1.5rem; background: #f8f9fa; border-radius: 8px;">
    <small style="color: #6c757d;">
        <strong>Vortex Server</strong> - Transformando dados industriais em vantagem competitiva<br>
        © 2024 Veter Engineering. Todos os direitos reservados.
    </small>
</div>
