# Phase G — Desenho do Fluxo-Alvo

Esta fase define **fluxos de execução lógicos** para o sistema-alvo. Ela não prescreve etapas de implementação, classes, queues ou bibliotecas de retry.

### Vocabulário de resultados lógicos (Adaptador)

| Resultado | Significado |
|---------|---------|
| `SEND_SUCCESS` | Submissão do WhatsApp aceita para entrega (sucesso do REQ-DR-006) |
| `SEND_FAILURE` | Envio elegível tentado; a submissão ao provedor falhou (falha do REQ-DR-006) |
| `NOT_ELIGIBLE_MOMENT` | A atualização não é um momento equivalente a um gatilho de e-mail (regra do MVP) |
| `NOT_ELIGIBLE_RECIPIENT` | Momento dentro do escopo, mas o endereçamento do WhatsApp do cliente final está indisponível |

Os resultados de entrega do e-mail continuam sob responsabilidade das Notificações por E-mail e **nunca** são usados como status do WhatsApp (Phase F AD-006).

### Pontos de decisão compartilhados (caminho do WhatsApp)

1. Este momento é uma notificação de pedido **equivalente a um gatilho de e-mail**? (MVP do workshop / REQ-DR-001)
2. O **endereçamento do WhatsApp do cliente final** pode ser resolvido a partir da Gestão de Clientes? (REQ-DR-003)
3. *(Gate ainda em aberto)* O envio é **permitido** sob as regras de consentimento/regulatórias? (MR-005 — deve ser resolvido antes da produção; os fluxos abaixo presumem permissão quando ocorre uma submissão)
4. O **provedor aceitou** a submissão? (REQ-DR-004, REQ-DR-006)

---

## FLOW-HP — Caminho Feliz

**Diagrama:** `phase_G_happy_path.puml`

| Campo | Descrição |
|-------|-------------|
| **Gatilho** | Ocorre um momento do ciclo de vida do pedido dentro do escopo — a mesma classe de momento que já dispara uma notificação de pedido por e-mail |
| **Estado inicial** | O pedido existe; o caminho de e-mail opera como hoje; nenhum envio via WhatsApp ainda registrado para este pedido+atualização |
| **Pontos de decisão** | Momento elegível? → sim. Endereçamento resolvível? → sim. Provedor aceita? → sim |
| **Atores / componentes** | Pedidos; Notificações por E-mail; Adaptador de Canal do WhatsApp; Gestão de Clientes; API do Provedor de WhatsApp; Cliente Final |
| **Mensagens** | Atualização de pedido (id do pedido + significado da atualização) para Notificações por E-mail e para o Adaptador; leitura de endereçamento a partir da Gestão de Clientes; payload de submissão do WhatsApp associado ao pedido/atualização |
| **Chamadas externas** | Submissão à API do Provedor de WhatsApp |
| **Resultado de sucesso** | E-mail enviado (existente); WhatsApp aceito para entrega; Adaptador registra `SEND_SUCCESS` |
| **Estado final** | O cliente final pode receber a atualização de pedido por e-mail e WhatsApp; o sucesso do WhatsApp é observável de forma independente |

| Meta | Valor |
|------|-------|
| **Propósito de negócio** | Entregar a atualização de pedido em um canal com maior chance de ser percebido, mantendo o e-mail |
| **Requisitos afetados** | REQ-EX-001, REQ-EX-002, REQ-DR-001, REQ-DR-002, REQ-DR-003, REQ-DR-004, REQ-DR-005, REQ-DR-006 |
| **Estado final esperado** | Complemento de canal duplo concluído para este momento; `SEND_SUCCESS` visível |
| **Método de validação** | Disparar uma atualização dentro do escopo; confirmar que o e-mail ainda ocorre; confirmar que a submissão do WhatsApp foi aceita; confirmar que o resultado do Adaptador é `SEND_SUCCESS` e referencia o pedido/atualização corretos |

---

## FLOW-NE — Fluxo Desabilitado / Não Elegível

**Diagrama:** `phase_G_non_eligible.puml`

Dois casos não elegíveis estão dentro do escopo do desenho de fluxo:

### NE-A — Momento fora do escopo do MVP

| Campo | Descrição |
|-------|-------------|
| **Gatilho** | Atividade de pedido que **não** corresponde a um momento de notificação de pedido equivalente a um gatilho de e-mail |
| **Estado inicial** | O pedido pode mudar; nenhuma notificação por e-mail para esta atividade |
| **Pontos de decisão** | Equivalente a um gatilho de e-mail? → **não** |
| **Atores / componentes** | Pedidos; Adaptador de Canal do WhatsApp (Notificações por E-mail não envia para esta atividade) |
| **Mensagens** | Observação opcional da atividade do pedido; nenhuma mensagem de submissão ao WhatsApp |
| **Chamadas externas** | Nenhuma à API do Provedor de WhatsApp |
| **Resultado de sucesso** | Supressão correta: nenhum envio via WhatsApp |
| **Estado final** | `NOT_ELIGIBLE_MOMENT`; o cliente não é notificado via WhatsApp para esta atividade |

### NE-B — Momento dentro do escopo, destinatário não endereçável

| Campo | Descrição |
|-------|-------------|
| **Gatilho** | Atualização de pedido dentro do escopo (o e-mail será enviado) |
| **Estado inicial** | Atualização de pedido elegível pelo momento; endereçamento do WhatsApp do cliente final ausente/inutilizável |
| **Pontos de decisão** | Momento elegível? → sim. Endereçamento resolvível? → **não** |
| **Atores / componentes** | Pedidos; Notificações por E-mail; Adaptador; Gestão de Clientes; Cliente Final (apenas e-mail) |
| **Mensagens** | Caminho de e-mail como hoje; a leitura da Gestão de Clientes não retorna endereçamento utilizável; nenhuma submissão ao provedor |
| **Chamadas externas** | Nenhuma à API do Provedor de WhatsApp |
| **Resultado de sucesso** | O e-mail pode ter sucesso; o WhatsApp é corretamente ignorado, com um resultado não elegível visível |
| **Estado final** | `NOT_ELIGIBLE_RECIPIENT` (não `SEND_FAILURE`) |

| Meta | Valor |
|------|-------|
| **Propósito de negócio** | Evitar envios de WhatsApp arbitrários ou não entregáveis; manter o MVP alinhado aos momentos de e-mail |
| **Requisitos afetados** | REQ-DR-001 (vinculado a momentos dentro do escopo), REQ-DR-003, REQ-DR-005 (e-mail ainda permitido), REQ-DR-006 (resultado distinguível de falha) |
| **Estado final esperado** | Nenhuma submissão ao provedor; resultado não elegível registrado; e-mail inalterado quando o momento está dentro do escopo |
| **Método de validação** | (A) Disparar uma atualização que não é gatilho de e-mail → verificar que não há submissão ao WhatsApp. (B) Disparar uma atualização dentro do escopo com endereçamento ausente → verificar que o e-mail pode ser enviado, que o resultado do WhatsApp é `NOT_ELIGIBLE_RECIPIENT`, não `SEND_FAILURE` |

---

## FLOW-FL — Fluxo de Falha

**Diagrama:** `phase_G_failure.puml`

| Campo | Descrição |
|-------|-------------|
| **Gatilho** | Atualização de pedido dentro do escopo; endereçamento disponível; a submissão ao provedor falha (rejeição, timeout, ou equivalente) |
| **Estado inicial** | Envio elegível via WhatsApp prestes a ser tentado; caminho de e-mail independente |
| **Pontos de decisão** | Momento elegível? → sim. Endereçamento? → sim. Provedor aceita? → **não** |
| **Atores / componentes** | Pedidos; Notificações por E-mail; Adaptador; Gestão de Clientes; API do Provedor de WhatsApp; Cliente Final |
| **Mensagens** | Mesmas do caminho feliz até que a resposta do provedor indique falha |
| **Chamadas externas** | Submissão à API do Provedor de WhatsApp → resultado de falha |
| **Resultado de sucesso** | *(sucesso do fluxo = tratamento correto da falha)* O Adaptador registra `SEND_FAILURE`; Pedidos não é afetado; o status do e-mail não é reescrito |
| **Estado final** | Alcance parcial possível (e-mail ok / WhatsApp falhou); a falha é distinguível do sucesso e do não elegível |

| Meta | Valor |
|------|-------|
| **Propósito de negócio** | Tornar visível o alcance falho do WhatsApp sem quebrar o processamento de pedidos nem falsificar o status do e-mail |
| **Requisitos afetados** | REQ-EX-001 (tentativa), REQ-DR-004, REQ-DR-006; REQ-DR-005 (o e-mail ainda pode ser concluído) |
| **Estado final esperado** | `SEND_FAILURE` visível; resultado do e-mail independente |
| **Método de validação** | Simular falha do provedor em um envio elegível; verificar `SEND_FAILURE`; verificar que o e-mail ainda pode ter sucesso; verificar que a atualização de negócio de Pedidos não sofre rollback por causa da falha do WhatsApp |

---

## FLOW-RR — Fluxo de Retry / Recuperação

**Aplicável:** sim — falhas do provedor podem ser transitórias; a recuperação é necessária para eventualmente satisfazer o REQ-EX-001 quando o canal estiver saudável novamente, sem confundir os resultados.

**Diagrama:** `phase_G_retry_recovery.puml`

| Campo | Descrição |
|-------|-------------|
| **Gatilho** | Uma submissão elegível anterior terminou em `SEND_FAILURE` passível de retry para o pedido+atualização `(O, U)` |
| **Estado inicial** | `SEND_FAILURE` registrado; mensagem ainda não aceita para entrega |
| **Pontos de decisão** | Ainda elegível? → sim. Retry/recuperação permitido para esta classe de falha? → sim (limites a definir com MR-008/MR-009). Provedor aceita em tentativa posterior? → sim |
| **Atores / componentes** | Adaptador de Canal do WhatsApp; API do Provedor de WhatsApp; Cliente Final |
| **Mensagens** | Nova submissão da mesma intenção de notificação associada ao pedido `(O, U)` |
| **Chamadas externas** | Uma ou mais submissões subsequentes à API do Provedor de WhatsApp |
| **Resultado de sucesso** | Uma tentativa posterior é aceita; o Adaptador registra o `SEND_SUCCESS` final |
| **Estado final** | O cliente pode receber o WhatsApp para `(O, U)`; o resultado final é sucesso, não uma falha travada sem caminho de recuperação |

| Meta | Valor |
|------|-------|
| **Propósito de negócio** | Recuperar-se de falhas transitórias de canal para que atualizações elegíveis ainda alcancem o WhatsApp |
| **Requisitos afetados** | REQ-EX-001, REQ-DR-002 (mesmo pedido/atualização), REQ-DR-004, REQ-DR-006 |
| **Estado final esperado** | `SEND_SUCCESS` terminal após a recuperação (ou falha terminal se a recuperação se esgotar — detalhe de política na Phase J) |
| **Método de validação** | Fazer a primeira submissão falhar, a posterior ter sucesso para o mesmo `(O, U)`; verificar `SEND_SUCCESS` final e uma única intenção lógica de notificação |

---

## FLOW-ID — Fluxo de Duplicata / Idempotência

**Aplicável:** sim — a mesma intenção de notificação de pedido+atualização pode ser entregue ao Adaptador mais de uma vez.

**Diagrama:** `phase_G_idempotency.puml`

| Campo | Descrição |
|-------|-------------|
| **Gatilho** | Segunda entrega da mesma intenção de notificação dentro do escopo para `(pedido O, atualização U)` após uma submissão bem-sucedida |
| **Estado inicial** | `SEND_SUCCESS` já registrado para `(O, U)`; o cliente já foi alcançado uma vez |
| **Pontos de decisão** | Já foi submetido com sucesso para `(O, U)`? → **sim** → suprimir a submissão duplicada ao provedor |
| **Atores / componentes** | Pedidos; Adaptador de Canal do WhatsApp; API do Provedor de WhatsApp; Cliente Final |
| **Mensagens** | Gatilho duplicado; nenhum segundo payload de submissão ao provedor |
| **Chamadas externas** | Nenhuma na duplicata após o sucesso |
| **Resultado de sucesso** | Nenhuma segunda mensagem de WhatsApp ao cliente para a mesma intenção |
| **Estado final** | Ainda um único `SEND_SUCCESS` lógico para `(O, U)` |

| Meta | Valor |
|------|-------|
| **Propósito de negócio** | Evitar ruído duplicado de WhatsApp para uma atualização de pedido |
| **Requisitos afetados** | REQ-EX-001/002 (qualidade do "envio"), REQ-DR-002 (uma associação por intenção de atualização) |
| **Estado final esperado** | Sucesso idempotente; uma mensagem de WhatsApp voltada ao cliente por intenção `(O, U)` |
| **Método de validação** | Entregar o mesmo `(O, U)` duas vezes após o sucesso; verificar uma única aceitação do provedor para essa intenção |

**Chave de idempotência lógica:** identidade do pedido + significado da atualização / momento da notificação (conjunto exato de campos adiado para a Phase H; não inventado como design de armazenamento aqui).

---

## FLOW-CC — Fluxo de Processamento Concorrente

**Aplicável:** sim — a política de complemento implica que os caminhos de e-mail e WhatsApp para o mesmo momento não podem depender da conclusão um do outro.

**Diagrama:** `phase_G_concurrent.puml`

| Campo | Descrição |
|-------|-------------|
| **Gatilho** | Uma única atualização de pedido dentro do escopo `(O, U)` notificada tanto para Notificações por E-mail quanto para o Adaptador de Canal do WhatsApp |
| **Estado inicial** | Ambos os caminhos prontos para processar o mesmo momento |
| **Pontos de decisão** | Cada caminho avalia seu próprio sucesso/falha; nenhum bloqueia o outro |
| **Atores / componentes** | Pedidos; Notificações por E-mail; Adaptador; Gestão de Clientes; Provedor; Cliente Final |
| **Mensagens** | Envio de e-mail em paralelo com o endereçamento+submissão do WhatsApp |
| **Chamadas externas** | Saída de e-mail (existente); API do Provedor de WhatsApp (nova) — sobrepostas no tempo |
| **Resultado de sucesso** | Ambos podem ser concluídos com sucesso sem espera mútua |
| **Estado final** | Resultados independentes possíveis (ambos com sucesso; ou misto — ver FLOW-FL) |

| Meta | Valor |
|------|-------|
| **Propósito de negócio** | Preservar a latência/comportamento do e-mail ao adicionar o WhatsApp; evitar acoplamento serial |
| **Requisitos afetados** | REQ-DR-005 (complemento), REQ-DR-006 (visibilidade independente do WhatsApp), Phase F R-004/R-007 |
| **Estado final esperado** | Os caminhos são concluídos de forma independente; os status não são confundidos |
| **Método de validação** | Verificar que o caminho do WhatsApp não bloqueia na conclusão do e-mail e vice-versa; injetar atraso/falha em um caminho e confirmar que o outro ainda consegue concluir |

---

## FLOW-E2E — Fluxo de Negócio Ponta a Ponta

**Diagrama:** `phase_G_end_to_end.puml`

| Campo | Descrição |
|-------|-------------|
| **Gatilho** | Momento de negócio do ciclo de vida do pedido que está dentro do escopo para notificações de pedido por e-mail |
| **Estado inicial** | O cliente tem um pedido; a atualização ainda não foi comunicada no WhatsApp |
| **Pontos de decisão** | Os pontos de decisão compartilhados 1–4 acima, todos aprovados no caminho feliz |
| **Atores / componentes** | Negócio/ciclo de vida do pedido; Pedidos; Notificações por E-mail; Adaptador de Canal do WhatsApp; Gestão de Clientes; API do Provedor de WhatsApp; Cliente Final |
| **Mensagens** | Identidade do pedido + significado da atualização; notificação por e-mail; resolução de endereçamento do WhatsApp; submissão ao WhatsApp; mensagens de e-mail e WhatsApp voltadas ao cliente |
| **Chamadas externas** | API do Provedor de WhatsApp (e o mecanismo de entrega de e-mail existente, não especificado) |
| **Resultado de sucesso** | O cliente recebe a atualização de pedido relevante por e-mail e WhatsApp; o Adaptador mostra `SEND_SUCCESS` |
| **Estado final** | Comunicação de canal duplo concluída para esse momento de negócio sob a política de complemento |

| Meta | Valor |
|------|-------|
| **Propósito de negócio** | Tornar as atualizações de pedido importantes mais fáceis de perceber, sem remover o e-mail |
| **Requisitos afetados** | REQ-EX-001, REQ-EX-002, REQ-DR-001–REQ-DR-006 |
| **Estado final esperado** | Complemento ponta a ponta satisfeito para um momento dentro do escopo |
| **Método de validação** | Teste de caminho completo: momento dentro do escopo → e-mail presente + WhatsApp aceito + associação de pedido correta + `SEND_SUCCESS` observável |

---

## Cobertura de fluxo ↔ requisito

| Requisito | Fluxos principais |
|-------------|----------------|
| REQ-EX-001 | HP, E2E, RR |
| REQ-EX-002 | HP, E2E |
| REQ-DR-001 | HP, NE-A, E2E |
| REQ-DR-002 | HP, ID, E2E |
| REQ-DR-003 | HP, NE-B |
| REQ-DR-004 | HP, FL, RR, E2E |
| REQ-DR-005 | HP, NE-B, FL, CC, E2E |
| REQ-DR-006 | HP, NE-*, FL, RR, CC |

---

## Itens em aberto adiados (não inventados como mecânica de fluxo)

| Item | Impacto nos fluxos |
|------|-----------------|
| MR-005 consentimento/envio permitido | Gate de produção antes da submissão; tratar como ponto de decisão obrigatório assim que for definido |
| MR-007 campos de conteúdo da mensagem | Afeta o formato do payload de submissão, não quais fluxos existem |
| MR-008 provedor + limites de retry | Afeta os limites do FLOW-RR e a classificação de falhas |
| MR-010 inventário de gatilhos de e-mail | Lista concreta de momentos para a classificação NE-A vs. HP |
| Política de negócio de falha parcial da Phase J | O que operadores/negócio fazem quando o e-mail está ok / o WhatsApp falhou |

---

## Avaliação Final

Os fluxos de execução estão **definidos em nível lógico** para os caminhos feliz, não elegível, falha, retry/recuperação, idempotência, canal duplo concorrente, e ponta a ponta.

**Pronto para:** contratos da Phase H (payload de gatilho, leitura de endereçamento, submissão ao provedor, sinais de resultado, campos da chave de idempotência).

**Não pronto para codificação de implementação** até que os MRs em aberto acima e a política de falha da Phase J fechem as lacunas restantes.

### Índice de diagramas

| Fluxo | Arquivo |
|------|------|
| Caminho feliz | `phase_G_happy_path.puml` |
| Não elegível | `phase_G_non_eligible.puml` |
| Falha | `phase_G_failure.puml` |
| Retry / recuperação | `phase_G_retry_recovery.puml` |
| Idempotência | `phase_G_idempotency.puml` |
| Concorrente | `phase_G_concurrent.puml` |
| Ponta a ponta | `phase_G_end_to_end.puml` |

---

## Artefato Compartilhado

### Resumo de fluxo (Episódio 1)

Para cada momento de notificação de pedido equivalente a um gatilho de e-mail, a FlowCommerce mantém o **caminho de e-mail existente** e executa um **caminho de WhatsApp paralelo** através do Adaptador de responsabilidade de Notificações: resolver o endereçamento do cliente final na Gestão de Clientes → submeter ao Provedor de WhatsApp → registrar `SEND_SUCCESS` ou `SEND_FAILURE`.

Se o momento não for equivalente a um gatilho de e-mail, ou o endereçamento estiver ausente, o Adaptador registra **não elegível** e não chama o provedor. Falhas do provedor não reescrevem o status do e-mail nem quebram Pedidos. Duplicatas da mesma intenção `(pedido, atualização)` não devem enviar em duplicidade. E-mail e WhatsApp devem poder ser concluídos de forma **concorrente e independente** sob a política de complemento.
