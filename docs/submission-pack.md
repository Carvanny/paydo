# Submission Pack — UDAX 2026 (PayDo Cross-Rail)

Checklist operacional e estratégico para fechar a inscrição da **PayDo Cross-Rail** no **UBRI Digital Asset Xcelerator (UDAX) 2026**.

> Objetivo: garantir que a candidatura seja enviada com narrativa consistente, evidência técnica suficiente e materiais alinhados com a tese **“XRPL backend-only para auditabilidade no MVP”**.

---

## 1) Resumo da tese (usar como referência em todos os materiais)

### Tese do projeto (1 linha)
Plataforma **B2B2C** para pagamentos transfronteiricos **instantaneos e seguros** de obrigacoes (tributos, contas concessionarias e convenios de arrecadacao), com execucao em **rails locais** e **XRPL** como camada **backend-only** de auditabilidade e prova de integridade.

### O que isso evita (ruído)
- Não posicionar como “app de cripto para usuário final”
- Não prometer liquidação on-chain no MVP
- Não confundir ledger interno com XRPL

---

## 2) Checklist geral da submissão (visão rápida)

## Formulário
- [ ] Nome completo do contato primário
- [ ] E-mail principal
- [ ] Ranking de aceleradores (FGV / demais)
- [ ] Observações sobre ranking (viagem, visto, idioma, conexão regional etc.)
- [ ] Associação com universidades (se houver)
- [ ] Nome do projeto
- [ ] Descrição em 1 frase
- [ ] Descrição longa
- [ ] Link do projeto (live/demo, se houver)
- [ ] Link do GitHub (público, se houver)
- [ ] Histórico em programas XRPL/UBRI (se aplicável)
- [ ] Referência/recomendação (se houver)
- [ ] Como conheceu o UDAX

## Materiais obrigatórios / recomendados
- [ ] Pitch deck ou resumo executivo (PDF)
- [ ] Evidência técnica (GitHub + docs + diagrama)
- [ ] MVP/POC funcional (mesmo que parcial)
- [ ] Vídeo demo de 2 minutos (opcional, mas recomendado)
- [ ] Arquitetura/documentação organizada (README + docs)

## Coerência narrativa
- [ ] Problema real e recorrente
- [ ] Caso de uso inicial bem definido
- [ ] Papel da XRPL claro e defensável
- [ ] Escopo MVP e anti-escopo explícitos
- [ ] Roadmap de evolução (UDAX → pós-UDAX)

---

## 3) Pacote mínimo de artefatos (repositório/documentação)

## Já preparados (base)
- [ ] `README.md` (problema, solução, escopo MVP, XRPL adapter)
- [ ] `docs/architecture.md`
- [ ] `docs/system-diagram` (draw.io / png)
- [ ] `docs/udax-roadmap.md`
- [ ] `docs/glossary.md`
- [ ] `docs/pitch-script.md`
- [ ] `docs/faq-udax.md`
- [ ] `docs/event-model.md`

## Recomendado antes de enviar
- [ ] `docs/demo-script.md` (roteiro da demo em 2 min)
- [ ] `docs/api-spec.md` (OpenAPI simplificado ou payloads finais)
- [ ] `docs/risk-register.md` (riscos + mitigação)
- [ ] `docs/compliance-assumptions.md` (premissas, sem parecer jurídico)
- [ ] `docs/partner-integration-plan.md` (piloto, mocks, fallback)

> **Contraponto importante (intelectual e prático):**  
> Você pode cair na tentação de “documentar demais” e atrasar o envio. Para UDAX, o mais importante é **clareza + execução demonstrável**. Documentação extra só vale se aumentar confiança de forma direta.

---

## 4) Checklist do formulário (campo a campo) — versão prática

## A. Informações básicas
### Nome completo do contato primário
- [ ] Usar nome do founder principal (quem responderá rápido e pode participar do programa)

### E-mail principal
- [ ] E-mail ativo e monitorado diariamente
- [ ] Preferir domínio profissional (se tiver) ou Gmail confiável

---

## B. Ranking de aceleradores
### Pergunta: “Qual(is) acelerador(es) você gostaria de ser considerado?”
Sugestão de estratégia (ajuste conforme sua realidade):
- [ ] **1ª escolha: UDAX - FGV** (Brasil / timing / acesso)
- [ ] 2ª escolha: Berkeley (se fizer sentido)
- [ ] 3ª escolha: Oxford/UCT (se fizer sentido)
- [ ] 4ª escolha: outro / não aplicável

### Campo: considerações sobre o ranking
Checklist do que mencionar (se for verdade):
- [ ] Facilidade de viagem (Brasil)
- [ ] Proximidade com mercado alvo inicial (BR↔PY)
- [ ] Idioma / comunicação
- [ ] Disponibilidade do time nas datas
- [ ] Interesse em coorte internacional, mas prioridade por execução imediata

> **Boa prática:** mostrar preferência forte por FGV sem parecer fechado a outras opções.

---

## C. Associação com universidades
- [ ] Informar somente se houver vínculo real (aluno, ex-aluno, professor, colaborador)
- [ ] Se não houver, deixar claro e seguir

---

## D. Projeto
### Nome do Projeto
- [ ] Usar nome consistente em todos os materiais:
  - **PayDo Cross-Rail** (ou variante final padronizada)

### Descrição em 1 frase
Checklist:
- [ ] mencionar **B2B2C**
- [ ] mencionar **obrigações transfronteiriças**
- [ ] mencionar **rails locais**
- [ ] mencionar **XRPL auditabilidade backend-only** (ou equivalente)

### Descrição longa
Checklist:
- [ ] Problema real
- [ ] Público/cliente (parceiro B2B2C)
- [ ] Casos de uso iniciais (tributo/concessionaria/convenio_arrecadacao)
- [ ] Como funciona (orquestração + rails locais)
- [ ] Onde XRPL entra (hash + audit trail)
- [ ] Escopo MVP e visão de escala
- [ ] Por que é adequado ao UDAX (entrega em 8 semanas)

---

## E. Links
### Link do projeto (site/live)
- [ ] Se não houver site ainda, usar:
  - landing page simples
  - repositório + demo video
  - ambiente mock (se disponível)
- [ ] Evitar link quebrado / placeholder vazio

### Link do GitHub
- [ ] Repositório público minimamente organizado
- [ ] README atualizado
- [ ] docs acessíveis
- [ ] sem segredos / chaves expostas
- [ ] issues TODO podem existir (isso é ok)

> **Se o repo ainda não estiver “bonito”**: melhor um repo simples e coerente do que algo grande e confuso.

---

## F. Upload do pitch deck / resumo executivo
Checklist do arquivo PDF:
- [ ] Problema
- [ ] Solução
- [ ] Arquitetura (macro)
- [ ] Casos de uso MVP
- [ ] Papel da XRPL
- [ ] Roadmap UDAX
- [ ] Go-to-market (parceiros B2B2C)
- [ ] Time / capacidade de execução
- [ ] Pedido ao programa (o que quer acelerar)

### Nome sugerido do arquivo
- [ ] `PayDo_CrossRail_UDAX2026_Executive_Summary.pdf`
ou
- [ ] `PayDo_CrossRail_UDAX2026_PitchDeck.pdf`

---

## G. Histórico em programas anteriores XRPL/UBRI
- [ ] Responder com precisão
- [ ] Se nunca participou, dizer claramente
- [ ] Se participou/aplicou, informar:
  - programa/coorte
  - período
  - resultado
  - funding (se houve)

> **Risco de narrativa:** exagerar proximidade com o ecossistema pode ser pior do que assumir estágio inicial com boa tese técnica.

---

## H. Recomendação / referral
- [ ] Incluir apenas nome de quem realmente autorizaria contato
- [ ] Se não houver referência, não inventar
- [ ] Melhor sem referral do que referral fraco/inconsistente

---

## I. Como conheceu o UDAX
- [ ] Ser transparente (ex.: comunidade, LinkedIn, contato, divulgação da FGV, Ripple, indicação)

---

## 5) Checklist de pitch deck (conteúdo mínimo para subir confiança)

## Slide 1 — Título
- [ ] Nome do projeto
- [ ] tagline clara
- [ ] contato

## Slide 2 — Problema
- [ ] dores reais e específicas (cross-border obligations)
- [ ] fricção + rastreabilidade + reconciliação

## Slide 3 — Solução
- [ ] PayDo como orquestrador B2B2C
- [ ] execução em rails locais

## Slide 4 — Como a XRPL entra
- [ ] hash de comprovante
- [ ] audit trail
- [ ] backend-only
- [ ] sem PII on-ledger

## Slide 5 — Fluxo (jornada)
- [ ] tributos e concessionarias (principal)
- [ ] convenios de arrecadacao (secundario)

## Slide 6 — Arquitetura
- [ ] visão macro (core, integrations, ledger, receipt, XRPL adapter)

## Slide 7 — MVP e anti-escopo
- [ ] o que entra
- [ ] o que fica fora
- [ ] justificativa de foco

## Slide 8 — UDAX Fit / Plano de 8 semanas
- [ ] o que será entregue no programa
- [ ] demo E2E
- [ ] prova na XRPL (testnet/devnet)

## Slide 9 — Mercado / estratégia de distribuição
- [ ] parceiros B2B2C
- [ ] corredor inicial
- [ ] expansão por corredor

## Slide 10 — Time / capacidade
- [ ] arquitetura, integração, execução
- [ ] papel de cada fundador/colaborador

## Slide 11 — Roadmap
- [ ] MVP
- [ ] UDAX
- [ ] pós-UDAX

## Slide 12 — Ask
- [ ] mentoria técnica XRPL
- [ ] suporte de produto/escala
- [ ] acesso à rede e investidores

---

## 6) Checklist de demo (2 minutos — opcional, mas recomendado)

### Objetivo da demo
Provar utilidade da integração XRPL sem depender de produção completa.

### Fluxo recomendado (2 min)
- [ ] Criar solicitacao de tributo/concessionaria/convenio_arrecadacao (via API/portal)
- [ ] Mostrar mudança de status
- [ ] Mostrar emissão de comprovante
- [ ] Mostrar `receipt_hash`
- [ ] Mostrar evento/âncora na XRPL testnet/devnet (`xrpl_tx_ref`)
- [ ] Mostrar endpoint `/proofs/{payment_id}` (ou tela equivalente)

### O que evitar na demo
- [ ] Explicar tudo de blockchain
- [ ] Mostrar detalhes sensíveis
- [ ] Tentar demonstrar muitos casos de uso
- [ ] Confiar em integrações instáveis sem fallback/mock

### Plano B da demo
- [ ] Mocks/stubs preparados
- [ ] Script de execução ensaiado
- [ ] Screenshot/video backup
- [ ] Dados de exemplo padronizados

---

## 7) Checklist técnico (antes de enviar GitHub/links)

## Repositório
- [ ] README atualizado e legível
- [ ] estrutura `docs/` organizada
- [ ] diagrama abre corretamente
- [ ] sem arquivos quebrados
- [ ] sem credenciais/token no código
- [ ] `.env.example` (se aplicável)
- [ ] instruções mínimas para rodar (mesmo que mock)

## Documentação
- [ ] linguagem consistente (PT ou EN, evitar mistura excessiva)
- [ ] termos padronizados (usar glossário)
- [ ] escopo MVP alinhado com pitch e formulário
- [ ] XRPL descrita como backend-only no MVP

> **Teste de consistência (importante):**  
> Se alguém ler **formulário + README + deck + demo**, a narrativa precisa parecer a mesma empresa/produto.

---

## 8) Riscos de avaliação e como mitigar (visão crítica)

## Risco 1 — “Parece genérico demais”
**Sinal:** projeto parece “mais uma plataforma de pagamentos”.

**Mitigação**
- [ ] reforçar foco em **obrigações transfronteiriças**
- [ ] mostrar reconciliação + comprovante + prova auditável
- [ ] mostrar caso de uso concreto (tributo/concessionaria/convenio_arrecadacao)

---

## Risco 2 — “XRPL parece decorativa”
**Sinal:** banca sente que XRPL foi adicionada só para se encaixar no programa.

**Mitigação**
- [ ] explicar problema de integridade/rastreabilidade
- [ ] mostrar fluxo de hash + audit trail
- [ ] mostrar endpoint de prova ou consulta ligada ao uso da XRPL

---

## Risco 3 — “Escopo grande demais para 8 semanas”
**Sinal:** narrativa promete multi-país, vários trilhos e produção completa.

**Mitigação**
- [ ] delimitar MVP/anti-escopo
- [ ] mostrar roadmap por fases
- [ ] focar em 1 demo E2E sólida

---

## Risco 4 — “Dependência excessiva de parceiro externo”
**Sinal:** sem parceiro pronto, demo pode quebrar.

**Mitigação**
- [ ] usar mocks/stubs com clareza
- [ ] separar demonstração do core + XRPL
- [ ] ter plano B de demo gravada

---

## Risco 5 — “Compliance subestimado”
**Sinal:** respostas vagas ou triunfalistas sobre regulação.

**Mitigação**
- [ ] tratar compliance como camada crítica por corredor/parceiro
- [ ] evitar promessas jurídicas sem validação
- [ ] mostrar estratégia faseada

---

## 9) Pacote “Pronto para Enviar” (final)

Antes de apertar enviar:
- [ ] Revisar ortografia e consistência dos campos
- [ ] Testar todos os links (site, GitHub, demo)
- [ ] Confirmar que o PDF abre e está abaixo de 10 MB
- [ ] Verificar datas/disponibilidade do time para a coorte escolhida
- [ ] Confirmar e-mail de contato correto
- [ ] Salvar cópia das respostas do formulário (local/Doc)

### Snapshot recomendado (guardar internamente)
- [ ] versão do formulário enviada (texto copiado)
- [ ] versão do deck enviada
- [ ] versão do repo/documentação no momento da submissão
- [ ] gravação demo enviada (se houver)

---

## 10) Mensagem interna de alinhamento da equipe (sugestão)

> Nossa candidatura ao UDAX deve mostrar uma tese simples e forte: a PayDo resolve pagamentos transfronteiricos instantaneos e seguros para tributos, concessionarias e convenios de arrecadacao, com execucao em rails locais e XRPL no backend para auditabilidade e prova de integridade. O foco nao e “fazer tudo”, e sim demonstrar utilidade real, clareza de arquitetura e capacidade de execucao em 8 semanas.

---

## 11) Próximos passos sugeridos (operacional)

1. [ ] Fechar texto final da descrição curta e longa (formulário)
2. [ ] Revisar README + docs + draw.io
3. [ ] Montar pitch deck PDF (ou resumo executivo)
4. [ ] Preparar demo de 2 min (opcional, recomendado)
5. [ ] Validar links e anexos
6. [ ] Submeter antes do priority deadline
7. [ ] Registrar versão enviada e follow-up (e-mail/contato, se necessário)

---

## 12) Decisão estratégica (lembrar)
A proposta fica mais forte quando vocês mostram **foco, clareza e execução** — não quando tentam parecer “plataforma total” desde o dia 1.
