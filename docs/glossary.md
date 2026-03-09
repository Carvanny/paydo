# Glossário — PayDo Cross-Rail (MVP / UDAX)

Glossário oficial de termos usados nos desenhos, documentação e pitch do projeto **PayDo Cross-Rail**.

---

## 1. Termos de negócio e produto

### B2B2C
Modelo em que a PayDo atende um **parceiro (B2B)** que, por sua vez, atende o **cliente final (C)**.

**Exemplo no projeto:** fintech, escritório contábil ou integrador oferecendo a solução ao usuário final.

### Parceiro (Partner)
Empresa/canal que integra com a PayDo para operar ou distribuir o serviço.

**Exemplos:**
- fintech
- escritório contábil
- integrador
- plataforma parceira

### Corredor (Corridor)
Caminho operacional de pagamento entre dois países/mercados.

**Exemplo:** `BR↔PY` (Brasil ↔ Paraguai).

### Bidirecional
Capacidade de operar nos dois sentidos do corredor.

**Exemplo:**
- Brasil → Paraguai
- Paraguai → Brasil

### Obrigações (Obligations)
Pagamentos com finalidade definida, normalmente vinculados a uma cobrança, conta ou compromisso operacional.

**Exemplos:**
- tributo
- conta de concessionaria
- convenio de arrecadacao
- parcelas

> Diferente de uma remessa genérica de dinheiro.

### Utility Bill / Conta Concessionária
Conta emitida por concessionaria de serviço essencial (água, luz, telefone etc.).

### Convenio de Arrecadacao
Pagamento vinculado a convenio formal de arrecadacao, geralmente associado a codigo de arrecadacao, convenio e referencia operacional.

### Referência Estruturada
Conjunto de dados padronizados para identificar e reconciliar um pagamento.

**Exemplos de referência:**
- codigo de arrecadacao
- codigo do tributo
- código do convenio
- identificador de cobrança

### Pagamento Instantaneo
Pagamento com confirmacao em baixa latencia, de acordo com disponibilidade do rail local e politicas do parceiro.

### Seguranca Operacional
Conjunto de controles para proteger autenticacao, dados, execucao e conciliacao ponta a ponta.

### MVP (Minimum Viable Product)
Versão mínima do produto com funcionalidades essenciais para validar operação, proposta de valor e integração.

### Anti-escopo (Out of Scope)
Itens explicitamente excluídos do MVP para manter foco e velocidade.

**Exemplos no projeto:**
- exposição de cripto ao usuário final
- cartão de crédito
- liquidação on-chain em produção
- expansão para muitos países no início

---

## 2. Termos de arquitetura e integração

### Orchestrator / Orquestrador
Componente central que coordena o fluxo de pagamento ponta a ponta.

**Responsabilidades típicas:**
- validar a solicitação
- classificar tipo de obrigação
- selecionar rota
- controlar estados
- acionar integrações
- consolidar retorno

### Routing / Seleção de Rota
Processo de escolha de como um pagamento será executado.

**Critérios comuns:**
- custo
- prazo
- disponibilidade
- tipo de obrigação
- corredor (país de origem/destino)

### Integration Layer / Camada de Integração
Camada que conecta a PayDo com serviços externos (parceiros/bancos/rails).

### Local Rails / Trilhos Locais
Infraestruturas de pagamento já existentes no país que efetivamente executam a transação (bancos, gateways, parceiros locais).

### Partner API
API da PayDo exposta para parceiros B2B2C.

### White-label
Modelo em que o parceiro pode oferecer a solução com a própria marca/interface.

### State Machine / Máquina de Estados
Modelo que define estados válidos de uma transação e as transições permitidas entre eles.

**Exemplo de fluxo de estados (MVP):**
`CREATED → VALIDATED → ROUTE_SELECTED → SENT_TO_PARTNER → PROCESSING → CONFIRMED → RECEIPT_ISSUED`

Estados de exceção:
- `FAILED`
- `REQUIRES_MANUAL_REVIEW`

### Ledger Interno
Registro operacional interno da PayDo com dados da transação, estados, referências e correlações.

**Importante:** não é blockchain.

### Reconciliação (Reconciliation)
Processo de conferência para garantir consistência entre solicitação, execução, valor, comprovante e status final.

### Receipt Service / Serviço de Comprovante
Componente que gera e disponibiliza o comprovante de pagamento.

### Comprovante (Receipt)
Prova operacional do pagamento realizado.

### Proof / Prova
Evidência verificável associada ao pagamento, combinando comprovante, hash, trilha auditável e referência XRPL (quando aplicável).

### API Contract / Contrato de API
Definição dos endpoints, payloads, campos e respostas esperadas para integração.

### Webhook
Notificação automática ao parceiro quando há mudança de status (item futuro no MVP).

---

## 3. Termos de XRPL e auditabilidade

### XRPL (XRP Ledger)
Ledger distribuído usado como camada de auditabilidade no backend (MVP/UDAX).

### XRPL Adapter
Componente intermediário entre a PayDo e a XRPL para:
- preparar payload mínimo
- aplicar regras de privacidade
- enviar registros
- controlar idempotência/retry
- armazenar referência da transação XRPL

### Backend-only
Uso da XRPL apenas nos bastidores. O usuário final não vê XRP e não precisa carteira.

### Testnet / Devnet
Redes de teste/desenvolvimento usadas para validação técnica e demo.

### Hash
Resumo criptográfico (impressão digital) de um conteúdo; se o conteúdo muda, o hash muda.

### Receipt Hash
Hash derivado do comprovante, usado para ancoragem e verificação de integridade.

### Anchoring / Âncora
Registro de um hash/evento na XRPL para prova temporal e integridade.

### Audit Trail / Trilha Auditável
Sequência de eventos que permite reconstituir o ciclo de vida de uma transação.

**Exemplos de eventos:**
- `payment_created`
- `route_selected`
- `payment_sent`
- `payment_confirmed`
- `receipt_issued`

### Event Payload Hash
Hash do payload de um evento, usado para prova de integridade da trilha.

### On-ledger / On-chain
Dados registrados no ledger (XRPL) — no MVP: hashes e metadados mínimos anonimizados.

### Off-chain
Dados mantidos fora do ledger, em sistemas internos (comprovante completo, dados sensíveis, payloads operacionais).

### `xrpl_tx_ref`
Referência da transação registrada na XRPL para correlação e consulta.

---

## 4. Robustez operacional e engenharia

### Idempotência (Idempotency)
Garantia de que repetir a mesma solicitação não duplica o efeito da operação.

### `idempotency_key`
Chave enviada pelo parceiro para identificar uma tentativa lógica e evitar duplicações.

### Retry
Nova tentativa automática após falha temporária.

### Timeout
Tempo limite de espera por resposta de serviço externo.

### Correlation ID / ID de Correlação
Identificador para rastrear uma transação ponta a ponta entre componentes e integrações.

### Manual Review
Análise/tratamento manual para exceções não resolvidas automaticamente.

### Exception Handling / Tratamento de Exceções
Estratégias para lidar com falhas sem perder consistência e rastreabilidade.

### Observabilidade (Observability)
Capacidade de entender o comportamento do sistema via logs, métricas e rastreamento.

---

## 5. Termos de apresentação e planejamento (UDAX)

### UDAX
UBRI Digital Asset Xcelerator, programa de aceleração ligado ao ecossistema XRPL.

### Demo E2E (End-to-End)
Demonstração ponta a ponta do fluxo principal.

### Pitch Readiness
Nível de preparo para apresentar o projeto com narrativa clara, demo e arquitetura coerente.

### Roadmap por Fase
Planejamento por etapas para evoluir o projeto com foco:
- MVP Operacional
- Integração XRPL (UDAX)
- Escala pós-UDAX

---

## 6. Observações de alinhamento (importante para a equipe)

1. **XRPL no MVP não significa expor cripto ao usuário.**  
   É uso **backend-only** para auditabilidade.

2. **Ledger interno e XRPL têm papéis diferentes.**  
   - Ledger interno = operação e conciliação  
   - XRPL = prova de integridade / trilha auditável

3. **B2B2C é parte central da tese.**  
   O parceiro é canal de distribuição e integração desde o início.

4. **Corredor inicial não limita a visão do produto.**  
   BR↔PY é validação inicial; o roadmap prevê expansão.

---

## 7. Padronização de linguagem para pitch

Para evitar ruído em apresentações:
- Dizer: **“XRPL como camada de auditabilidade backend-only no MVP”**
- Evitar: **“vamos liquidar tudo em cripto no MVP”**
- Diferenciar sempre: **“pagamento executado em rails locais”** vs **“prova/auditoria registrada na XRPL”**
