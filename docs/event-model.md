# Event Model — PayDo Cross-Rail (MVP / UDAX)

Modelo inicial de eventos auditáveis para a PayDo com foco em rastreabilidade, prova de integridade e ancoragem na XRPL (backend-only).

## 1. Objetivos
1. Reconstituir o ciclo de vida da solicitação de pagamento.
2. Separar eventos operacionais e eventos auditáveis.
3. Suportar prova de integridade sem expor PII on-ledger.
4. Alimentar APIs de status/comprovante/prova.

## 2. Princípios
- **Sem PII on-ledger**
- **Canonicalização antes de hash**
- **Idempotência por evento**
- **Correlation-first**

## 3. Entidades conceituais
- `payment_request`
- `execution_attempt`
- `receipt`
- `audit_event`
- `xrpl_anchor`

## 4. Identificadores recomendados
- `payment_id`
- `partner_id`
- `correlation_id`
- `execution_attempt_id`
- `receipt_id`
- `idempotency_key`
- `event_idempotency_key`
- `audit_event_id`
- `xrpl_tx_ref`

## 5. Taxonomia inicial (MVP)

### Eventos operacionais
- `payment_request.created`
- `payment_request.validated`
- `payment_request.route_selected`
- `payment_request.sent_to_partner`
- `payment_request.processing`
- `payment_request.confirmed`
- `payment_request.failed`
- `payment_request.requires_manual_review`
- `payment_request.reconciled`
- `receipt.issued`

### Eventos auditáveis (XRPL candidates)
- `audit.receipt_hash_generated`
- `audit.event_payload_hashed`
- `audit.xrpl_anchor_requested`
- `audit.xrpl_anchor_submitted`
- `audit.xrpl_anchor_confirmed`
- `audit.xrpl_anchor_failed`

## 6. Envelope base do evento (exemplo)
```json
{
  "event_id": "evt_01...",
  "event_type": "payment_request.created",
  "event_version": "1.0",
  "occurred_at": "2026-02-25T12:34:56Z",
  "correlation_id": "corr_01...",
  "payment_id": "pay_01...",
  "partner_id": "partner_abc",
  "environment": "dev",
  "source": "paydo-orchestrator",
  "payload": {},
  "meta": {
    "idempotency_key": "idem_...",
    "schema_ref": "event-schema/payment_request.created/v1"
  }
}
```

## 7. Payloads sugeridos (resumo)

### `payment_request.created`
```json
{
  "obligation_type": "tax_payment",
  "corridor": "BR_PY",
  "amount": { "value": "150.00", "currency": "BRL" },
  "reference_data": { "type": "tax_payment", "reference_id": "tax_or_collection_internal_ref" }
}
```

### `receipt.issued`
```json
{
  "receipt_id": "rcpt_01...",
  "receipt_ref": "receipt://internal/rcpt_01...",
  "issued_at": "2026-02-25T12:35:40Z"
}
```

### `audit.receipt_hash_generated`
```json
{
  "receipt_id": "rcpt_01...",
  "hash_algorithm": "SHA-256",
  "receipt_hash": "hex_or_base64_hash_value"
}
```

### `audit.xrpl_anchor_confirmed`
```json
{
  "audit_event_id": "aud_01...",
  "xrpl_tx_ref": "XRPL_TX_HASH_OR_REF",
  "network": "testnet",
  "anchored_at": "2026-02-25T12:35:55Z"
}
```

## 8. Evento auditável derivado (conceito)
Nem todo evento operacional precisa ser ancorado. Criar um `audit_event` derivado com payload mínimo (sem PII), hashado e correlacionado ao `payment_id`.

## 9. Diretriz de canonicalização e hashing
1. Selecionar payload mínimo (sem PII)
2. Ordenar campos de forma determinística
3. Serializar em JSON canônico
4. Aplicar hash (ex.: SHA-256)
5. Guardar hash + algoritmo + referências (off-chain)

## 10. Fluxo de âncora na XRPL (MVP)
1. `receipt.issued`
2. `audit.receipt_hash_generated`
3. `audit.xrpl_anchor_requested`
4. `audit.xrpl_anchor_submitted`
5. `audit.xrpl_anchor_confirmed`

## 11. Falhas, retry e idempotência
- usar `event_idempotency_key`
- retries com limite
- registrar `audit.xrpl_anchor_failed`
- fallback para manual review após exceder política

## 12. Relação com APIs
Esse modelo alimenta:
- `/payment-requests/{id}`
- `/payment-requests/{id}/receipt`
- `/proofs/{payment_id}`
- webhooks (futuro)

## 13. Resposta sugerida para `/proofs/{payment_id}` (resumo)
Retornar:
- `payment_id`
- `status`
- `receipt_hash`
- `hash_algorithm`
- trilha de eventos relevantes
- `xrpl_tx_ref` (quando houver)

## 14. Decisões em aberto
1. Quais eventos serão ancorados no MVP?
2. Âncora por evento ou lote (futuro)?
3. Biblioteca padrão de canonicalização/hash
4. Política de retry do XRPL Adapter
5. Contrato final do endpoint `/proofs/{payment_id}`

## 15. Recomendação prática para o UDAX
Para maximizar entrega em 8 semanas:
- focar em 1 fluxo E2E (ex.: tributo/concessionaria/convenio_arrecadacao)
- ancorar `receipt_hash` + 1 evento auditável
- expor consulta de prova simples
- demonstrar claramente o que é off-chain vs on-ledger
