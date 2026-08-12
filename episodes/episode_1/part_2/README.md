# Episódio 1 • Parte 2 — Descobrindo o Sistema Existente

Esta pasta contém o resultado de apenas uma etapa do detalhamento de atividade compartilhado para:

**Episódio 1 • Parte 2 — Descobrindo o Sistema Existente**

## Sobre o que é esta parte

A Parte 1 mostrou que a solicitação do WhatsApp era clara como uma reclamação de negócio — e incompleta como um conjunto de requisitos pronto para ser construído.

A Parte 2 faz uma pergunta diferente antes de desenhar uma solução:

> Como podemos mudar um sistema que não entendemos completamente?

Antes da arquitetura-alvo ou dos fluxos, o time:

1. Definiu os limites de escopo e as responsabilidades dos componentes
2. Inventariou todos os sistemas envolvidos — verificados, referenciados mas indefinidos, e apenas potencialmente necessários

**Conclusão principal:** apenas três componentes estão verificados para esta tarefa (Pedidos, Notificações por E-mail, WhatsApp). Várias capacidades necessárias permanecem sem nome, e nenhum banco de dados, queue ou API foi especificado — portanto, não devem ser inventados.

## Fases compartilhadas nesta parte

Apenas os resultados necessários para a Parte 2 são publicados aqui:

| Fase | Nome | Por que é compartilhada |
|-------|------|------------------|
| **E** | Inventário de Sistemas e Integrações | Sistemas verificados vs. indefinidos vs. potencialmente necessários, além do mapa de dependências |


## Resultados nesta pasta

| Ordem | Arquivo | Propósito |
|-------|------|---------|
| 1 | [system_inventory.md](./02-phase-e-system-inventory.md) | Inventário completo: componentes verificados, referenciados mas indefinidos, lacunas potencialmente necessárias, mapa de dependências em texto |

## Como ler

Se você quiser o resultado direto primeiro, abra a fase E e vá para:

* **Componentes Verificados** — os únicos sistemas evidenciados para esta tarefa
* **Componentes Potencialmente Necessários** — capacidades implícitas pelos requisitos, mas ainda sem nome
* **Avaliação Final** — por que o inventário é honesto, mas ainda não está pronto para construção

## Pergunta de engenharia

> Como podemos mudar um sistema que não entendemos completamente?

## O que esta parte não inclui

* Diagramas de arquitetura-alvo
* Desenhos de fluxo de execução
* Esquemas de contrato de dados/integração
* Plano de implementação

Isso vem em partes posteriores, depois que os limites e o inventário estiverem claros.

## Próximo

Episódio 1 • Parte 3 — Desenhando a Solução
