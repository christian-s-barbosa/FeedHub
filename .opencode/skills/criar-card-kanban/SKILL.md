---
name: criar-card-kanban
description: Use when creating a new card on the FeedHub kanban board or adding a task as an issue/card (user says "criar card", "nova task", "adicionar no kanban"). Standardizes creation: confirm preview first, assignee, due date, priority, labels, milestone, parent épico, structured body. Reads config from .opencode/skills/criar-card-kanban/config.md. Trigger keywords: criar card, nova tarefa, card kanban, board, nova issue.
---

# Criar card no Kanban do FeedHub

Crie um card no board seguindo o padrão do projeto. **Nunca crie sem confirmação prévia do usuário.**

Leia `config.md` (mesma pasta desta skill) para IDs, caminhos e convenções. Se os IDs não baterem, verifique com `gh project field-list 1 --owner christian-s-barbosa` e atualize o `config.md`.

## Passo 1 — Coletar os dados (uma coisa por vez)
Pergunte ao usuário (não assuma):
- **Fase** (1–8) e **título** curto e claro do card.
- **Objetivo** (1 linha: o que o card entrega).
- **Prioridade** (`Alta` / `Média` / `Baixa`).
- **Prazo** (opcional, `YYYY-MM-DD`).
- **RF vinculado** (opcional, ex.: `RF-01`) — vai no corpo.
- **Label de contexto** (default pela fase, ver `config.md`).

## Passo 2 — Mostrar prévia e aguardar OK (trava)
Apresente ao usuário esta prévia em texto e **espere a resposta OK** antes de criar:
```
Fase: 1 — Levantamento de Requisitos
Título: <título>
Responsável: christian-s-barbosa (fixo)
Prioridade: <Alta|Média|Baixa>
Prazo: <YYYY-MM-DD | sem>
Labels: fase-1, levantamento
Milestone: Fase 1 — Levantamento de Requisitos
Épico pai: Fase 1 — Levantamento de Requisitos
RF: <RF-0x | sem>
```
Só prossiga após o ok. Se o usuário pedir mudança, ajuste e mostre de novo.

## Passo 3 — Garantir dependências (labels, milestone, época)
1. **Labels de repo** (criar se não existirem):
   `gh label create "fase-N" --repo christian-s-barbosa/FeedHub` e o de contexto, se faltarem.
2. **Milestone da fase**: crie via REST se não existir:
   `POST /repos/{owner}/{repo}/milestones  {"title":"Fase N — <nome>"}` — guarde o `number`.
3. **Épico (issue pai)**: se não existir uma issue `Fase N — <nome>`, crie-a (label `fase-N`). Guarde o `node_id` e `html_url` dela.

## Passo 4 — Criar o card (issue)
Crie a issue via REST (use **bytes UTF-8** — ver `config.md`), com:
```json
{
  "title": "<título>",
  "body": "## Fase\n<Fase N — nome>\n\n## Objetivo\n<objetivo>\n\n## Documento de referência\n<doc/vault/engenharia-software/0N-....md>\n\n## Requisito vinculado\n<RF-0x | —>\n\n## Definição de pronto\n- [ ] ...\n- [ ] ...\n\n## Prazo\n<YYYY-MM-DD | —>",
  "assignees": ["christian-s-barbosa"],
  "labels": ["fase-N", "<contexto>"],
  "milestone": <number da fase>
}
```
Guarde `html_url` e `node_id` da issue criada.

## Passo 5 — Adicionar ao board + setar campos do projeto
```powershell
gh project item-add 1 --owner christian-s-barbosa --url <html_url>
gh project item-edit --id <item_id> --project-id PVT_kwHOEJaIWM4BiHd6 --field-id PVTF_lAHOEJaIWM4BiHd6zhhCSoU --date <YYYY-MM-DD>        # Due date (se houver)
gh project item-edit --id <item_id> --project-id PVT_kwHOEJaIWM4BiHd6 --field-id PVTSSF_lAHOEJaIWM4BiHd6zhhCT3o --single-select-option-id <opt_id>  # Priority
```
- `item_id` é o retorno do `item-add` (formato `PVTI_...`).
- `opt_id` é o ID da opção `Alta`/`Média`/`Baixa` (ver `config.md`).

## Passo 6 — Vincular como sub-issue do época (relação)
Se/quando o GitHub expuser a mutação adequada e o usuário pedir, unirá o card como filho do `épico` (GraphQL `addSubIssue`). Ok não sugerir vínculo se não estiver disponível — avisar o usuário.

## Passo 7 — Confirmar e, se pedido, ajustar Status
Mostre o card criado (`html_url`), o assignee, prazo e prioridade, e resuma os campos. Não mude o Status para "Done" sozinho — isso segue a regra doc ↔ board (fase validada).

## Regras
- Sempre **confirme a prévia** (Passo 2) antes de criar.
- **Assignee sempre** o dono; nunca outro, salvo instrução explícita.
- Não exponha tokens; use `gh auth token` internamente, nunca imprima.
- Execução: se um passo falhar, pare e informe o erro ao usuário — não registre como criado.
