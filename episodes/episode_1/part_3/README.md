# Episódio 1 • Parte 3 — Desenhando a Solução

Esta pasta contém os resultados de detalhamento de atividade compartilhados para:

**Episódio 1 • Parte 3 — Desenhando a Solução**

## Sobre o que é esta parte

A Parte 2 estabeleceu o que existe, o que está fora do escopo, e quais lacunas de responsabilidade bloqueavam o design.

Uma reunião de esclarecimento então fixou as regras para a v1:

* WhatsApp **complementa** o e-mail
* destinatário = cliente final do pedido
* momentos de disparo do fluxo de Whatsapp do MVP = os mesmos que já disparam o e-mail
* Time de Notificações é responsável pelo canal WhatsApp; Pedidos continua sendo a fonte da verdade

A Parte 3 responde:

> Onde essa nova responsabilidade deve viver?

Antes do código, o time:

1. Desenhou a arquitetura-alvo
2. Definiu os fluxos de execução que precisam funcionar
3. Especificou os contratos de integração — incluindo desconhecidos explícitos

**Conclusão principal:** o WhatsApp vive atrás de um **Adaptador de Canal** de responsabilidade do time de Notificações, não dentro de Pedidos. O e-mail permanece inalterado. Fluxos e contratos tornam visíveis, antes da implementação, o sucesso, os casos não elegíveis, a falha, o retry e a idempotência.

## Phases compartilhadas nesta parte

Apenas os resultados necessários para a Parte 3 são publicados aqui:

| Phase | Nome | Por que é compartilhada |
|-------|------|------------------|
| **G** | Desenho do Fluxo-Alvo | Fluxos de sucesso, não elegível, falha, retry, idempotência, concorrente e ponta a ponta |
| **H** | Contratos de Dados e Integração | CTR-001–CTR-005 entre Pedidos, E-mail, Adaptador, Gestão de Clientes, Provedor, e observabilidade |


## Artefatos nesta pasta

| Ordem | Arquivo | Propósito |
|-------|------|---------|
| 1 | [execution_flows.md](./execution_flows.md) | Fluxos lógicos, resultados (`SEND_*` / não elegível), métodos de validação |
| 2 | [contracts.md](./contracts.md) | Contratos produtor/consumidor, esquemas, erros, semântica de entrega |

## Como ler

Caminho sugerido para uma revisão de tech lead:

1. [fase F omitida aqui] Diagrama de arquitetura-alvo (atual vs. alvo)
2. Fluxos de caminho feliz + falha + não elegível
3. Mapa de contratos, depois CTR-002 / CTR-004 / CTR-005 no documento de contratos

Se você quiser o resultado direto primeiro:

* Phase G — vocabulário de resultados (`SEND_SUCCESS`, `SEND_FAILURE`, não elegível)
* Phase H — **Desconhecidos que bloqueiam o congelamento em nível de protocolo** (protocolos e tipos de campo não inventados)

## Pergunta de engenharia

> Onde essa nova responsabilidade deve viver?

## Próximo

Episódio 1 • Parte 4 — Preparando-se para a Falha
