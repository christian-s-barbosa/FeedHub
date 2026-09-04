---
fase: 3
titulo: Arquitetura
data: 2026-09-02
status: rascunho
milestone: Fase 3 — Arquitetura (#4)
---

# Fase 3 — Arquitetura

> **Status:** rascunho | validado
> **Fase:** 3 de 8

## Kanban

| Card | Título                                        | Status      |
| ---- | --------------------------------------------- | ----------- |
| #21  | Fase 3: Definir stack do backend              | Done        |
| #22  | Fase 3: Definir stack do frontend             | Done        |
| #23  | Fase 3: Definir banco de dados                | Done        |
| #24  | Fase 3: Definir infraestrutura                | Done        |
| #25  | Fase 3: Definir camadas e fluxos              | Todo        |
| #28  | ↳ Fluxo do worker (ingestão)                  | Todo        |
| #29  | ↳ Fluxo do site (exibição)                    | Todo        |
| #26  | Fase 3: Modelagem de dados e contratos de API | Todo        |
| #27  | Fase 3: Validar doc de arquitetura            | Todo        |
| #20  | Fase 3 — Arquitetura (épico)                  | In Progress |

> Prazo dos cards: 2026-09-04. As referências de card preenchem quando o card atinge `Done`.

## Objetivo

Decisões técnicas de alto nível: stack, camadas (dados/serviço/apresentação), como RSS e busca se conectam, modelagem de dados e contratos de API. Cada decisão registra o **porquê**.

## O que já está definido

<Em construção — definir stack, camadas, fluxos, dados e API.>

## Decisões e racional

| Decisão | Descartou | Motivo |
|---------|-----------|--------|
| Topologia: **App + Worker separados** (web e worker independentes) | Monólito único / serverless | Base de conhecimento vasta (muitas fontes); os jobs de fetch não podem travar o site |
| Agendamento: **`APScheduler`/beat** (decide *quando*) **+ RabbitMQ** (processa via fila, com `ack`/redelivery) | Só agendador embutido, sem fila | Separa o "quando" do "como processa sem perder/travar"; RabbitMQ (o usuário domina) garante robustez de entrega |
| Estado/integridade: **tabela de scheduler** no banco (last run, status) + **heartbeat do worker** | Contar só com o broker | Permite auditar e **recuperar jobs atrasados** ("deveria ter rodado e não rodou") |
| **Idempotência** no processamento | — | Redelivery do RabbitMQ não gera duplicado (buscar fonte já existente = atualizar) |
| **Banco único: PostgreSQL** (colunas relacionais + `JSONB` pro documento + `pgvector` pro RAG) | MongoDB (documento em banco separado) | Domínio relacional (conteúdo→fonte→tema N:N); RAG (pgvector) ao lado do texto; custo/backup simples (um banco só); JSONB dá a flexibilidade de documento sem segundo banco |
| **Hosting (PQ-04): Hetzner VPS** por enquanto, com **migração p/ servidor próprio (self-host) em casa** no futuro | PaaS (Fly/Render/Railway) e hospedagem compartilhada | Sempre online (sem cold start); roda web+worker+RabbitMQ+volume local; stack aberta (Docker) permite migrar "movendo a caixa" pro home-server |
| **Reverse proxy: nginx** (HTTPS via Let's Encrypt/certbot) | Caddy / Traefik | Mais usado/didático (skill transferível); serve áudio com Range; rate-limit (PQ-10); aprendizagem explícita de TLS |
| **Imagens:** baixar/armazenar as **imagens do corpo do artigo** (não só a capa), **redimensionadas (WebP)**, com o `src` reescrito para a versão local (image fetcher/rewriter) | Hotlink / só a capa | Imagens carregam informação (tabelas, gráficos, mensagens); hotlink quebra (bloqueio/CORS); uso pessoal |
| **Snapshot do feed:** XML cru **gzipado**, com **retenção limitada** (N dias, ex. 7), só para depurar/reprocessar | Guardar XML cru sem compressão/eternamente | XML comprime bem (gzip ~5–10x); retenção controla o custo de storage do VPS |
| **CI/CD: GitHub Actions** — push → lint/test → build Docker → push **GHCR** → VPS puxa e sobe (`docker compose up`); segredos no **Actions Secrets** | Deploy manual | Automatiza build/entrega; grátis no GitHub; testes/lint como gate (ferramentas definidas na Fase 6) |
| **Backend: Django** (server-rendered; admin + auth + ORM) — cadeia de deploy: `visitante → nginx (reverse proxy) → Gunicorn (WSGI) → Django` | FastAPI | Web é CRUD (o I/O pesado já está no worker); admin/auth aceleram login+cadastro (RF-05/07); deploy Django+nginx+Gunicorn é trilha batida; FastAPI fica para API pura/ML futura |
| **Frontend: monolito — Django templates + HTMX** (sem framework JS separado) | SPA em React/Vue/Next | Coerente com o Django (server-rendered); HTMX cobre interatividade (filtro por tema, player de áudio, painel); evita aprender um segundo framework JS agora |

## Perguntas em aberto

Perguntas desta fase — detalhes em [[00-perguntas-em-aberto]]:
- [[00-perguntas-em-aberto|PQ-04]] Onde hospedar + HTTPS/secrets — *Aberta*

## Definição de pronto (fase concluída)

- [ ] Stack definida e justificada
- [ ] Camadas e fluxos desenhados
- [ ] Modelo de dados e contratos esboçados

## Próximos passos

Design de módulos (Fase 4): detalhar tipos, responsabilidades e interfaces.

## Navegação

← Anterior: [[02-especificacao|Fase 2 — Especificação]] | Próxima: [[04-design-de-modulos|Fase 4 — Design de Módulos]] →
