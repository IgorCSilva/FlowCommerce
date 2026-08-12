# Phase E — Inventário de Sistemas e Integrações

Esta fase inventaria todos os sistemas envolvidos na tarefa usando apenas informações verificadas ou explicitamente referenciadas. Campos sem evidência são marcados como **Não especificado** ou **Desconhecido**. Nenhuma aplicação, API, banco de dados, cache, broker, queue, worker, scheduler, motor de busca ou provedor é inventado.

Resultados de fases anteriores foram utilizados como informações importantes para a elaboração deste conteúdo, aqui mencionados como Phase {letra}.

---

## Cobertura do inventário (tipos solicitados)

| Tipo | Status nesta tarefa |
|------|---------------------|
| Aplicações / módulos | Presente — ver seções Verificados e Referenciados |
| APIs | **Nenhuma especificada** (Phase A) |
| Bancos de dados | **Nenhum especificado** (Phase A) |
| Caches | **Nenhum especificado** |
| Brokers | **Nenhum especificado** |
| Queues | **Nenhuma especificada** (Phase A) |
| Workers | **Nenhum especificado** |
| Schedulers | **Nenhum especificado** |
| Motores de busca | **Nenhum especificado** como motor; o módulo do ecossistema **Busca** é apenas referenciado (ver Referenciados) |
| Serviços externos | Presente — canal WhatsApp |
| Provedores terceirizados | Provedor/API de WhatsApp **referenciado como desconhecido** (Phase A); não selecionado |

---

## Componentes Verificados

Explicitamente identificados como envolvidos nesta tarefa (escopo técnico da Phase A; componentes dentro do escopo da Phase D).

### VC-001 — Pedidos

| Campo | Valor |
|-------|---------|
| **Nome** | Pedidos |
| **Tipo** | Aplicação / módulo |
| **Responsável** | Não atribuído (Phase D OG-001) |
| **Responsabilidade** | Fornecer o contexto do pedido no qual as atualizações de pedido ocorrem; fornecer a identidade do pedido para que uma notificação via WhatsApp possa ser associada a um pedido específico (Phase D) |
| **Entrada** | Atualizações de pedido / momentos do ciclo de vida do pedido (conjunto exato dentro do escopo a definir — Phase D cita MR-001, MR-003, MR-010) |
| **Saída** | Identidade do pedido e o significado da atualização necessários para iniciar uma notificação dentro do escopo |
| **Protocolo** | Não especificado |
| **Dependência** | Notificações por E-mail (caminho atual de notificação de pedidos existente hoje); canal WhatsApp para o novo caminho de entrega (política de canal a definir) |
| **Ambiente** | Não especificado |
| **Modos de falha conhecidos** | Não especificado nas fontes. A falha na disponibilidade de atualizações de pedido bloquearia o início de notificações via WhatsApp dentro do escopo (implícito pelo CAP-003 / Phase D), mas nenhum modo de falha concreto está documentado. |

### VC-002 — Notificações por E-mail

| Campo | Valor |
|-------|---------|
| **Nome** | Notificações por E-mail |
| **Tipo** | Aplicação / módulo |
| **Responsável** | Não atribuído (Phase D OG-002) |
| **Responsabilidade** | Entregar notificações por e-mail relacionadas a pedidos como hoje; participar da eventual política de canal e-mail↔WhatsApp sem presumir substituição vs. complementação vs. alternativa (Phase D) |
| **Entrada** | Gatilhos de notificação relacionados a pedidos já usados para e-mail (inventário do estado atual a definir) |
| **Saída** | Notificações por e-mail para atualizações de pedido (comportamento atual) |
| **Protocolo** | Não especificado |
| **Dependência** | Pedidos (atualizações de pedido); decisão da política de canal antes que o comportamento de canal duplo possa ser finalizado |
| **Ambiente** | Não especificado |
| **Modos de falha conhecidos** | Não especificado. A Phase A estabelece que os clientes perdem algumas mensagens de e-mail (problema de negócio), o que é um problema de alcance/atenção — não um modo de falha técnica documentado do módulo Notificações por E-mail. |

### VC-003 — WhatsApp (canal externo)

| Campo | Valor |
|-------|---------|
| **Nome** | WhatsApp |
| **Tipo** | Serviço externo / canal de comunicação |
| **Responsável** | Não atribuído internamente; o provedor externo também está a definir (Phase D OG-003) |
| **Responsabilidade** | Canal de mensagens externo que recebe notificações de atualização de pedido para entrega aos destinatários designados (Phase D; CAP-006) |
| **Entrada** | Conteúdo de notificação associado ao pedido, endereçado a um destinatário designado (destinatário e campos de conteúdo a definir) |
| **Saída** | Mensagem aceita para entrega, ou uma falha de entrega/envio distinguível para fins de observabilidade (CAP-008) |
| **Protocolo** | Não especificado (provedor/API não selecionado) |
| **Dependência** | A experiência de notificação da FlowCommerce deve submeter os envios; o uso permitido depende de regras de consentimento/regulatórias ainda não resolvidas (Phase D OG-008) |
| **Ambiente** | Externo à FlowCommerce (Phase A: ferramenta/provedor externo) |
| **Modos de falha conhecidos** | A falha de envio deve ser distinguível da submissão bem-sucedida para entrega (CAP-008 / Phase D). Modos de falha técnica específicos (timeouts, rejeições, indisponibilidades do provedor) **não são especificados**. |

---

## Componentes Referenciados mas Indefinidos

Mencionados no ecossistema ou no contexto da tarefa, mas sem informação suficiente para serem tratados como alvos de integração inventariados nesta tarefa.

### RU-001 — Gestão de Clientes

| Campo | Valor |
|-------|---------|
| **Nome** | Gestão de Clientes |
| **Tipo** | Aplicação / módulo (ecossistema) |
| **Responsável** | Desconhecido |
| **Responsabilidade** | Não atribuída para esta tarefa. A identidade do destinatário é necessária (Phase D CAP-005 / OG-006), mas **não** está verificado que seja de responsabilidade deste módulo. |
| **Entrada** | Não especificado |
| **Saída** | Não especificado |
| **Protocolo** | Não especificado |
| **Dependência** | Não especificado |
| **Ambiente** | Não especificado |
| **Modos de falha conhecidos** | Não especificado |
| **Por que indefinido** | A Phase A o lista no ecossistema, mas não como uma aplicação/serviço dentro do escopo desta solicitação. Phase D: não confirmado como dentro do escopo. |

### RU-002 — Catálogo de Produtos

| Campo | Valor |
|-------|---------|
| **Nome** | Catálogo de Produtos |
| **Tipo** | Aplicação / módulo (ecossistema) |
| **Responsável** | Desconhecido |
| **Responsabilidade** | Nenhuma para esta tarefa (Phase D OOS-002) |
| **Entrada / Saída / Protocolo / Dependência / Ambiente / Modos de falha conhecidos** | Não especificado |
| **Por que indefinido** | Apenas menção no ecossistema; explicitamente fora do escopo para mudanças funcionais. |

### RU-003 — Pagamentos

| Campo | Valor |
|-------|---------|
| **Nome** | Pagamentos |
| **Tipo** | Aplicação / módulo (ecossistema) |
| **Responsável** | Desconhecido |
| **Responsabilidade** | Nenhum trabalho de nova funcionalidade dentro do escopo (Phase D OOS-006). Pode apenas contribuir com *momentos do ciclo de vida do pedido*, se posteriormente aprovados como eventos dentro do escopo. |
| **Entrada / Saída / Protocolo / Dependência / Ambiente / Modos de falha conhecidos** | Não especificado |
| **Por que indefinido** | Referenciado no ecossistema; não confirmado como um componente de integração para esta tarefa. |

### RU-004 — Envios

| Campo | Valor |
|-------|---------|
| **Nome** | Envios |
| **Tipo** | Aplicação / módulo (ecossistema) |
| **Responsável** | Desconhecido |
| **Responsabilidade** | Mesmo status de Pagamentos (Phase D OOS-006 / nota sobre fonte de eventos) |
| **Entrada / Saída / Protocolo / Dependência / Ambiente / Modos de falha conhecidos** | Não especificado |
| **Por que indefinido** | Referenciado no ecossistema; não confirmado como um componente de integração para esta tarefa. |

### RU-005 — Busca

| Campo | Valor |
|-------|---------|
| **Nome** | Busca |
| **Tipo** | Aplicação / módulo (ecossistema). **Não** identificado como um produto de motor de busca. |
| **Responsável** | Desconhecido |
| **Responsabilidade** | Nenhuma para esta tarefa (Phase D OOS-002) |
| **Entrada / Saída / Protocolo / Dependência / Ambiente / Modos de falha conhecidos** | Não especificado |
| **Por que indefinido** | Apenas menção no ecossistema; nenhum motor de busca (ex.: índice, cluster) é especificado. |

### RU-006 — Provedor/API de WhatsApp

| Campo | Valor |
|-------|---------|
| **Nome** | Provedor/API de WhatsApp (sem nome) |
| **Tipo** | Provedor terceirizado / API |
| **Responsável** | Desconhecido |
| **Responsabilidade** | Operacionalizaria a entrega através do canal WhatsApp (desconhecido conhecido da Phase A; Phase D OOS-003) |
| **Entrada** | Não especificado |
| **Saída** | Não especificado |
| **Protocolo** | Não especificado |
| **Dependência** | Depende do VC-003 (WhatsApp como conceito de canal) |
| **Ambiente** | Externo; não selecionado |
| **Modos de falha conhecidos** | Não especificado |
| **Por que indefinido** | Phase A: WhatsApp solicitado; **nenhum provedor/API especificado**. Não deve ser inventado neste inventário. |

### RU-007 — Experiência de notificação (coordenador sem nome)

| Campo | Valor |
|-------|---------|
| **Nome** | Experiência de notificação (conceito transversal) |
| **Tipo** | Sem nome — não é uma aplicação separada verificada |
| **Responsável** | Desconhecido (Phase D OG-011) |
| **Responsabilidade** | A Phase D descreve uma experiência de notificação existente baseada em e-mail que deve incorporar o WhatsApp sob uma política de canal coerente. Nenhuma aplicação separada é nomeada além de Pedidos, Notificações por E-mail e WhatsApp. |
| **Entrada / Saída / Protocolo / Dependência / Ambiente / Modos de falha conhecidos** | Não especificado como um componente distinto |
| **Por que indefinido** | Referenciado conceitualmente nas Phases A/D; **carece de um sistema nomeado** e não deve ser inventariado como um serviço fabricado. |

### Explicitamente ausente (não referenciado como sistemas existentes)

Conforme a Phase A / Phase D OOS-007, os seguintes tipos **não têm instâncias nomeadas** para esta tarefa:

- APIs
- Bancos de dados
- Caches
- Brokers
- Queues
- Workers
- Schedulers
- Motores de busca (como infraestrutura)

Eles são listados aqui para que o inventário seja completo por omissão — não para que se presuma que existem.

---

## Componentes Potencialmente Necessários

Apenas capacidades **estritamente implícitas** pelas capacidades dentro do escopo da Phase D, para as quais nenhum componente verificado ainda é responsável. Estas são **lacunas de capacidade**, não licenças para inventar infraestrutura.

### PR-001 — Fonte de identidade do destinatário

| Campo | Valor |
|-------|---------|
| **Nome** | Fonte de identidade do destinatário sem nome |
| **Tipo** | Desconhecido (não deve presumir Gestão de Clientes) |
| **Responsável** | Desconhecido (Phase D OG-006) |
| **Responsabilidade** | Fornecer o destinatário designado para cada notificação de pedido via WhatsApp (CAP-005) |
| **Entrada** | Pedido (e quaisquer chaves de identidade definidas posteriormente) — não especificado |
| **Saída** | Informações de endereçamento do destinatário designado — não especificado |
| **Protocolo** | Não especificado |
| **Dependência** | Pedidos (associação ao pedido); WhatsApp (endereçamento de entrega) |
| **Ambiente** | Não especificado |
| **Modos de falha conhecidos** | Não especificado. Um destinatário ausente ou incorreto violaria o CAP-005. |
| **Por que potencialmente necessário** | CAP-005 / Phase D OG-006 exigem um destinatário designado; nenhum componente verificado está atribuído. |

### PR-002 — Caminho de início do envio via WhatsApp

| Campo | Valor |
|-------|---------|
| **Nome** | Caminho de início de envio sem nome |
| **Tipo** | Desconhecido (não deve inventar um orquestrador, worker ou queue) |
| **Responsável** | Desconhecido (Phase D OG-011) |
| **Responsabilidade** | Quando ocorre uma atualização de pedido dentro do escopo, iniciar uma notificação via WhatsApp (CAP-003) |
| **Entrada** | Sinal de atualização de pedido dentro do escopo — conjunto a definir |
| **Saída** | Início de um envio via WhatsApp em direção ao VC-003 |
| **Protocolo** | Não especificado |
| **Dependência** | Pedidos; WhatsApp; política de canal em relação às Notificações por E-mail |
| **Ambiente** | Não especificado |
| **Modos de falha conhecidos** | Não especificado. A falha ao iniciar violaria o CAP-003. |
| **Por que potencialmente necessário** | O CAP-003 é obrigatório; a Phase D adia explicitamente a atribuição entre Pedidos vs. Notificações por E-mail vs. um coordenador sem nome, para evitar arquitetura especulativa. |

### PR-003 — Visibilidade de sucesso/falha do envio via WhatsApp

| Campo | Valor |
|-------|---------|
| **Nome** | Mecanismo de visibilidade do resultado do envio sem nome |
| **Tipo** | Desconhecido (não deve inventar uma stack de monitoramento) |
| **Responsável** | Desconhecido (Phase D OG-010) |
| **Responsabilidade** | Tornar a submissão bem-sucedida para entrega distinguível da falha de envio (CAP-008) |
| **Entrada** | Resultado da tentativa de envio via WhatsApp |
| **Saída** | Sinal de sucesso vs. falha operacionalmente distinguível (formato a definir) |
| **Protocolo** | Não especificado |
| **Dependência** | WhatsApp (VC-003); possivelmente o eventual provedor (RU-006) |
| **Ambiente** | Não especificado |
| **Modos de falha conhecidos** | O requisito existe **porque** a falha de envio é um modo relevante; modos concretos não estão documentados. |
| **Por que potencialmente necessário** | O CAP-008 é obrigatório para validar o CAP-001; nenhum componente verificado é responsável pela forma de observabilidade. |

---

## Mapa de dependências (texto)

Apenas dependências verificadas. Links conceituais tracejados apontam caminhos não resolvidos sem adicionar nós inventados como sistemas reais.

```text
                    [Momentos do ciclo de vida do pedido]
                    (conjunto a definir; pode incluir depois
                     *eventos* de Pagamentos/Envios
                     — os módulos em si não estão no inventário como integrações)
                              |
                              v
                         +---------+
                         | Pedidos |  VC-001
                         +----+----+
                              |
      caminho existente       |   novo caminho necessário (política a definir)
                              |
              +---------------+---------------+
              |                               |
              v                               v
   +----------------------+        +-------------------+
   | Notificações por     |        | Canal WhatsApp    |  VC-003
   | E-mail — VC-002      |        | (externo)         |
   +----------------------+        +--------+----------+
              ^                               |
              |                               |
              +-------- política de canal ----+
                     (MR-004 / CAP-007;
                      valor único por atualização;
                      ainda não decidido)

   Provedor/API de WhatsApp (RU-006) ........ operacionaliza o VC-003
                                               (referenciado, não selecionado)

   Fonte de identidade do destinatário (PR-001) --> necessária para endereçar o VC-003
                                                     (implícita, componente sem nome)

   Caminho de início do envio (PR-002) ------> atualização de Pedidos --> envio VC-003
                                                (implícito, responsável sem nome)

   Visibilidade do resultado do envio (PR-003) <----- sucesso/falha do VC-003
                                                       (implícita, mecanismo sem nome)

   Gestão de Clientes / Catálogo de Produtos / Busca
        (RU-001, RU-002, RU-005) --- apenas ecossistema; não conectados aqui

   APIs / DBs / caches / brokers / queues / workers /
   schedulers / motores de busca ---------------- NENHUM ESPECIFICADO
```

**Lista de arestas (verificadas ou explicitamente registradas):**

| De | Para | Relação | Nível de evidência |
|------|----|--------------|----------------|
| Pedidos | Notificações por E-mail | Caminho existente de notificação de pedidos | Verificado (Phase A/D) |
| Pedidos | WhatsApp | Novo caminho de entrega para atualizações de pedido (política a definir) | Necessidade verificada; atribuição do caminho incompleta |
| Notificações por E-mail | WhatsApp | Acoplados pela política de canal (CAP-007) | Obrigação verificada; política indefinida |
| Experiência de notificação da FlowCommerce | WhatsApp | Deve submeter os envios | Verificado (Phase D) |
| Provedor/API de WhatsApp | Canal WhatsApp | Operacionalizaria o canal | Referenciado, indefinido |
| Fonte de identidade do destinatário | WhatsApp | Endereçamento | Potencialmente necessário |
| Caminho de início do envio | WhatsApp | Iniciar o envio em uma atualização dentro do escopo | Potencialmente necessário |
| WhatsApp | Visibilidade do resultado do envio | Sucesso vs. falha | Potencialmente necessário |

---

## Avaliação Final

| Pergunta | Resposta |
|----------|--------|
| Quais sistemas estão verificados para esta tarefa? | **Pedidos**, **Notificações por E-mail**, **WhatsApp** (canal externo) |
| Qual infraestrutura está verificada? | **Nenhuma** — nenhuma API, banco de dados, cache, broker, queue, worker, scheduler ou motor de busca especificado |
| O que bloqueia um inventário de integração completo? | Fonte de destinatário sem nome; atribuição de início de envio sem nome; visibilidade de resultado de envio sem nome; provedor/API de WhatsApp não selecionado; política de canal e conjunto de eventos indefinidos |
| Pronto para a arquitetura-alvo (Phase F)? | **Não** — o inventário é honesto, mas incompleto; projetar a arquitetura-alvo agora exigiria inventar infraestrutura ou responsáveis |

**Condicionalmente completo como um inventário baseado em evidências.** Completo como um registro do que é conhecido e desconhecido; incompleto como um mapa de integração pronto para construção.
