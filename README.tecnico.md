# PayDo - README Tecnico

## Objetivo

Este documento resume a visao tecnica do projeto PayDo para orientar arquitetura,
integracao e execucao do MVP.

## Escopo tecnico do MVP

- Modelo operacional: B2B2C (partner-first)
- Corredor inicial: BR_PY e PY_BR
- Tipos de obrigacao:
  - `tax_payment`
  - `utility_bill`
  - `collection_agreement_payment`
- Execucao financeira: rails locais/parceiros
- Auditabilidade: XRPL backend-only (testnet/devnet)
- Sem exposicao de cripto para usuario final

## Arquitetura (resumo)

Componentes principais:
- Partner API / Partner Portal
- Payment Orchestrator
- Quote & Routing
- Integration Layer (partners locais)
- Internal Ledger & Reconciliation
- Receipt Service
- XRPL Adapter

Fluxo base:
1. Criacao da solicitacao
2. Validacao e classificacao
3. Selecao de rota
4. Execucao no parceiro local
5. Atualizacao de status
6. Emissao de comprovante
7. Registro de hash/eventos na XRPL
8. Consulta de status/prova pelo parceiro

## Modelo de dominio

Entidade central: `PaymentRequest`
- `payment_id`
- `partner_id`
- `corridor`
- `obligation_type`
- `amount`
- `reference_data`
- `status`

Estados sugeridos:
`CREATED -> VALIDATED -> ROUTE_SELECTED -> SENT_TO_PARTNER -> PROCESSING -> CONFIRMED -> RECEIPT_ISSUED`

Estados de excecao:
- `FAILED`
- `REQUIRES_MANUAL_REVIEW`

## Eventos e prova auditavel

Eventos operacionais:
- `payment_request.created`
- `payment_request.validated`
- `payment_request.route_selected`
- `payment_request.sent_to_partner`
- `payment_request.processing`
- `payment_request.confirmed`
- `receipt.issued`

Eventos auditaveis:
- `audit.receipt_hash_generated`
- `audit.xrpl_anchor_requested`
- `audit.xrpl_anchor_submitted`
- `audit.xrpl_anchor_confirmed`
- `audit.xrpl_anchor_failed`

## API (v1 - referencia)

- `POST /v1/payment-requests`
- `GET /v1/payment-requests/{payment_id}`
- `GET /v1/payment-requests/{payment_id}/receipt`
- `GET /v1/proofs/{payment_id}`

## Nao-funcionais prioritarios

- Idempotencia por parceiro e chave de requisicao
- Seguranca de dados (transito e repouso)
- Correlation ID ponta a ponta
- Retry controlado e fallback para manual review
- Separacao clara entre status de pagamento e status da ancora XRPL

## Leitura recomendada (docs)

- `docs/architecture.md`
- `docs/mvp-technical-blueprint.md`
- `docs/event-model.md`
- `docs/glossary.md`
