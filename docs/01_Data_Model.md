# Modelo dimensional #
Modelo dimensional escolhido foi: Star Schema

Tabelas Possivelmente criadas:
    - dim_customer (dimensão de clientes)
    - dim_agent (dimensão de agentes)
    - dim_product (dimensão de produtos)
    - dim_category (dimensão de categorias)
    - dim_channel (dimensão de canais)
    - dim_priority (dimensão de prioridades)
    - dim_status (dimensão de status)
    - fact_tickets (armazenará os acontecimentos da operação de suporte)

## Grão da Tabela Fato ##
Cada linha da fact_tickets representa um chamado individual registrado na plataforma de suporte
    ticket_id = TKT-00000001

## Estruturas das tabelas ##

### Dim_Custumer ###
Informações dos clientes da Nexora Systems.
    - customer_id 
    - customer_nome
    - customer_segmento
    - customer_size
    - customer_estado
    - customer_regiao
    - contract_plano
    - contract_dt_inicio
    - customer_ativo
  
Primary Key: custumer_id

### Dim_Agent ###
Informações sobre os profissionais do suporte da Nexora Systems.
    - agent_id
    - agent_nome
    - agent_nivel_suporte
    - agent_equipe
    - agent_senioridade
    - agent_dt_admissao
    - agent_turno
    - agent_ativo
  
### Dim_Produto ###
Produtos oferecidos pela empresa Nexora Systems.
    - product_id
    - product_name
    - product_family
    - product_version
    - criticality_level 


### Dim_Category ###
Classificação dos chamados.
    - category_id
    - category_nome
    - subcategory_nome
    - category_grupo

### Dim_Channel ###
Canal de abertura.
    - channel_id
    - channel_name
  
## Dim_Priority ###
Prioridade de chamado
    - priority_id
    - priority_nome
    - priority_peso
  
### Dim_Status ### 
Situação do chamado.
    - status_id
    - status_nome
    - status_grupo

### Dim_SLA_Policy ### 
Dimensão ou tabela de política 
    - sla_policy_id
    - priority_id
    - contract_plan
    - response_sla_minutes
    - resolution_sla_minutes
    - valido_de
    - valido_ate
    - sla_ativo

### Fact_Tickets ###
    - ticket_id
    - customer_id
    - agent_id
    - product_id
    - category_id
    - channel_id
    - priority_id
    - status_id
    - sla_policy_id
    - dt_criacao
    - dt_primeira_resposta
    - dt_resolucao
    - dt_finalizacao
    - escalado
    - nivel_ticket
    - reaberto
    - qtd_reabertura
    - qtd_transferido
    - qtd_iteracoes
    - escala_satisfacao
    - dt_escala_satisfacao

## Regras de negócio ##
### Backlog ###
Um chamado faz parte do backlog quando:
    status não está em: Resolvido,Fechado,Cancelado

### escala_satisfacao ###
de 1 a 5
1 = Very Dissatisfied
2 = Dissatisfied
3 = Neutral
4 = Satisfied
5 = Very Satisfied

somente pode ser informado quando dt_resolucao e dt_finalizacao estiverem preenchidos

## Relacionamentos ##
Todos serão de valores 1:N

Dim_Customer ────────────┐
Dim_Agent ───────────────┤
Dim_Product ─────────────┤
Dim_Category ────────────┤── Fact_Tickets
Dim_Channel ─────────────┤
Dim_Priority ────────────┼
Dim_Status ──────────────┤
Dim_SLA_Policy ──────────┘

dim_customer.customer_id     (1) - (N) fact_tickets.customer_id
dim_agent.agent_id           (1) - (N) fact_tickets.agent_id
dim_product.product_id       (1) - (N) fact_tickets.product_id
dim_category.category_id     (1) - (N) fact_tickets.category_id
dim_channel.channel_id       (1) - (N) fact_tickets.channel_id
dim_priority.priority_id     (1) - (N) fact_tickets.priority_id
dim_status.status_id         (1) - (N) fact_tickets.status_id
dim_sla_policy.sla_policy_id (1) - (N) fact_tickets.sla_policy_id

