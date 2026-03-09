# Architecture — PayDo Cross-Rail (MVP)

## Visão geral

A PayDo Cross-Rail e uma plataforma **B2B2C** para orquestracao de pagamentos transfronteiricos **instantaneos e seguros** de obrigacoes. O MVP inicia no corredor **Brasil ↔ Paraguai**, com execucao via trilhos tradicionais/parceiros e uso da **XRPL no backend** (testnet/devnet) para auditabilidade.

## Objetivos da arquitetura (MVP)

- Permitir criação e acompanhamento de solicitações de pagamento por parceiros
- Padronizar fluxo de orquestração para diferentes tipos de obrigação
- Garantir confirmacao rapida de status e trilha segura ponta a ponta
- Manter ledger interno e conciliação básica
- Gerar comprovante de pagamento
- Registrar evidências auditáveis na XRPL (hash + eventos)
- Preparar evolução para novos corredores e novos rails

## Componentes principais

### 1. Partner Portal / Partner API
Canal de entrada B2B2C para parceiros:
- criar solicitação de pagamento
- consultar status
- recuperar comprovante
- consultar histórico básico

### 2. Payment Orchestrator
Núcleo de orquestração:
- valida payload
- classifica tipo de obrigação
- seleciona rota de execução (MVP: parceiros/trilhos tradicionais)
- coordena transições de estado

### 3. Quote & Routing Module (MVP simplificado)
Responsável por:
- estimativa de custo/prazo
- escolha de rota disponível
- fallback básico (quando aplicável)

### 4. Integration Layer (Bank / Payment Partners)
Adaptadores para parceiros de execução em BR/PY:
- envio de instrução
- consulta de status
- confirmação de execução
- tratamento de falhas/timeout (MVP simples)

### 5. Internal Ledger & Reconciliation
Mantém trilha interna de:
- solicitação
- status de processamento
- execução
- valores e referência
- conciliação básica
- inconsistências pendentes

### 6. Receipt Service
Gera e armazena comprovante:
- metadados da transação
- referência da obrigação
- timestamps
- status final
- hash do comprovante

### 7. XRPL Adapter (testnet/devnet)
Componente backend-only para:
- registrar hash de comprovante
- registrar eventos anonimizados de pagamento
- recuperar/validar referência on-ledger (quando necessário)

## Fluxo principal (MVP)

1. **Parceiro cria solicitação**
   - `payment_request` com dados da obrigação
2. **Orquestrador valida e classifica**
   - tipo: tributo/concessionaria/convenio_arrecadacao
3. **Cotação/rota**
   - rota selecionada (MVP: trilhos tradicionais/parceiro)
4. **Execução**
   - integração com parceiro local BR/PY
5. **Atualização de status**
   - ledger interno recebe eventos de estado
6. **Comprovante**
   - serviço de comprovante gera documento + hash
7. **Audit trail XRPL**
   - XRPL Adapter registra hash de comprovante e eventos anonimizados
8. **Parceiro consulta resultado**
   - status final + comprovante

## Estados sugeridos de pagamento (MVP)

- `CREATED`
- `VALIDATED`
- `ROUTE_SELECTED`
- `SENT_TO_PARTNER`
- `PROCESSING`
- `CONFIRMED`
- `RECEIPT_ISSUED`
- `FAILED`
- `REQUIRES_MANUAL_REVIEW`

## Modelo lógico de dados (MVP)

### PaymentRequest
- id
- partner_id
- corridor (ex.: BR_PY / PY_BR)
- obligation_type (tax_payment | utility_bill | collection_agreement_payment)
- source_currency
- destination_currency
- amount
- reference_data (estruturado por tipo)
- status
- created_at / updated_at

### PaymentExecution
- id
- payment_request_id
- route_id
- partner_execution_ref
- sent_at
- confirmed_at
- execution_status
- error_code (opcional)

### Receipt
- id
- payment_request_id
- receipt_number
- receipt_payload_uri (ou storage ref)
- receipt_hash
- issued_at

### AuditEvent
- id
- payment_request_id
- event_type
- event_payload_hash
- xrpl_tx_ref (opcional no MVP inicial)
- created_at

## XRPL Adapter — desenho funcional (MVP)

### Inputs
- `receipt_hash`
- `payment_id`
- `event_type`
- `event_payload_hash`
- timestamps e metadados anonimizados

### Outputs
- `xrpl_tx_ref` (hash/ref da transação em testnet/devnet)
- status de submissão/confirmado
- erro técnico (se houver)

### Regras do MVP
- Sem dados pessoais sensíveis on-ledger
- Somente hashes e metadados mínimos
- Retries simples com fila básica (ou mecanismo equivalente)
- Idempotência por evento/hash

## Segurança e compliance (MVP — diretrizes)

- XRPL usado apenas para auditabilidade (backend)
- Dados sensíveis e comprovantes completos ficam off-chain
- Criptografia em transito e em repouso para dados operacionais
- Logs com correlação por IDs internos
- Controles de acesso por parceiro
- Preparação para trilha de auditoria e conciliação

## Evolução pós-MVP

- Multi-corridor (Mercosul)
- Motor de roteamento mais sofisticado (custo/prazo/risco)
- APIs externas para parceiros maiores
- Observabilidade avançada
- Expansão do uso de XRPL para novos casos de interoperabilidade
