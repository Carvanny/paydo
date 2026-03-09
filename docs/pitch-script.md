# Pitch Script — PayDo Cross-Rail (UDAX)

Roteiro de apresentação de **3–5 minutos** usando as abas do draw.io (`paydo-cross-rail-journeys-v2.drawio`).

## Objetivo do pitch
Demonstrar que a PayDo e uma plataforma **B2B2C** para pagamentos transfronteiricos **instantaneos e seguros** de obrigacoes (tributos, contas concessionarias e convenios de arrecadacao), com:
- execução em **rails locais**
- **XRPL backend-only** para auditabilidade e prova de integridade
- foco em MVP com corredor inicial **BR↔PY**, expandível para Mercosul

## Estrutura recomendada (3–5 min)

### 1) Problema e contexto
Pessoas e empresas com obrigacoes em outro pais enfrentam friccao, custo, demora e baixa rastreabilidade.

**Fala sugerida**
> A PayDo resolve um problema pratico de pagamentos transfronteiricos de obrigacoes reais, com foco em tributos, contas concessionarias e convenios de arrecadacao.

### 2) Tese do produto
A execucao ocorre em rails locais; a XRPL entra no backend para prova e auditabilidade.

**Frase-chave de valor**
- **Pagamentos instantaneos e seguros no corredor BR↔PY**

**Frase-chave**
- **XRPL como camada de auditabilidade backend-only no MVP**

## Uso das abas do draw.io

### Aba 01 — Executive Overview
Explique:
- canais de entrada (Partner/API/White-label)
- PayDo Core como orquestrador
- rails locais para execução
- ledger + comprovante
- XRPL adapter + XRPL para audit trail

### Aba 02 — Swimlane Tributos/Concessionarias
Fluxo principal do MVP:
1. parceiro cria solicitação
2. PayDo valida e seleciona rota
3. parceiro local executa
4. ledger atualiza
5. comprovante é emitido
6. hash/evento auditável vai para XRPL
7. parceiro recebe status + proof

### Aba 03 — Swimlane Convenios de Arrecadacao
Mostre:
- referencia estruturada (codigo de arrecadacao/ref)
- reconciliação
- exceções e manual review

### Aba 04 — XRPL Audit Trail Detailed
Destaque:
- comprovante completo fica **off-chain**
- hash é gerado
- payload mínimo é ancorado
- correlação `payment_id ↔ xrpl_tx_ref`

**Frases defensivas**
- Sem PII on-ledger
- Testnet/devnet durante UDAX
- Foco em auditabilidade, não liquidação on-chain no MVP

### Aba 05 — Partner API Contracts
Mostre prontidão de integração:
- `POST /v1/payment-requests`
- `GET /v1/payment-requests/{id}`
- `GET /v1/payment-requests/{id}/receipt`
- `GET /v1/proofs/{payment_id}`

### Aba 06 — Roadmap by Phase
- Fase 1: MVP operacional (BR↔PY)
- Fase 2: XRPL integration (UDAX)
- Fase 3: escala pós-UDAX (Mercosul)

## Encerramento sugerido
> A PayDo combina um problema real de pagamentos transfronteiricos com uma integracao XRPL pratica e defensavel. No MVP, a execucao fica em rails locais e a XRPL adiciona auditabilidade, seguranca operacional e prova de integridade.

## Checklist
- [ ] Diferenciar execução vs auditabilidade
- [ ] Reforçar backend-only
- [ ] Mostrar scope e anti-scope
- [ ] Mostrar API contracts
- [ ] Fechar com roadmap
