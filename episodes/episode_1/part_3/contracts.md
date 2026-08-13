# Phase H — Contratos de Dados e Integração

Esta fase define **contratos lógicos** entre os sistemas na arquitetura-alvo. Formatos de wire, endpoints concretos e tipos de campo que nunca foram especificados são marcados como **Desconhecido**. Nenhum contrato de queue/broker é inventado.

**Mapa de contratos:** `phase_H_contract_map.puml`

| ID | Contrato | Produtor → Consumidor |
|----|----------|---------------------|
| CTR-001 | Gatilho de Notificação de Pedido (caminho de e-mail) | Pedidos → Notificações por E-mail |
| CTR-002 | Gatilho de Notificação de Pedido (caminho do WhatsApp) | Pedidos → Adaptador de Canal do WhatsApp |
| CTR-003 | Leitura de Endereçamento do Destinatário | Adaptador de Canal do WhatsApp → Gestão de Clientes |
| CTR-004 | Submissão ao WhatsApp | Adaptador de Canal do WhatsApp → API do Provedor de WhatsApp |
| CTR-005 | Sinal de Resultado do Envio | Adaptador de Canal do WhatsApp → Consumidor de observabilidade (operações/validação) |

CTR-001 e CTR-002 compartilham o **mesmo conteúdo lógico de gatilho** (workshop: mesmos momentos do e-mail). Eles podem ou não compartilhar um canal físico — **mecanismo físico de fan-out = Desconhecido**.

---

## CTR-001 — Gatilho de Notificação de Pedido (caminho de e-mail)

### Produtor
Pedidos

### Consumidor
Notificações por E-mail

### Protocolo
**Desconhecido** (caminho existente; não especificado na Phase F/G)

### Endpoint ou Canal
**Desconhecido** (existente; não inventariado)

### Esquema de Requisição/Mensagem

| Campo | Tipo | Obrigatório/Opcional | Significado | Validação |
|-------|------|-------------------|---------|------------|
| `order_identity` | **Desconhecido** | Obrigatório | Identifica o pedido ao qual esta notificação se refere | Deve estar presente e referenciar um contexto de pedido existente (formato exato desconhecido) |
| `update_meaning` | **Desconhecido** | Obrigatório | Identifica qual atualização de pedido / momento de notificação ocorreu | Deve ser interpretável como um momento de gatilho de e-mail quando o e-mail é enviado (inventário concreto = MR-010, lista desconhecida) |
| Campos adicionais específicos do e-mail | **Desconhecido** | **Desconhecido** | O caminho de e-mail existente pode carregar mais campos | **Desconhecido** — não inventar |

### Esquema de Resposta/Resultado

| Campo | Tipo | Significado |
|-------|------|---------|
| Resultado do envio de e-mail | **Desconhecido** | Hoje sob responsabilidade de Notificações por E-mail; **redefinir está fora do escopo** |
| *(nenhum exigido pelo Episódio 1 para o WhatsApp)* | — | O WhatsApp não deve ler o resultado do e-mail como seu próprio status (Phase F AD-006) |

### Contrato de Erro

| Aspecto | Definição |
|--------|------------|
| Formato de erro | **Desconhecido** (existente) |
| Status | **Desconhecido** (existente) |
| Erros passíveis de retry | **Desconhecido** (existente) |
| Erros não passíveis de retry | **Desconhecido** (existente) |

O Episódio 1 **não** redesenha este contrato de erro.

### Semântica de Entrega

| Aspecto | Definição |
|--------|------------|
| Ordenação | **Desconhecido** (existente) |
| Duplicação | **Desconhecido** (existente) |
| Idempotência | **Desconhecido** (existente) |
| Retry | **Desconhecido** (existente) |
| Timeout | **Desconhecido** (existente) |
| Comportamento de dead-letter | **Desconhecido** / não especificado |
| Compatibilidade de versão | Tratar como contrato existente; o Episódio 1 deve permanecer compatível com o comportamento atual do gatilho de e-mail |

### Compatibilidade

| Aspecto | Definição |
|--------|------------|
| Backward compatibility | **Obrigatória** — o caminho de e-mail deve continuar funcionando sem alterações (política de complemento) |
| Versionamento | **Desconhecido** para o identificador de versão do contrato existente |
| Compatibilidade de rollout | O novo caminho do WhatsApp não deve exigir a quebra do CTR-001 |

---

## CTR-002 — Gatilho de Notificação de Pedido (caminho do WhatsApp)

### Produtor
Pedidos

### Consumidor
Adaptador de Canal do WhatsApp

### Protocolo
**Desconhecido** (não selecionado na Phase F/G)

### Endpoint ou Canal
**Desconhecido** (canal lógico: "os mesmos momentos de atualização de pedido dentro do escopo do e-mail")

### Esquema de Requisição/Mensagem

| Campo | Tipo | Obrigatório/Opcional | Significado | Validação |
|-------|------|-------------------|---------|------------|
| `order_identity` | **Desconhecido** | Obrigatório | Pedido associado à notificação do WhatsApp (REQ-DR-002) | Deve estar presente; deve corresponder à identidade do pedido usada na intenção paralela de e-mail quando o momento é compartilhado |
| `update_meaning` | **Desconhecido** | Obrigatório | Qual atualização de pedido / momento de notificação é este (REQ-DR-001) | Junto com `order_identity`, forma a chave de idempotência lógica (Phase G FLOW-ID); codificação concreta desconhecida |
| `end_customer_reference` | **Desconhecido** | Obrigatório para a resolução de endereçamento | Referência necessária para que o Adaptador possa consultar a Gestão de Clientes sobre o cliente final associado ao pedido | Deve permitir a busca na Gestão de Clientes; nome/tipo exato do campo desconhecido |
| Outros campos do payload | **Desconhecido** | Opcional / Desconhecido | Qualquer contexto adicional além da associação ao pedido | **Não inventar**; campos do corpo da mensagem para o conteúdo do WhatsApp = MR-007 |

### Esquema de Resposta/Resultado

| Campo | Tipo | Significado |
|-------|------|---------|
| Aceitação do gatilho pelo Adaptador | Tipo de wire **desconhecido** | Confirmação lógica de que o Adaptador recebeu o gatilho (nível de transporte) — distinto do resultado `SEND_*` do WhatsApp |
| Resultado do envio via WhatsApp | ver **CTR-005** | Não necessariamente retornado de forma síncrona neste contrato (síncrono vs. assíncrono = **Desconhecido**) |

### Contrato de Erro

| Aspecto | Definição |
|--------|------------|
| Formato de erro | **Desconhecido** |
| Status | Classes lógicas: gatilho rejeitado (campos lógicos obrigatórios malformados/ausentes) vs. aceito para processamento |
| Erros passíveis de retry | Falhas de transporte/disponibilidade ao entregar o gatilho ao Adaptador — códigos concretos **desconhecidos**; tratar como passível de retry na camada de transporte quando aplicável |
| Erros não passíveis de retry | Campos lógicos obrigatórios ausentes (`order_identity`, `update_meaning`, ou `end_customer_reference` inutilizável) — o Adaptador não deve inventar valores |

### Semântica de Entrega

| Aspecto | Definição |
|--------|------------|
| Ordenação | **Desconhecida** entre atualizações diferentes; nenhum requisito de ordenação global declarado |
| Duplicação | Possível (Phase G FLOW-ID) — o consumidor deve tolerar duplicatas |
| Idempotência | Responsabilidade do consumidor para envios bem-sucedidos: chave = `order_identity` + `update_meaning` (lógica); formato de wire da chave **desconhecido** |
| Retry | O produtor/transporte pode fazer retry; o Adaptador deve permanecer idempotente em caso de sucesso |
| Timeout | **Desconhecido** |
| Comportamento de dead-letter | **Desconhecido** (nenhuma DLQ inventada) |
| Compatibilidade de versão | Contrato novo; deve permanecer alinhado ao conjunto de momentos do CTR-001 (regra do MVP) |

### Compatibilidade

| Aspecto | Definição |
|--------|------------|
| Backward compatibility | N/A por ser totalmente novo; não deve quebrar o CTR-001 |
| Versionamento | Identificador de versão do contrato = **Desconhecido** (a ser atribuído quando o protocolo for escolhido) |
| Compatibilidade de rollout | O Adaptador pode entrar em rollout antes que o provedor esteja definido apenas se as submissões forem controladas por gate (MR-008/MR-005); o contrato de gatilho deve permitir que o Adaptador registre resultados sem submissão |

---

## CTR-003 — Leitura de Endereçamento do Destinatário

### Produtor
Adaptador de Canal do WhatsApp *(chamador / solicitante)*

### Consumidor
Gestão de Clientes *(sistema de registro que responde pelo contato do cliente)*

*Nota: em termos de requisição/resposta, o Adaptador é o cliente e a Gestão de Clientes é o servidor. A responsabilidade pelos dados de endereço permanece com a Gestão de Clientes (Phase F AD-004).*

### Protocolo
**Desconhecido**

### Endpoint ou Canal
**Desconhecido** (lógico: ler o endereçamento de WhatsApp do cliente final associado ao pedido)

### Esquema de Requisição/Mensagem

| Campo | Tipo | Obrigatório/Opcional | Significado | Validação |
|-------|------|-------------------|---------|------------|
| `end_customer_reference` | **Desconhecido** | Obrigatório | Identifica o cliente final associado ao pedido | Deve estar presente; formato desconhecido |
| Capacidade solicitada | Lógico | Obrigatório | Solicitar endereçamento compatível com WhatsApp para esse cliente final | Não deve solicitar funcionalidades de redesenho da Gestão de Clientes |

### Esquema de Resposta/Resultado

| Campo | Tipo | Significado |
|-------|------|---------|
| `addressing_available` | Booleano lógico (tipo de wire **desconhecido**) | Se existe endereçamento de WhatsApp utilizável |
| `whatsapp_addressing` | **Desconhecido** | Valor(es) de endereçamento necessários para submeter ao provedor quando disponível — **campos exatos desconhecidos** (telefone/ID do WA/etc. não especificados) |
| `customer_identity` (se retornado) | **Desconhecido** | Correlação opcional de volta ao cliente final — **desconhecido** se é retornado |

Quando `addressing_available` for falso, o Adaptador deve emitir `NOT_ELIGIBLE_RECIPIENT` (Phase G) e **não** deve chamar o CTR-004.

### Contrato de Erro

| Aspecto | Definição |
|--------|------------|
| Formato de erro | **Desconhecido** |
| Status | Lógico: sucesso com endereçamento; sucesso sem endereçamento; falha de leitura na Gestão de Clientes |
| Erros passíveis de retry | Indisponibilidade transitória da Gestão de Clientes / timeout — códigos **desconhecidos**; passível de retry na camada de leitura |
| Erros não passíveis de retry | `end_customer_reference` inválido; endereçamento de WhatsApp permanentemente ausente → mapear para `NOT_ELIGIBLE_RECIPIENT`, não `SEND_FAILURE` |

### Semântica de Entrega

| Aspecto | Definição |
|--------|------------|
| Ordenação | N/A (leitura de requisição/resposta) |
| Duplicação | Leituras podem se repetir; devem ser seguras |
| Idempotência | A leitura é idempotente |
| Retry | Permitido em falhas transitórias da Gestão de Clientes |
| Timeout | **Desconhecido** |
| Comportamento de dead-letter | N/A |
| Compatibilidade de versão | Somente leitura; evitar exigir redesenho do esquema da Gestão de Clientes |

### Compatibilidade

| Aspecto | Definição |
|--------|------------|
| Backward compatibility | Deve funcionar com os dados existentes da Gestão de Clientes como estão; nenhum redesenho de funcionalidade da Gestão de Clientes (Phase F) |
| Versionamento | **Desconhecido** |
| Compatibilidade de rollout | Se o campo de endereçamento estiver ausente na Gestão de Clientes hoje, os resultados são `NOT_ELIGIBLE_RECIPIENT` até que os dados existam — não inventar campos da Gestão de Clientes neste contrato |

---

## CTR-004 — Submissão ao WhatsApp

### Produtor
Adaptador de Canal do WhatsApp

### Consumidor
API do Provedor de WhatsApp (fornecedor **a definir** — MR-008)

### Protocolo
**Desconhecido** (depende do provedor selecionado)

### Endpoint ou Canal
**Desconhecido** (específico do provedor; abstração de "submeter mensagem para entrega")

### Esquema de Requisição/Mensagem

| Campo | Tipo | Obrigatório/Opcional | Significado | Validação |
|-------|------|-------------------|---------|------------|
| `whatsapp_addressing` | **Desconhecido** | Obrigatório | Endereçamento de destino vindo do CTR-003 | Deve estar presente; validação específica do provedor **desconhecida** até o MR-008 |
| `order_identity` | **Desconhecido** | Obrigatório | Associa a mensagem ao pedido (REQ-DR-002) | Deve estar presente na intenção de responsabilidade do Adaptador; se é enviado ao provedor ou apenas armazenado internamente = **desconhecido** |
| `update_meaning` | **Desconhecido** | Obrigatório | Transmite que uma atualização de pedido ocorreu (REQ-DR-002) | Campos exatos de conteúdo visíveis ao cliente = **desconhecidos** (MR-007) |
| Campos de corpo/template visíveis ao cliente | **Desconhecido** | **Desconhecido** / exigido pelo provedor a definir | Conteúdo da mensagem além da associação ao pedido | **Não inventar**; bloqueado pelo MR-007 + MR-008 |
| Token de idempotência / deduplicação do provedor | **Desconhecido** | Opcional / Desconhecido | Pode ser exigido pelo provedor | **Desconhecido** até o MR-008 |

### Esquema de Resposta/Resultado

| Campo | Tipo | Significado |
|-------|------|---------|
| `submit_result` | Mapeamento de enum lógico | `accepted` → `SEND_SUCCESS` do Adaptador; `rejected`/`failed`/`timeout` → `SEND_FAILURE` do Adaptador |
| ID de correlação do provedor | **Desconhecido** | Referência opcional do provedor para suporte — **desconhecido** se está disponível |
| Detalhe de erro do provedor | **Desconhecido** | Usado para classificar passível de retry ou não — códigos **desconhecidos** até o MR-008 |

### Contrato de Erro

| Aspecto | Definição |
|--------|------------|
| Formato de erro | **Desconhecido** (específico do provedor) |
| Status | Apenas mapeamento lógico do Adaptador: aceite com sucesso vs. falha (Phase G) |
| Erros passíveis de retry | Falhas transitórias do provedor / timeouts — elegíveis para o FLOW-RR; **lista concreta desconhecida** até o MR-008 |
| Erros não passíveis de retry | Rejeições permanentes (endereçamento inválido, negação por política, etc.) — lista **desconhecida** até o MR-008; não deve tentar retry infinito |

### Semântica de Entrega

| Aspecto | Definição |
|--------|------------|
| Ordenação | Nenhum requisito de ordenação entre pedidos declarado |
| Duplicação | O provedor pode receber duplicatas se o Adaptador fizer retry; preferir idempotência do lado do provedor ou do Adaptador (FLOW-ID) |
| Idempotência | Exigida no Adaptador para `(order_identity, update_meaning)` após `SEND_SUCCESS`; idempotência nativa do provedor = **desconhecida** |
| Retry | Permitido para falhas passíveis de retry (FLOW-RR); limites/backoff = **desconhecidos** (MR-008/MR-009) |
| Timeout | **Desconhecido** |
| Comportamento de dead-letter | **Desconhecido** (não projetado); `SEND_FAILURE` terminal após a recuperação se esgotar é um tópico de política da Phase J |
| Compatibilidade de versão | Isolar o provedor atrás do Adaptador (Phase F AD-007) para que mudanças de versão do SDK/API do provedor não vazem para Pedidos |

### Compatibilidade

| Aspecto | Definição |
|--------|------------|
| Backward compatibility | N/A até que o primeiro provedor seja escolhido |
| Versionamento | O Adaptador deve versionar sua integração com o provedor; Pedidos não deve depender da versão do provedor |
| Compatibilidade de rollout | O Episódio 2 (múltiplos provedores) implica que o CTR-004 deve permanecer substituível por provedor sem alterar o CTR-002 |

---

## CTR-005 — Sinal de Resultado do Envio

### Produtor
Adaptador de Canal do WhatsApp

### Consumidor
Consumidores de observabilidade / validação (operadores, validação de negócio do REQ-DR-006). **Sistema de destino concreto = Desconhecido** (nenhum produto de monitoramento inventado).

### Protocolo
**Desconhecido**

### Endpoint ou Canal
**Desconhecido** (lógico: resultado de envio distinguível para uma intenção de atualização de pedido via WhatsApp)

### Esquema de Requisição/Mensagem

| Campo | Tipo | Obrigatório/Opcional | Significado | Validação |
|-------|------|-------------------|---------|------------|
| `order_identity` | **Desconhecido** | Obrigatório | Pedido ao qual o resultado se refere | Deve estar presente |
| `update_meaning` | **Desconhecido** | Obrigatório | Atualização/momento ao qual o resultado se refere | Deve estar presente |
| `outcome` | Enum lógico | Obrigatório | Um entre: `SEND_SUCCESS`, `SEND_FAILURE`, `NOT_ELIGIBLE_MOMENT`, `NOT_ELIGIBLE_RECIPIENT` (Phase G) | Deve ser exatamente um resultado por sinal registrado; não deve ser igual ao status do e-mail |
| `recorded_at` | **Desconhecido** | Opcional / Desconhecido | Quando o resultado foi registrado | Precisão/formato **desconhecidos** |
| ID de correlação do provedor | **Desconhecido** | Opcional | Vínculo com a resposta do CTR-004 quando presente | **Desconhecido** |

### Esquema de Resposta/Resultado

| Campo | Tipo | Significado |
|-------|------|---------|
| Confirmação do consumidor | **Desconhecido** | Opcional; não exigida para satisfazer o REQ-DR-006 se o resultado for consultável/armazenado — mecanismo de armazenamento **desconhecido** |

### Contrato de Erro

| Aspecto | Definição |
|--------|------------|
| Formato de erro | **Desconhecido** |
| Status | A falha ao emitir/armazenar o resultado é um defeito operacional em relação ao REQ-DR-006 |
| Erros passíveis de retry | Falhas transitórias do destino — **desconhecido** |
| Erros não passíveis de retry | Payload de resultado inválido (pedido/atualização/resultado ausentes) |

### Semântica de Entrega

| Aspecto | Definição |
|--------|------------|
| Ordenação | Resultados de intenções diferentes não ordenados, a menos que especificado posteriormente |
| Duplicação | Sinais duplicados para a mesma intenção são possíveis; os consumidores devem tratar o estado final por `(order_identity, update_meaning)` |
| Idempotência | O sucesso final deve prevalecer sobre uma falha anterior após o FLOW-RR (lógico) |
| Retry | Permitido para problemas de disponibilidade do destino |
| Timeout | **Desconhecido** |
| Comportamento de dead-letter | **Desconhecido** |
| Compatibilidade de versão | O enum de resultado faz parte do contrato do Episódio 1; adicionar valores depois deve ser backward compatible para os consumidores |

### Compatibilidade

| Aspecto | Definição |
|--------|------------|
| Backward compatibility | Novos consumidores devem aceitar os quatro resultados da Phase G |
| Versionamento | Extensões do enum exigem atualizações coordenadas dos consumidores ou leitores tolerantes |
| Compatibilidade de rollout | O REQ-DR-006 não é atendido se os resultados não forem distinguíveis no destino escolhido |

---

## Regras entre contratos

| Regra | Enunciado |
|------|-----------|
| CR-1 | A elegibilidade de momento do CTR-002 deve corresponder aos momentos de gatilho de e-mail do CTR-001 (MVP) |
| CR-2 | O resultado do e-mail (CTR-001) nunca deve ser copiado como `outcome` do WhatsApp (CTR-005) |
| CR-3 | O CTR-004 não deve ser chamado quando o CTR-003 não retornar endereçamento utilizável |
| CR-4 | Chave de idempotência lógica para a intenção do WhatsApp = `order_identity` + `update_meaning` |
| CR-5 | Campos específicos do provedor ficam dentro do CTR-004; Pedidos só fala CTR-001/CTR-002 |
| CR-6 | O gate de consentimento/envio permitido (MR-005) **ainda não** é um contrato totalmente especificado — a submissão em produção exige isso |

---

## Desconhecidos que bloqueiam o congelamento em nível de protocolo (wire-level)

| Desconhecido | Bloqueia |
|---------|--------|
| Protocolo / endpoint para CTR-001–CTR-005 | Bindings de implementação |
| Tipos concretos para `order_identity`, `update_meaning`, `end_customer_reference` | Congelamento do schema |
| Conjunto de campos de endereçamento do WhatsApp | CTR-003/CTR-004 |
| Conteúdo da mensagem visível ao cliente (MR-007) | Corpo do CTR-004 |
| API do provedor + códigos de erro (MR-008) | Matriz de erro/retry do CTR-004 |
| Sistema de destino do resultado | Contrato físico do CTR-005 |
| Síncrono vs. assíncrono CTR-002 ↔ CTR-005 | Latência e experiência de falha para operadores |

---

## Avaliação Final

Os contratos lógicos **CTR-001–CTR-005** estão definidos o suficiente para guiar a Phase I/J e as discussões de design de interface.

Eles **não** são especificações de API implementáveis até que os Desconhecidos e os MR-005/007/008/010 sejam resolvidos.

**Diagrama:** `phase_H_contract_map.puml`

---

## Artefato Compartilhado

### Resumo dos contratos (Episódio 1)

| Contrato | De → Para | Deve carregar | Regra crítica |
|----------|-----------|------------|---------------|
| CTR-001 | Pedidos → Notificações por E-mail | identidade do pedido + significado da atualização (existente) | Manter inalterado |
| CTR-002 | Pedidos → Adaptador WhatsApp | mesmo gatilho lógico + referência do cliente final | Mesmos momentos do e-mail; idempotente em `(pedido, atualização)` |
| CTR-003 | Adaptador → Gestão de Clientes | referência do cliente final | Somente leitura; endereço ausente ≠ falha de envio |
| CTR-004 | Adaptador → Provedor de WhatsApp | endereçamento + conteúdo de atualização associado ao pedido | Provedor a definir; isolado atrás do Adaptador |
| CTR-005 | Adaptador → Observabilidade | resultados `SEND_SUCCESS` / `SEND_FAILURE` / não elegíveis | Nunca reutilizar o status do e-mail |

