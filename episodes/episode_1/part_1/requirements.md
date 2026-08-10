# Phase C — Definição de Requisitos

Esta fase se apoiou em resultados de fases anteriores, chamadas aqui de Phase A e Phase B.

Este documento converte a tarefa esclarecida em um conjunto de requisitos testável. Ele não projeta a implementação nem atribui soluções em nível de código. Valores que não estão definidos nas Phases A e B são listados em Requisitos Faltantes e não são adivinhados.

---

## Catálogo de Requisitos

### Requisitos Explícitos

Declarados diretamente na Phase A / Phase B.

#### REQ-EX-001

| Campo | Conteúdo |
|-------|---------|
| **ID** | REQ-EX-001 |
| **Requisito** | O sistema deve enviar atualizações de pedidos pelo WhatsApp. |
| **Tipo** | Funcional |
| **Prioridade** | Obrigatório (única capacidade explicitamente solicitada) |
| **Origem** | Solicitação do Customer Success (Phase A R-1) |
| **Comportamento de negócio afetado** | As atualizações de pedidos se tornam alcançáveis via WhatsApp, além do canal atual, que é somente e-mail (política de canal exata a definir). |
| **Área do sistema afetada** | Pedidos; Notificações por E-mail (experiência de notificação existente); WhatsApp como canal de comunicação externo |
| **Método de validação** | Para cada atualização de pedido no conjunto de notificações aprovado (a definir), confirmar que uma mensagem via WhatsApp para essa atualização é submetida para entrega ao destinatário aprovado (a definir). A validação está bloqueada até que os Requisitos Faltantes MR-001, MR-002 e MR-003 sejam resolvidos. |

#### REQ-EX-002

| Campo | Conteúdo |
|-------|---------|
| **ID** | REQ-EX-002 |
| **Requisito** | Os clientes devem receber notificações relevantes de pedidos pelo WhatsApp, de modo que atualizações importantes de pedidos sejam mais fáceis de alcançar do que apenas com o e-mail. |
| **Tipo** | Funcional |
| **Prioridade** | Obrigatório (resultado esperado declarado) |
| **Origem** | Resultado esperado declarado na Phase A; Objetivo de Negócio / Comportamento Desejado da Phase B |
| **Comportamento de negócio afetado** | Clientes das empresas-clientes da FlowCommerce podem receber notificações relevantes de pedidos no WhatsApp; o efeito pretendido é menos notificações perdidas e menos incerteza sobre o status do pedido. |
| **Área do sistema afetada** | Pedidos; experiência de notificação; canal WhatsApp |
| **Método de validação** | Confirmar que os destinatários aprovados recebem mensagens via WhatsApp para o conjunto aprovado de notificações relevantes de pedidos (conjuntos a definir). As medidas de resultado de negócio (taxa de notificações perdidas, volume de suporte relacionado) exigem o MR-006 antes que um resultado objetivo de aprovação/reprovação possa ser afirmado. |

---

### Requisitos Derivados

Estritamente necessários para satisfazer os requisitos explícitos. Valores de negócio específicos (quais eventos, quem, quando, política de canal) não são inventados; quando esses valores são desconhecidos, o requisito derivado declara a obrigação e aponta para a definição faltante.

#### REQ-DR-001

| Campo | Conteúdo |
|-------|---------|
| **ID** | REQ-DR-001 |
| **Requisito** | Quando ocorrer uma atualização de pedido designada como dentro do escopo para o WhatsApp, o sistema deve iniciar uma notificação via WhatsApp para essa atualização. |
| **Tipo** | Funcional |
| **Prioridade** | Obrigatório |
| **Origem** | Derivado do REQ-EX-001 |
| **Comportamento de negócio afetado** | O envio pelo WhatsApp está vinculado aos momentos de atualização de pedido, não é enviado arbitrariamente. |
| **Área do sistema afetada** | Pedidos; disparo de notificação; canal WhatsApp |
| **Método de validação** | Disparar uma atualização de pedido dentro do escopo e verificar que um envio via WhatsApp é iniciado; disparar uma atualização explicitamente fora do escopo (uma vez que o conjunto seja definido) e verificar que nenhum envio via WhatsApp é iniciado. |
| **Por que é necessário** | O REQ-EX-001 exige que atualizações de pedidos sejam enviadas pelo WhatsApp; o envio deve estar vinculado à ocorrência dessas atualizações. |

#### REQ-DR-002

| Campo | Conteúdo |
|-------|---------|
| **ID** | REQ-DR-002 |
| **Requisito** | Cada notificação de pedido via WhatsApp deve estar associada a um pedido específico e transmitir que uma atualização de pedido ocorreu para esse pedido. |
| **Tipo** | Dados |
| **Prioridade** | Obrigatório |
| **Origem** | Derivado do REQ-EX-001 ("atualizações de pedidos") |
| **Comportamento de negócio afetado** | Os destinatários conseguem relacionar a mensagem via WhatsApp à atualização de pedido correta, em vez de receber uma mensagem sem escopo definido. |
| **Área do sistema afetada** | Pedidos; conteúdo da notificação; canal WhatsApp |
| **Método de validação** | Inspecionar uma notificação via WhatsApp enviada e verificar que ela referencia o pedido correto e reflete a atualização que a disparou (campos de conteúdo exatos a definir no MR-007). |
| **Por que é necessário** | Sem a associação ao pedido e o significado da atualização, a mensagem não é uma notificação de "atualização de pedido" e não satisfaz o REQ-EX-001 / REQ-EX-002. |

#### REQ-DR-003

| Campo | Conteúdo |
|-------|---------|
| **ID** | REQ-DR-003 |
| **Requisito** | Cada notificação de pedido via WhatsApp deve ser endereçada ao destinatário designado para essa atualização de pedido. |
| **Tipo** | Regra de Negócio |
| **Prioridade** | Obrigatório |
| **Origem** | Derivado do REQ-EX-001 e do REQ-EX-002 |
| **Comportamento de negócio afetado** | As notificações alcançam a parte que deveria ser informada sobre a atualização de pedido. |
| **Área do sistema afetada** | Pedidos; identidade do cliente/destinatário usada para notificações; canal WhatsApp |
| **Método de validação** | Para um pedido conhecido e um destinatário designado (uma vez que o MR-002 seja definido), verificar que a mensagem via WhatsApp é endereçada a esse destinatário e não a uma parte incorreta. |
| **Por que é necessário** | Enviar sem um destinatário definido não consegue cumprir "os clientes devem receber" (REQ-EX-002) nem produzir uma mensagem via WhatsApp entregável (REQ-EX-001). |

#### REQ-DR-004

| Campo | Conteúdo |
|-------|---------|
| **ID** | REQ-DR-004 |
| **Requisito** | O sistema deve usar o WhatsApp como um canal de mensagens externo para entregar as notificações de atualização de pedido exigidas pelo REQ-EX-001. |
| **Tipo** | Integração |
| **Prioridade** | Obrigatório |
| **Origem** | Derivado do REQ-EX-001 (o WhatsApp é externo à FlowCommerce; a Phase A lista o WhatsApp como ferramenta/provedor externo) |
| **Comportamento de negócio afetado** | As atualizações de pedidos saem da experiência de notificação da FlowCommerce e alcançam os clientes no WhatsApp. |
| **Área do sistema afetada** | Experiência de notificação; canal externo WhatsApp |
| **Método de validação** | Verificação ponta a ponta de que uma atualização de pedido dentro do escopo resulta em uma mensagem entregue (ou aceita para entrega) no WhatsApp para o destinatário designado. A escolha do provedor/API é o MR-008 e não deve ser presumida nos testes além de "canal WhatsApp". |
| **Por que é necessário** | O REQ-EX-001 nomeia o WhatsApp como o canal de entrega; a entrega não pode ser satisfeita apenas pelo e-mail. |

#### REQ-DR-005

| Campo | Conteúdo |
|-------|---------|
| **ID** | REQ-DR-005 |
| **Requisito** | As notificações de pedido via WhatsApp devem seguir a política de canal de negócio em relação às notificações por e-mail existentes (complementar, substituir, alternativa, ou outra regra aprovada), uma vez que essa política seja definida. |
| **Tipo** | Regra de Negócio |
| **Prioridade** | Obrigatório |
| **Origem** | Derivado do REQ-EX-001 / REQ-EX-002 e do comportamento desejado da Phase B + ambiguidade #4; a linguagem da Phase A "adicionar o WhatsApp como um canal de comunicação" não é tratada como uma política resolvida |
| **Comportamento de negócio afetado** | Os clientes vivenciam uma política de notificação coerente entre e-mail e WhatsApp, em vez de um resultado indefinido de canal duplo. |
| **Área do sistema afetada** | Notificações por E-mail; canal WhatsApp; experiência de notificação como um todo |
| **Método de validação** | Depois que o MR-004 for decidido, para cada atualização dentro do escopo, verificar que a presença/ausência de e-mail e WhatsApp corresponde à política aprovada. |
| **Por que é necessário** | Os requisitos explícitos introduzem o WhatsApp em uma experiência de notificação por e-mail já existente; sem uma regra de interação entre canais, o REQ-EX-001 não pode ser concluído sem um comportamento conflitante ou indefinido voltado ao cliente. |

#### REQ-DR-006

| Campo | Conteúdo |
|-------|---------|
| **ID** | REQ-DR-006 |
| **Requisito** | Deve ser possível determinar se uma notificação de atualização de pedido via WhatsApp foi submetida com sucesso para entrega ou falhou ao ser enviada. |
| **Tipo** | Observabilidade |
| **Prioridade** | Obrigatório |
| **Origem** | Derivado do REQ-EX-001 (testabilidade de "deve enviar") e da necessidade dos Critérios de Sucesso da Phase B de saber se as mensagens são recebidas/enviadas |
| **Comportamento de negócio afetado** | O negócio e os operadores conseguem distinguir "enviado" de "não enviado" ao avaliar se os clientes estão sendo alcançados. |
| **Área do sistema afetada** | Canal WhatsApp; experiência de notificação |
| **Método de validação** | Produzir um envio bem-sucedido e um envio malsucedido (ou falha simulada) e verificar que cada resultado é distinguível pelo sinal operacional acordado (mecanismo a definir; nenhuma implementação prescrita). |
| **Por que é necessário** | Sem a visibilidade de sucesso/falha do envio, o REQ-EX-001 não pode ser validado e o negócio não consegue saber se o canal está funcionando. |

---

### Não Requisitos

Explicitamente fora do escopo desta tarefa (a partir dos Não Objetivos da Phase B / limites de escopo da Phase A). Estes não devem ser tratados como obrigações de entrega deste conjunto de requisitos.

| ID | Não requisito | Justificativa |
|----|-----------------|-----------|
| NR-001 | Redesenho do produto de notificações como um todo, além de adicionar o WhatsApp para atualizações de pedidos | Não solicitado; Não Objetivos da Phase B |
| NR-002 | Mudanças funcionais no Catálogo de Produtos ou na Busca como parte desta tarefa | Módulos do ecossistema referenciados, mas não confirmados como dentro do escopo; Não Objetivos da Phase B |
| NR-003 | Tratar a seleção do provedor/API de WhatsApp como um requisito de negócio desta fase | Phase A: provedor/API não especificado; Phase B: adiado como questão técnica |
| NR-004 | Definir ou implementar o *conteúdo da política* regulatória, de opt-in/consentimento ou de tratamento de dados como uma decisão de negócio concluída neste conjunto de requisitos | Registrado como desconhecido na Phase A; a Phase B lista a resolução dessa política como fora do que foi solicitado — **uma implementação segura ainda exige o MR-005** antes da construção |
| NR-005 | Percentuais garantidos de redução para chamados de suporte ou taxas de notificações perdidas | Valor de negócio esperado declarado qualitativamente; nenhuma meta fornecida |

Observação: Pagamentos e Envios aparecem no ecossistema e podem ser fontes de momentos do ciclo de vida do pedido, mas a Phase B exclui o *redesenho não solicitado de módulos*. Disparar o WhatsApp a partir de atualizações de pedido originadas nesses domínios continua sujeito ao MR-001 / MR-003, e não à construção de novas funcionalidades de Pagamentos/Envios.

---

### Requisitos Faltantes

Informações necessárias para implementar e validar com segurança, ainda não definidas. **Não adivinhar valores.**

| ID | Informação faltante | Bloqueia | Necessário para |
|----|---------------------|--------|------------|
| MR-001 | Quais notificações/eventos específicos de pedido estão dentro do escopo para o WhatsApp ("notificações relevantes de pedidos"; quais causam confusão) | REQ-EX-001, REQ-EX-002, REQ-DR-001 | Escopo funcional e casos de teste |
| MR-002 | Quem recebe as mensagens via WhatsApp (cliente final vs. usuário do cliente B2B vs. papel/contato do pedido) | REQ-EX-002, REQ-DR-003 | Endereçamento e testes de aceitação |
| MR-003 | Quando uma notificação via WhatsApp deve ser enviada em relação ao ciclo de vida do pedido | REQ-DR-001 | Momento do disparo |
| MR-004 | Como o WhatsApp interage com o e-mail existente (substituir / duplicar / complementar / alternativa de opt-in / regras por mensagem) | REQ-DR-005 | Comportamento coerente multicanal |
| MR-005 | Regras regulatórias, de opt-in/consentimento e de tratamento de dados necessárias para enviar mensagens aos clientes no WhatsApp | Requisitos de Segurança / Regra de Negócio ainda não redigíveis | Envio lícito e permitido |
| MR-006 | Métricas de sucesso, linhas de base e limiares (notificações perdidas, volume de suporte relacionado, engajamento) | Conclusão objetiva dos critérios de sucesso da Phase B | Aceitação de lançamento/negócio |
| MR-007 | Campos de conteúdo de mensagem exigidos para uma "atualização de pedido" no WhatsApp (além da associação ao pedido declarada no REQ-DR-002) | Profundidade de validação do REQ-DR-002 | Aceitação de conteúdo |
| MR-008 | Provedor/API de WhatsApp e quaisquer limites não funcionais impostos por essa escolha | Operacionalização do REQ-DR-004 | Design de integração posterior; não especificado aqui |
| MR-009 | Restrições de performance, confiabilidade, segurança, deploy e compatibilidade | Nenhum requisito de NFR abaixo | Operação segura em produção |
| MR-010 | Quais eventos do ciclo de vida do pedido atualmente disparam o e-mail (inventário do estado atual) | Esclarece as opções do MR-001 / MR-004 | Análise de lacunas em relação ao conjunto desejado para o WhatsApp |

---

## Requisitos por Tipo (checagem de cobertura)

| Tipo | Presente? | Notas |
|------|----------|-------|
| Funcional | Sim | REQ-EX-001, REQ-EX-002, REQ-DR-001 |
| Regra de Negócio | Sim | REQ-DR-003, REQ-DR-005 (vínculo de política a definir) |
| Integração | Sim | REQ-DR-004 |
| Dados | Sim | REQ-DR-002 |
| Confiabilidade | Não | Não declarado; ver MR-009 |
| Performance | Não | Não declarado; ver MR-009 |
| Segurança | Não | Consentimento/regulatório não definido; ver MR-005, MR-009 |
| Observabilidade | Sim | REQ-DR-006 |
| Operacional | Não | Não declarado; ver MR-009 |
| Deploy | Não | Não declarado; ver MR-009 |
| Compatibilidade | Não | Não declarado; ver MR-009 |

---

## Avaliação Final

### Requisitos prontos para implementação

Nenhum está totalmente pronto para implementação.

REQ-EX-001, REQ-EX-002 e REQ-DR-001–REQ-DR-006 estão direcionalmente acordados como obrigações, mas cada um depende de um ou mais Requisitos Faltantes para uma especificação completa e testável. Eles podem ser usados para planejamento e workshops de esclarecimento, não para entrega pronta para construção.

### Requisitos que exigem esclarecimento

| Requisito | Esclarecimento necessário |
|-------------|----------------------|
| REQ-EX-001 | MR-001, MR-002, MR-003, MR-004 |
| REQ-EX-002 | MR-001, MR-002, MR-006 |
| REQ-DR-001 | MR-001, MR-003 |
| REQ-DR-002 | MR-007 |
| REQ-DR-003 | MR-002 |
| REQ-DR-004 | MR-008 (provedor), MR-005 (uso permitido) |
| REQ-DR-005 | MR-004 |
| REQ-DR-006 | Detalhes do sinal observável de sucesso/falha (forma operacional a definir; nenhuma implementação prescrita) |

Além disso, o MR-005 e o MR-009 precisam ser respondidos antes que os requisitos de Segurança, Confiabilidade, Performance, Operacional, Deploy e Compatibilidade possam ser escritos sem inventar valores.

### Requisitos que conflitam

Não existem conflitos diretos entre os requisitos declarados (apenas uma capacidade concreta foi solicitada: enviar atualizações de pedidos via WhatsApp).

**Tensão (não é um conflito formal):** A linguagem da Phase A/B descreve "adicionar" o WhatsApp como um canal, o que sugere complementação, enquanto a Ambiguidade #4 da Phase B ainda permite substituir / duplicar / complementar / alternativa de opt-in. Até que o MR-004 seja resolvido, o REQ-DR-005 não pode escolher um único comportamento; as implementações não devem presumir uma interpretação.

---

Nenhum diagrama PlantUML foi produzido para esta fase; o conjunto de requisitos é expresso como registros estruturados. Um diagrama não acrescentaria detalhe de obrigação testável além do que já está listado acima.
