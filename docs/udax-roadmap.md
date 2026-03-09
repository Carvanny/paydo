# UDAX Roadmap — 8 semanas (PayDo Cross-Rail)

## Objetivo do programa

Acelerar um protótipo/MVP **B2B2C** de pagamentos transfronteiricos **instantaneos e seguros** de obrigações (tributos, concessionarias e convenios de arrecadacao) no corredor BR↔PY, com integracao **XRPL backend-only** para:
- hash de comprovantes
- trilha auditável de eventos

---

## Semana 1 — Escopo e alinhamento técnico
### Objetivos
- Consolidar escopo do MVP
- Definir fronteiras do corredor BR↔PY
- Refinar arquitetura de alto nível

### Entregáveis
- Escopo funcional (MVP + anti-escopo)
- Lista de eventos auditáveis
- Primeira versão do modelo de dados
- Diagrama de arquitetura (v1)

---

## Semana 2 — Contratos e fluxo transacional
### Objetivos
- Definir contratos de API/integração
- Padronizar estados de pagamento
- Estruturar payloads de comprovante

### Entregáveis
- Contratos de entrada/saída (Partner API)
- Máquina de estados (MVP)
- Estrutura de comprovante e hash strategy
- Plano de idempotência e correlação

---

## Semana 3 — Núcleo do orquestrador (protótipo funcional)
### Objetivos
- Implementar fluxo principal da solicitação
- Persistência inicial (ledger básico)
- Status tracking

### Entregáveis
- `create payment request`
- `get payment status`
- persistência de estados
- mock de integração de execução

---

## Semana 4 — Comprovante e conciliação básica
### Objetivos
- Gerar comprovantes
- Calcular hash de comprovante
- Registrar trilha de eventos interna

### Entregáveis
- Receipt service (v1)
- geração de hash
- eventos internos (`payment_created`, `receipt_issued`, etc.)
- conciliação básica (protótipo)

---

## Semana 5 — XRPL Adapter (testnet/devnet) v1
### Objetivos
- Integrar XRPL para ancoragem de hash
- Testar registro de eventos auditáveis

### Entregáveis
- XRPL adapter (v1)
- registro de `receipt_hash` na XRPL
- registro de eventos anonimizados
- retorno de referência de transação (testnet/devnet)

---

## Semana 6 — End-to-end demo e robustez mínima
### Objetivos
- Demonstrar fluxo ponta a ponta
- Tratar erros comuns / retries básicos
- Melhorar observabilidade mínima

### Entregáveis
- Demo E2E: solicitação → status → comprovante → hash XRPL
- fallback e tratamento de erro básico
- logs de correlação
- checklist técnico de demo day

---

## Semana 7 — Narrativa de produto, parceiros e escala
### Objetivos
- Refinar tese B2B2C
- Estruturar mensagem de valor
- Definir roadmap Mercosul

### Entregáveis
- proposta de valor por parceiro
- narrativa de expansão BR↔PY → Mercosul
- riscos e mitigação
- insumos para pitch deck

---

## Semana 8 — Pitch readiness e demo day prep
### Objetivos
- Consolidar demo e pitch
- Preparar material técnico e executivo
- Ensaiar respostas para banca/investidores

### Entregáveis
- demo script final (2 min + 5 min)
- pitch deck (versão programa)
- FAQ técnico/regulatório
- plano pós-UDAX (próximos 90 dias)

---

## Métricas de sucesso do programa (internas)
- Protótipo funcional B2B2C demonstrável
- Hash de comprovante registrado na XRPL (testnet/devnet)
- Trilha auditável de eventos registrada
- Arquitetura documentada e pronta para evolução
- Narrativa de captação/aceleração consistente
