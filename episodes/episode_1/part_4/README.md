# Episódio 1 • Parte 4 — Preparando-se para a Falha

Esta pasta contém os artefatos de detalhamento de atividade compartilhados para:

**Episódio 1 • Parte 4 — Preparando-se para a Falha**

## Sobre o que é esta parte

A Parte 3 posicionou a nova responsabilidade corretamente: um **Adaptador de Canal do WhatsApp** de responsabilidade de Notificações, ao lado de Pedidos, com fluxos e contratos definidos em nível lógico.

A Parte 4 faz a pergunta que esses diagramas não respondem sozinhos:

> O que acontece quando as coisas não funcionam como esperado?

Antes do código, o time:

1. Modelou o ciclo de vida de uma intenção de notificação via WhatsApp, estado por estado
2. Definiu a política de falha e recuperação para cada categoria de falha que a arquitetura realmente pode produzir
3. Revisou onze características operacionais — latência, throughput, escalabilidade, disponibilidade, consistência, durabilidade, confiabilidade, custo, segurança, privacidade, operabilidade
4. Revisou preocupações de segurança e proteção de dados em todos os limites do sistema

**Conclusão principal:** o tratamento de falha, as metas operacionais e os limites de proteção de dados são definidos em **nível de política** — quais falhas são passíveis de retry vs. terminais, quem é responsável pela detecção vs. pela correção, quais dados podem cruzar quais limites. Metas numéricas, seleção de provedor e política de consentimento/regulatória permanecem em aberto e são rastreadas como itens explícitos de esclarecimento, não inventadas.

## Phases compartilhadas nesta parte

Apenas alguns dos resultados necessários para a Parte 4 são publicados aqui:

| Phase | Nome | Por que é compartilhada |
|-------|------|------------------|
| **I** | (não compartilhado) Modelagem de Estado e Ciclo de Vida | O ciclo de vida da Intenção de Notificação via WhatsApp, transições válidas/inválidas, regras de consistência de estado, e tratamento de duplicata/concorrência |
| **J** | Política de Falha e Recuperação | 13 categorias de falha mapeadas para componentes reais, regras de classificação retry-vs-terminal, e os fluxos de recuperação de crash/deploy |
| **K** | (não compartilhado) Conjunto de Requisitos Não Funcionais | Metas (ou "requer esclarecimento" explícito) nas onze dimensões operacionais, com um registro consolidado de esclarecimentos |
| **L** | (não compartilhado) Análise de Segurança e Proteção de Dados | Dados sensíveis/pessoais, limites de confiança, authN/authZ, credenciais, secrets, criptografia, isolamento de tenant, logging, exposição ao provedor, e retenção — além de questões de compliance em aberto |


## Artefatos nesta pasta

| Ordem | Arquivo | Propósito |
|-------|------|---------|
| 1 | [whatsapp-notification-intent.puml](./02-phase-i-whatsapp-notification-intent.puml) | PlantUML: máquina de estados da Intenção de Notificação via WhatsApp |
| 2 | [failure-recovery-policy.md](./04-phase-j-failure-recovery-policy.md) | Mapa de domínio de falhas, a tabela de falha e recuperação das 13 categorias, decisões formais, e decisões não resolvidas |


## Como ler

Caminho sugerido para uma revisão de tech lead:

1. Máquina de estados da intenção de notificação (diagrama da Phase I)
2. Diagrama de classificação de falha + a tabela de falha e recuperação (Phase J)
3. Registro de esclarecimentos (Phase K, §12)
4. Diagrama de limites de confiança + questões de compliance em aberto (Phase L, §14)

Se você quiser o resultado direto primeiro:

* Phase J — **FD-003**: nunca reenviar às cegas após um resultado ambíguo de crash/deploy

## Pergunta de engenharia

> O que acontece quando as coisas não funcionam como esperado?


## Próximo

Episódio 1 • Parte 5 — Provando a Prontidão
