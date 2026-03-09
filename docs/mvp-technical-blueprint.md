# MVP Technical Blueprint — PayDo Cross-Rail (Go + DDD + Event-Driven)

Blueprint técnico pragmático para o MVP da **PayDo Cross-Rail**, com foco em:
- entrega rápida com qualidade
- arquitetura evolutiva
- XRPL backend-only para auditabilidade (UDAX)

---

## 1) Objetivo técnico do MVP

Construir um MVP **B2B2C** para pagamentos transfronteiricos **instantaneos e seguros** de obrigacoes (tributos, concessionarias e convenios de arrecadacao), com:
- execução em **rails locais/parceiros**
- orquestração de estados ponta a ponta
- comprovante + prova auditável
- controles de seguranca e rastreabilidade ponta a ponta
- ancoragem de hash na **XRPL** (testnet/devnet)
- APIs para parceiros

---

## 2) Decisões arquiteturais (resumo)

### ADR-001 — Base
**Decisão:** **Modular Monolith** em Go + workers assíncronos.  
**Motivo:** menor complexidade operacional no MVP, boa separação de domínio.

### ADR-002 — Event-Driven
**Decisão:** eventos internos + **Outbox Pattern**.  
**Motivo:** desacoplamento sem precisar Kafka cedo.

### ADR-003 — Persistência
**Decisão:** **PostgreSQL**.  
**Motivo:** transações, JSONB, maturidade, `SKIP LOCKED` para workers.

### ADR-004 — XRPL no MVP
**Decisão:** XRPL **backend-only** para auditabilidade (hash/eventos), não liquidação on-chain.  
**Motivo:** foco UDAX e menor risco.

### ADR-005 — SAGA
**Decisão:** saga **orquestrada local** (state machine + commands + events) só onde necessário.  
**Motivo:** controle do fluxo sem arquitetura distribuída prematura.

---

## 3) Arquitetura lógica (alto nível)

### Componentes
1. **Partner API (Go/HTTP)**
   - recebe requests B2B2C
   - autentica parceiro
   - aplica idempotência
   - persiste solicitação inicial

2. **Payment Orchestrator (módulo core)**
   - valida
   - classifica obrigação
   - seleciona rota
   - controla estados
   - emite eventos

3. **Execution Adapters**
   - integra com parceiro/rail local
   - normaliza respostas/erros

4. **Receipt Module**
   - gera comprovante
   - salva referência

5. **Audit Proof Module**
   - canonicaliza payload
   - gera hash
   - expõe prova consultável

6. **XRPL Anchor Adapter**
   - envia hash/evento para XRPL (testnet/devnet)
   - retry/idempotência
   - persiste `xrpl_tx_ref`

7. **Workers**
   - processam outbox/jobs
   - executam tarefas assíncronas

8. **PostgreSQL**
   - core + eventos + outbox + idempotência + auditoria

9. **Object Storage (MinIO/S3)**
   - comprovantes e artefatos (recomendado)

---

## 4) Bounded Contexts (DDD pragmático)

> Boundaries no código (módulos), não microserviços agora.

### `payments`
Core de orquestração e state machine.  
Aggregate principal: `PaymentRequest`.

### `execution`
Adapters de parceiros/rails e normalização de status/erros.

### `receipts`
Geração e recuperação de comprovantes.

### `auditproof`
Hash/canonicalização/trilha auditável/consulta de proof.

### `xrplanchor`
Submissão na XRPL, retry e correlação.

### `partnerapi`
Handlers HTTP + DTOs + auth.

### `platform`
Infra compartilhada (db, tx, outbox, auth, logging, rate-limit).

---

## 5) Clean Architecture pragmática (Go)

### Regras
- Domínio não depende de infra.
- App layer coordena casos de uso.
- Infra implementa portas.
- Interface é criada onde é consumida.

### Estrutura por módulo
- `domain/` → entidades, VOs, regras
- `app/` → commands/handlers/queries
- `ports/` → contratos
- `infra/` → DB/HTTP/XRPL adapters

---

## 6) Domínio central (PaymentRequest)

### Campos principais (conceituais)
- `payment_id`
- `partner_id`
- `correlation_id`
- `idempotency_key`
- `corridor` (`BR_PY`, `PY_BR`)
- `obligation_type` (`tax_payment`, `utility_bill`, `collection_agreement_payment`)
- `amount`
- `status`
- `reference_data`
- `route_id`
- `execution_partner_id`
- `failure_code`, `failure_reason`
- `created_at`, `updated_at`

### Regras de domínio (exemplos)
- transição de estado inválida deve falhar
- comprovante só após `CONFIRMED`
- corredor e obligation_type devem estar suportados no MVP

### State machine (MVP)
`CREATED → VALIDATED → ROUTE_SELECTED → SENT_TO_PARTNER → PROCESSING → CONFIRMED → RECEIPT_ISSUED`

Desvios:
- `FAILED`
- `REQUIRES_MANUAL_REVIEW`

---

## 7) Comandos e queries (app layer)

### Commands (payments)
- `CreatePaymentRequest`
- `ValidatePaymentRequest`
- `SelectRoute`
- `SendToExecutionPartner`
- `MarkProcessing`
- `MarkConfirmed`
- `MarkFailed`
- `RequireManualReview`
- `MarkReceiptIssued`

### Commands (audit/xrpl)
- `GenerateReceipt`
- `GenerateReceiptHash`
- `RequestXRPLAnchor`
- `ConfirmXRPLAnchor`
- `FailXRPLAnchor`

### Queries
- `GetPaymentByID`
- `GetPaymentStatus`
- `GetReceiptByPaymentID`
- `GetProofByPaymentID`

---

## 8) Modelo de eventos (resumo implementável)

### Eventos operacionais
- `payment_request.created`
- `payment_request.validated`
- `payment_request.route_selected`
- `payment_request.sent_to_partner`
- `payment_request.processing`
- `payment_request.confirmed`
- `payment_request.failed`
- `payment_request.requires_manual_review`
- `receipt.issued`

### Eventos auditáveis (XRPL candidates)
- `audit.receipt_hash_generated`
- `audit.xrpl_anchor_requested`
- `audit.xrpl_anchor_submitted`
- `audit.xrpl_anchor_confirmed`
- `audit.xrpl_anchor_failed`

### Envelope mínimo
- `event_id`
- `event_type`
- `event_version`
- `occurred_at`
- `payment_id`
- `correlation_id`
- `partner_id`
- `payload`
- `meta`

---

## 9) Banco de dados (schema inicial sugerido)

### Tabelas principais
- `payment_requests`
- `payment_events`
- `execution_attempts`
- `receipts`
- `audit_proofs`
- `xrpl_anchors`
- `outbox_events`
- `partner_idempotency`

### Índices/padrões obrigatórios no MVP
- unique `(partner_id, idempotency_key)` em `payment_requests` (ou `partner_idempotency`)
- índices por `status`
- índices por `payment_id`
- unique `event_idempotency_key` em `xrpl_anchors`

### Observação de design
Separe:
- **status do pagamento**
- **status da âncora XRPL**

Assim, falha na XRPL não invalida pagamento já confirmado.

---

## 10) API do parceiro (MVP v1)

### Autenticação (MVP)
Headers:
- `X-Partner-Id`
- `X-API-Key`
- `Idempotency-Key` (para create)

> HMAC pode entrar depois, se necessário.

### Endpoints
- `POST /v1/payment-requests`
- `GET /v1/payment-requests/{payment_id}`
- `GET /v1/payment-requests/{payment_id}/receipt`
- `GET /v1/proofs/{payment_id}`

### Erros (padrão)
Campos:
- `code`
- `message`
- `correlation_id`

Códigos mínimos:
- `INVALID_REQUEST`
- `UNAUTHORIZED`
- `RATE_LIMITED`
- `NOT_FOUND`
- `INVALID_STATE_TRANSITION`
- `INTERNAL_ERROR`

---

## 11) Fluxo E2E (happy path do MVP)

1. Parceiro chama `POST /v1/payment-requests`
2. API valida + idempotência
3. Persiste `payment_request` em `CREATED`
4. Grava evento + `outbox_events`
5. Worker processa validação/roteamento/envio
6. Adapter de execução retorna confirmação
7. Status vai para `CONFIRMED`
8. Gera comprovante (`receipts`)
9. Gera hash (`audit_proofs`)
10. Solicita âncora XRPL (`xrpl_anchors`)
11. Confirma `xrpl_tx_ref`
12. `/proofs/{payment_id}` retorna trilha + hash + XRPL ref

### Desvios
- falha no parceiro → `FAILED`
- timeout/inconsistência → retry → `REQUIRES_MANUAL_REVIEW`
- falha XRPL → pagamento segue `CONFIRMED`; retry de âncora separado

---

## 12) SAGA local (prática)

### Onde usar
No fluxo de longa duração do pagamento.

### Como implementar (MVP)
- state machine no aggregate
- comandos explícitos
- eventos para passos assíncronos
- retries + backoff
- manual review como fallback

### Compensação
Só implementar se houver reversão real suportada por parceiro (cancel/rollback).

---

## 13) Infraestrutura recomendada

## 13.1 Dev local (Docker Compose)
Serviços sugeridos:
- `api`
- `worker`
- `postgres`
- `minio` (recomendado)
- `redis` (opcional)
- `prometheus/grafana` (opcional, depois)

### Exemplo (conceitual)
```yaml
version: "3.9"
services:
  postgres:
    image: postgres:16
    environment:
      POSTGRES_DB: paydo
      POSTGRES_USER: paydo
      POSTGRES_PASSWORD: paydo
    ports: ["5432:5432"]

  minio:
    image: minio/minio
    command: server /data --console-address ":9001"
    environment:
      MINIO_ROOT_USER: minio
      MINIO_ROOT_PASSWORD: minio123
    ports: ["9000:9000", "9001:9001"]

  api:
    build: .
    command: ["./bin/api"]
    env_file: .env
    ports: ["8080:8080"]
    depends_on: [postgres]

  worker:
    build: .
    command: ["./bin/worker"]
    env_file: .env
    depends_on: [postgres]
```

## 13.2 Piloto/demo (cloud)
### Opção alinhada a evolução
- AWS ECS Fargate (api/worker)
- RDS PostgreSQL
- S3
- CloudWatch
- Secrets Manager
- ALB

### Opção rápida
- Render / Railway / Fly.io + Postgres gerenciado

> Não travar MVP esperando infra perfeita.

---

## 14) Observabilidade (mínimo viável)

### Logs estruturados (obrigatório)
Campos mínimos:
- `timestamp`
- `level`
- `service`
- `payment_id`
- `correlation_id`
- `partner_id`
- `event_type`
- `error_code`

### Métricas mínimas
- latência por endpoint
- taxa de erro por endpoint
- jobs/outbox processados
- retries XRPL
- taxa de pagamento confirmado
- tempo até comprovante
- tempo até âncora confirmada

### Tracing
Opcional no começo.

---

## 15) Estrutura de repositório Go (sugerida)

```text
paydo/
├── cmd/
│   ├── api/
│   └── worker/
├── internal/
│   ├── payments/
│   ├── execution/
│   ├── receipts/
│   ├── auditproof/
│   ├── xrplanchor/
│   ├── partnerapi/
│   └── platform/
├── migrations/
├── deploy/
│   ├── docker-compose.yml
│   └── aws/
├── docs/
├── .env.example
├── Makefile
└── README.md
```

> Pode começar mais enxuto e evoluir.

---

## 16) Boas práticas de Go (aplicadas)

### Fazer
- interfaces pequenas
- `context.Context` em I/O
- erros explícitos
- testes de domínio + integração
- packages coesos
- nomes simples e claros

### Evitar
- “Java em Go”
- abstrações genéricas cedo
- interfaces para tudo
- microserviços antes de validar o fluxo

---

## 17) Segurança (mínimo viável)

- autenticação por parceiro (API key)
- rate limit por parceiro
- secrets fora do código
- TLS
- logs sem PII
- masking de referências sensíveis
- payload XRPL mínimo e anonimizado
- comprovante completo sempre off-chain
- `SHA-256` padronizado para hash

---

## 18) Roadmap de sprints (4–6 semanas)

### Sprint 1 — Fundação
- setup repo / Makefile / lint/test
- `cmd/api` + `cmd/worker`
- config + logging + db
- migrations iniciais
- `POST /v1/payment-requests`
- idempotência de entrada
- `GET /v1/payment-requests/{id}` básico

**Entregável:** create + status (`CREATED`)

### Sprint 2 — Orquestração + execução mock
- state machine
- `payment_events`
- `outbox_events`
- worker processando outbox
- mock execution adapter
- `execution_attempts`
- confirmação/falha mock
- `receipt.issued` básico

**Entregável:** fluxo E2E mock + comprovante básico

### Sprint 3 — Audit proof + XRPL (UDAX core)
- `receipts`, `audit_proofs`, `xrpl_anchors`
- canonicalização + hash
- eventos `audit.*`
- XRPL adapter (testnet/devnet)
- `GET /v1/proofs/{payment_id}`

**Entregável:** proof com hash + `xrpl_tx_ref`

### Sprint 4 — Robustez para demo/piloto
- retry/backoff XRPL
- `REQUIRES_MANUAL_REVIEW`
- rate limit
- padronização de erros
- logs estruturados
- testes críticos
- seed + script de demo

**Entregável:** demo E2E estável (plano A/B)

### Sprint 5/6 (opcional)
- parceiro piloto real
- polling/status externo
- webhook (beta)
- deploy cloud
- observabilidade mais rica

---

## 19) Definition of Done (por feature)

Uma feature está pronta quando:
- [ ] regra de domínio implementada
- [ ] handler/caso de uso funcional
- [ ] migration/persistência ajustada
- [ ] evento emitido (se aplicável)
- [ ] logs com `payment_id` e `correlation_id`
- [ ] teste mínimo
- [ ] docs de endpoint atualizadas (se API)

---

## 20) Riscos técnicos e mitigação (visão crítica)

### Risco A — Overengineering
**Mitigação:** modular monolith + foco em 1 fluxo E2E.

### Risco B — Acoplar sucesso do pagamento à XRPL
**Mitigação:** status da âncora separado.

### Risco C — Duplicidade por retries
**Mitigação:** idempotência de entrada + idempotência de âncora.

### Risco D — Hash inconsistente
**Mitigação:** biblioteca única de canonicalização + testes determinísticos.

### Risco E — Demo dependente de parceiro externo
**Mitigação:** adapter mock + plano B (seed/screenshot/video).

---

## 21) Próximos artefatos técnicos recomendados

- `docs/api-spec.md` (OpenAPI simplificado)
- `docs/db-schema.md` (ER + migrations)
- `docs/runbook-demo.md` (plano A/B)
- `docs/adr/` (ADRs curtos)
- `deploy/docker-compose.yml` real
- `Makefile` com `run-api`, `run-worker`, `migrate-up`, `test`

---

## 22) Resumo técnico final

A PayDo deve começar como um **modular monolith em Go** com **DDD pragmático**, **Clean Architecture leve**, **event-driven interno com outbox** e **saga orquestrada local**. A execução dos pagamentos ocorre em **rails locais/parceiros**, enquanto a **XRPL** é usada no **backend** para hash e trilha auditável verificável, sem expor dados sensíveis on-ledger. Essa abordagem preserva velocidade, clareza arquitetural e capacidade de evolução para piloto e escala.
