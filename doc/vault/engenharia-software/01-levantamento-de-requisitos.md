---
fase: 1
titulo: Levantamento de Requisitos
data: 2026-09-02
status: validado
milestone: Fase 1 — Levantamento de Requisitos (#1)
---

# Fase 1 — Levantamento de Requisitos

> **Status:** rascunho | validado
> **Fase:** 1 de 8

## Kanban

| Card | Título | Status |
|------|--------|--------|
| #1 | Fase 1: Levantar requisitos funcionais (RF) | Done |
| #4 | Fase 1: Levantar requisitos não funcionais (RNF) | Done |
| #2 | Fase 1: Responder perguntas em aberto | Done |
| #3 | Fase 1: Validar doc de requisitos | Done |
| #6 | Fase 1 — Levantamento de Requisitos (épico) | Milestone are done |

## Objetivo

Entender o que o FeedHub precisa fazer: agregar, organizado por **temas**,
informação de três fontes distintas (notícias via RSS, discussões via Reddit e
posts via Bluesky), para **uso pessoal** e demonstração no **portfólio** — e
separar o essencial (MVP) do secundário.

## O que já está definido

### Requisitos Funcionais (RF)

| ID | Requisito | Prioridade |
|----|-----------|------------|
| RF-01 | Agregar notícias via RSS de fontes curadas (artigos completos) | MVP |
| RF-02 | Buscar discussões do Reddit relacionadas aos temas | MVP |
| RF-03 | Listar postagens iniciais de perfis seguidos no Bluesky (sem respostas) | MVP |
| RF-04 | Organizar por temas; cada tema exibe as 3 seções filtradas (notícias / discussões / posts) | MVP |
| RF-05 | Cadastrar tema e fonte individualmente, pela interface | MVP |
| RF-06 | Cadastrar temas/fontes em massa via CSV | Depois |
| RF-07 | Login do dono (acesso + edição de temas/fontes) | MVP |
| RF-08 | Emitir token temporário read-only para visitantes | Depois |
| RF-09 | Publicar o site em acesso público (dono sempre + visitantes via token) | MVP |
| RF-10 | Ouvir os textos como áudio: TTS gerado no backend + player próprio dentro do app/site | MVP |
| RF-11 | Persistir o texto baixado das fontes em dois bancos: PostgreSQL (principal) + banco vetorial (para RAG futura) | Depois |

### Requisitos Não Funcionais (RNF)

| ID | Requisito | Tipo | Prioridade |
|----|-----------|------|------------|
| RNF-01 | Conteúdo atualizável por fonte (campo dinâmico). Padrão: Bluesky a cada 1h, Reddit a cada 1 dia, RSS manhã e noite | Desempenho/Frescura | MVP |
| RNF-02 | Sem autenticação, sem acesso (proteção por login/token) | Segurança | MVP |
| RNF-03 | Interface responsiva (adapta-se ao mobile) | Usabilidade | MVP |
| RNF-04 | Token com expiração e restrito a leitura | Segurança | MVP |
| RNF-05 | Site online constantemente (disponibilidade contínua) | Disponibilidade | MVP |
| RNF-07 | Nunca ultrapassar os limites de uso do free tier | Custo | MVP |
| RNF-08 | Segurança de segredos: tokens e chaves de API nunca versionados, nem expostos no front-end ou em logs/respostas; geridos via variáveis de ambiente / secrets do hosting | Segurança | MVP |

## Decisões e racional

| Decisão | Descartou | Motivo |
|---------|-----------|--------|
| Frente social via Bluesky | X (Twitter) e Threads | API pública/gratuita (AT Protocol); X é paga e Threads é restrita à aprovação da Meta |
| Organização por temas, com 3 seções separadas | Timeline única misturando tudo | Evita confusão entre tipos diferentes de conteúdo |
| Acesso: dono sempre + token temporário read-only | Login público aberto | Uso pessoal; permite demonstrar a terceiros por um período |
| Deploy público sempre-no-ar | Rodar apenas local | Necessário para uso contínuo e para o portfólio ser acessível |
| Vínculo tema ↔ fonte: N:N (uma fonte serve a vários temas) | 1:N (fonte pertence a um só tema) | Flexibilidade: o mesmo site/perfil/subreddit alimenta vários temas |

## Perguntas em aberto

Resolvidas:
- ~~Frequência de atualização~~ → RNF-01 (Bluesky 1h, Reddit 1 dia, RSS manhã+noite).
- ~~Vínculo tema ↔ fonte~~ → decidido: fonte pertence a vários temas (N:N).

Adiadas para as próximas fases:
- Esquema/validação do CSV → Design (Fase 4); RF-06 é Depois.
- Onde hospedar + HTTPS/secrets → Arquitetura/Deploy (Fases 3 e 7).
- Duração/UX do token → Especificação/Arquitetura (Fases 2 e 3).

## Definição de pronto (fase concluída)

- [x] Requisitos RF e RNF listados e priorizados (MVP definido)
- [x] Perguntas em aberto — as de requisito respondidas; as técnicas adiadas para as fases seguintes

## Próximos passos

Especificação (Fase 2): transformar cada RF em critérios de aceite mensuráveis e
cada RNF em uma medida verificável.

## Navegação

→ Próxima: [[02-especificacao|Fase 2 — Especificação]]
