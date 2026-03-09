# FAQ — UDAX / Mentores / Banca (PayDo Cross-Rail)

## 1) Onde exatamente a XRPL entra?
A XRPL entra como **camada de auditabilidade backend-only** no MVP, registrando hashes de comprovantes e eventos auditáveis.

## 2) Vocês liquidam pagamentos em XRP?
**Não no MVP.** A liquidação ocorre em rails locais. No UDAX, o foco é prova/auditabilidade.

## 3) Se não liquidam on-chain, por que XRPL?
Porque auditabilidade verificável já gera valor real: integridade, trilha auditável e confiança operacional entre parceiros.

## 4) Qual o caso de uso inicial?
Pagamentos de **tributos**, **contas concessionarias** e **convenios de arrecadacao**, no corredor **BR↔PY**, via **B2B2C**, com foco em execucao instantanea e segura.

## 5) Quem é o cliente?
O cliente inicial é o **parceiro B2B** (fintech, integrador, escritório/plataforma), que atende o usuário final.

## 6) Como evitam dados sensíveis na blockchain?
Registramos apenas **hashes e metadados mínimos anonimizados**. Dados completos ficam **off-chain**.

## 7) Como provam integridade de comprovante?
Recalculando o hash do comprovante e comparando com o hash ancorado na XRPL.

## 8) O que conseguem demonstrar em 8 semanas?
Demo E2E com:
- criação de solicitação
- execução simulada/piloto
- comprovante
- hash + âncora XRPL (testnet/devnet)
- endpoint de prova

## 9) Principal risco?
Integração operacional/comercial com parceiros por corredor (mais do que a âncora XRPL em si).

## 10) Como escala para outros países?
Com arquitetura separando:
- core/orquestração (reaproveitável)
- adapters/rails por corredor (plugáveis)
- audit trail XRPL (componente comum)

## 11) Diferencial vs integrador tradicional?
Foco em **obrigações transfronteiriças estruturadas** + **prova auditável** (não apenas roteamento de pagamento).

## 12) Por que BR↔PY?
Corredor inicial para validação com dor real, sem limitar a tese de expansão para Mercosul.

## 13) Métricas de sucesso do MVP
- taxa de sucesso de execução
- tempo ate confirmacao de pagamento
- tempo de processamento
- tempo de emissão de comprovante
- % de âncoras XRPL concluídas
- tempo de reconciliação

## 14) Como lidam com duplicidade?
Com **idempotência** e `idempotency_key`.

## 15) Onde entra compliance?
É camada crítica do negócio por corredor/parceiro, mas não é o foco técnico principal da entrega UDAX.

## 16) Tese de longo prazo com XRPL?
Começar com auditabilidade e evoluir conforme maturidade operacional e regulatória.

## 17) Usuário final precisa entender blockchain?
Não. A XRPL fica transparente no backend.

## 18) O que esperam do UDAX?
Acelerar a integração XRPL, validar boas práticas e fortalecer tese para captação/parcerias.

## 19) Plano B se integração piloto atrasar?
Mock/stub + demo E2E do fluxo com prova XRPL.

## Linguagem recomendada
- Execução em rails locais, auditabilidade na XRPL
- XRPL backend-only no MVP
- Sem PII on-ledger

## Evitar
- “Tudo on-chain no MVP”
- “Nosso diferencial é só blockchain”
- Promessas regulatórias sem validação formal
