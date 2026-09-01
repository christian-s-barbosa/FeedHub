---
fase: 1
titulo: Levantamento de Requisitos
data: 2026-09-02
status: rascunho
---

# Fase 1 — Levantamento de Requisitos

> **Status:** rascunho | validado
> **Fase:** 1 de 8

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
| RF-06 | Cadastrar temas/fontes em massa via CSV | MVP |
| RF-07 | Login do dono (acesso + edição de temas/fontes) | MVP |
| RF-08 | Emitir token temporário read-only para visitantes | MVP |
| RF-09 | Publicar o site em acesso público (dono sempre + visitantes via token) | MVP |

### Requisitos Não Funcionais (RNF)

| ID | Requisito | Tipo |
|----|-----------|------|
| RNF-01 | O conteúdo se mantém atualizado; frequência de refresh a definir | Desempenho/Frescura |
| RNF-02 | Sem autenticação, sem acesso (proteção por login/token) | Segurança |
| RNF-03 | Navegação fluida também em mobile | Usabilidade |
| RNF-04 | Token com expiração e restrito a leitura | Segurança |
| RNF-05 | Acesso público contínuo (site sempre no ar) | Disponibilidade |

## Decisões e racional

| Decisão | Descartou | Motivo |
|---------|-----------|--------|
| Frente social via Bluesky | X (Twitter) e Threads | API pública/gratuita (AT Protocol); X é paga e Threads é restrita à aprovação da Meta |
| Organização por temas, com 3 seções separadas | Timeline única misturando tudo | Evita confusão entre tipos diferentes de conteúdo |
| Acesso: dono sempre + token temporário read-only | Login público aberto | Uso pessoal; permite demonstrar a terceiros por um período |
| Deploy público sempre-no-ar | Rodar apenas local | Necessário para uso contínuo e para o portfólio ser acessível |

## Perguntas em aberto

- Frequência de atualização de cada fonte (RSS / Reddit / Bluesky)?
- Esquema/validação do CSV (colunas, formatos, como vincular a um tema)?
- Onde hospedar (opções com free tier?) e como configurar HTTPS/secrets?
- Fluxo de emissão/expiração do token (duração padrão, UX de gerar)?
- Como um "tema" é definido e como as fontes/perfis/subreddits se vinculam a ele?

## Definição de pronto (fase concluída)

- [ ] Requisitos RF e RNF listados e priorizados (MVP definido)
- [ ] Perguntas em aberto levantadas

## Próximos passos

Especificação (Fase 2): transformar cada RF em critérios de aceite mensuráveis e
cada RNF em uma medida verificável.

## Navegação

→ Próxima: [[02-especificacao|Fase 2 — Especificação]]
