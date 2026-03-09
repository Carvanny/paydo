# PayDo

PayDo e uma plataforma para pagamentos transfronteiricos instantaneos e seguros,
com foco inicial em **tributos**, **concessionarias** e **convenios de arrecadacao**,
conectando **Brasil <-> Paraguai** com eficiencia e simplicidade.

## Perfis de leitura

- Visao tecnica: `README.tecnico.md`
- Visao comercial: `README.comercial.md`

## Visao do projeto

Este repositorio concentra os artefatos de definicao do produto e arquitetura:
- narrativa de negocio e proposta de valor
- arquitetura tecnica do MVP
- roteiro de pitch e submissao UDAX
- modelos de eventos e glossario
- template de landing page

## Escopo MVP

- Modelo: B2B2C (parceiro como canal principal)
- Corredor inicial: Brasil <-> Paraguai
- Casos de uso foco:
  - pagamentos de tributos
  - pagamentos de contas concessionarias
  - pagamentos em convenios de arrecadacao
- Execucao: rails locais/parceiros
- Auditabilidade: XRPL no backend (sem exposicao de cripto ao usuario final)

## Estrutura do repositorio

```text
workspace_paydo/
├── README.md
├── docs/
│   ├── README.md
│   ├── architecture.md
│   ├── mvp-technical-blueprint.md
│   ├── event-model.md
│   ├── glossary.md
│   ├── faq-udax.md
│   ├── pitch-script.md
│   ├── submission-pack.md
│   ├── udax-roadmap.md
│   ├── paydo-cross-rail-journeys.drawio
│   └── paydo-cross-rail-journeys-v2.drawio
└── template/
    ├── index.html
    ├── styles.css
    ├── logo.png
    └── ...
```

## Documentacao principal

- Produto e escopo: `docs/README.md`
- Arquitetura: `docs/architecture.md`
- Blueprint tecnico: `docs/mvp-technical-blueprint.md`
- Modelo de eventos: `docs/event-model.md`
- Glossario: `docs/glossary.md`
- FAQ de banca/mentoria: `docs/faq-udax.md`
- Roteiro de pitch: `docs/pitch-script.md`
- Pacote de submissao: `docs/submission-pack.md`
- Roadmap 8 semanas: `docs/udax-roadmap.md`

## Como visualizar o template web

```bash
cd template
python3 -m http.server 8080
```

Abra no navegador:

- http://localhost:8080

## Estado atual

Este repositorio esta focado em descoberta, planejamento e material de apresentacao do MVP.
Nao e um backend de producao pronto para deploy.

## Proximos passos sugeridos

1. Consolidar contrato de API (`POST /v1/payment-requests`, `GET /v1/payment-requests/{id}`).
2. Definir politicas de seguranca e SLA de confirmacao para jornada instantanea.
3. Implementar prototipo funcional do fluxo E2E com prova auditavel.
