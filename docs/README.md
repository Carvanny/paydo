# PayDo Cross-Rail

Plataforma **B2B2C** para pagamentos transfronteiricos **instantaneos e seguros** de obrigações (tributos, contas concessionarias e convenios de arrecadacao), com corredor inicial **Brasil ↔ Paraguai** e integracao **XRPL backend-only** para auditabilidade.

## Problema

Pessoas e pequenos negocios com obrigacoes em mais de um pais enfrentam um processo fragmentado para quitar tributos, contas concessionarias e convenios de arrecadacao:

- múltiplos intermediários
- custos pouco transparentes
- baixa rastreabilidade
- dificuldade de conciliação
- ausência de prova de pagamento verificável

O problema e especialmente critico para **obrigacoes recorrentes** (tributos e contas concessionarias) e **convenios de arrecadacao**, onde confiabilidade e comprovante importam mais do que uma remessa generica.

## Solução

A **PayDo Cross-Rail** permite que parceiros (fintechs, escritorios contabeis, integradores, comunidades) oferecam aos seus clientes uma experiencia simples, instantanea e segura para quitar obrigacoes transfronteiricas.

### Como funciona (visão de produto)
1. O parceiro cria uma solicitacao de pagamento (tributo/concessionaria/convenio_arrecadacao)
2. O sistema valida, cotiza e escolhe uma rota de execução
3. O pagamento é executado via trilhos tradicionais/parceiros locais (MVP)
4. A plataforma gera comprovante, status e conciliação
5. O backend registra evidências de auditoria na XRPL (testnet/devnet no MVP)

## Escopo do MVP

### Público-alvo (MVP)
- **B2B2C com parceiro desde o início**
  - fintechs locais
  - escritórios contábeis/administrativos
  - integradores
  - comunidades com demandas transfronteiriças

### Corredor inicial
- **Brasil ↔ Paraguai (bidirecional)**

### Obrigações foco
- **Tributos**
- **Contas concessionarias**
- **Convenios de arrecadacao**

### Funcionalidades MVP
- Cadastro/autenticação de parceiro (simples)
- Criação de solicitação de pagamento
- Status da transação
- Geração de comprovante
- Confirmacao rapida de status para jornada de pagamento instantaneo
- Ledger interno básico
- Conciliação básica
- XRPL adapter (hash + trilha auditável)

## Anti-escopo (não entra agora)

- App consumidor final completo
- Cartão de crédito
- Liquidação on-chain em produção
- Multi-país em execução real (além de BR↔PY)
- Exposição de cripto/XRPL ao usuário final
- Cobertura ampla de tipos de obrigação sem padronização

## XRPL Adapter (backend-only)

No MVP, a XRPL não é visível ao usuário final. Ela será usada no backend (testnet/devnet) para:

### 1) Hash de comprovante
- Gerar hash do comprovante emitido
- Registrar hash na XRPL
- Permitir verificação de integridade do comprovante

### 2) Trilha auditável de eventos
Registro (anonimizado) de eventos-chave do ciclo de pagamento, por exemplo:
- `payment_created`
- `route_selected`
- `payment_sent`
- `payment_confirmed`
- `receipt_issued`

### Objetivo
Criar uma camada de **confiança, integridade e auditabilidade** preparada para evolução futura da liquidação em corredores regionais.

## Estrutura de documentação

- `README.md` — visão geral, problema, solução, escopo MVP e XRPL adapter
- `docs/architecture.md` — arquitetura técnica e fluxos
- `docs/udax-roadmap.md` — plano de 8 semanas para o UDAX
- `docs/system-diagram.png` — diagrama de alto nível

## Roadmap de evolução (visão)
1. Validar BR↔PY com parceiros
2. Consolidar modelo B2B2C e conciliação
3. Expandir tipos de obrigação
4. Expandir para novos corredores (Mercosul)
5. Evoluir opções de liquidação multi-rail
