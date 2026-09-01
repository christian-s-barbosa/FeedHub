---
name: gerenciar-card-kanban
description: Use when managing/updating kanban cards of a phase or syncing the vault docs with the board (user says "iniciar fase", "mover status do card", "fechar fase", "marcar card como done", "sincronizar board com doc"). Creates phase milestone, moves card status, and closes a phase (doc validado -> cards Done + card refs added to the .md). Reads config from .opencode/skills/gerenciar-card-kanban/config.md. Trigger keywords: iniciar fase, milestone, mover status, fechar fase, card done, sincronizar kanban, board.
---

# Gerenciar cards do Kanban (doc ↔ board)

Cuida do ciclo de vida dos **cards médios** de uma fase e mantém o `.md` da fase em sincronia com o board. Leia `config.md` (mesma pasta) para IDs e convenções.

## Padrão de sincronia
- **Inicio de fase** → cria a **milestone** da fase e registra no `.md` (frontmatter `milestone:` + seção `## Kanban`).
- **Durante a fase** → cards médios aparecem na tabela `## Kanban`, cada um com seu **status** (Todo → In Progress → Done).
- **Fim de fase** (doc `validado`) → todos os cards da fase → **Done** e as **referências dos cards** preenchidas na tabela.

## Operação: iniciar fase
1. Confirmar com o usuário: fase (N) e nome.
2. **Criar a milestone** (se não existir) via REST → guardar o `number`.
3. **Listar os cards médios da fase** (issues com label `fase-N`) e seus status.
4. Atualizar o `.md` da fase:
   - frontmatter: `milestone: Fase N — <nome> (#<n>)`
   - seção `## Kanban`: tabela `Card | Título | Status` com os cards da fase.
5. Mostrar prévia e **aguardar OK** (trava) antes de gravar.

## Operação: mover status de um card
1. Confirmar o card (issue `#N`) e o novo **status** (In Progress / Done / etc.).
2. Achar o item id no board (`gh project item-list ... --format json`, casando pelo `number`).
3. `gh project item-edit --id <item_id> --project-id PVT_kwHOEJaIWM4BiHd6 --field-id PVTSSF_lAHOEJaIWM4BiHd6zhhAp5M --single-select-option-id <status_id>`
4. Atualizar a linha do card na tabela `## Kanban` do `.md`.
5. Confirmar o resultado.

## Operação: fechar fase (doc → validado)
1. Confirmar que todos os checks da **Definição de pronto** do `.md` estão marcados (e as perguntas resolvidas).
2. Mover **todos** os cards da fase para **Done** (loop da operação anterior).
3. Preencher as **referências dos cards** (números) na tabela `## Kanban` do `.md`.
4. Setar o frontmatter `status: validado` no `.md`.
5. Confirmar e sugerir atualizar a tela do Obsidian.

## Regras
- **Sempre confirme** antes de mover status / fechar fase / gravar no `.md`.
- Fechar fase exige que a fase esteja realmente concluída (não marcar Done à toa).
- Não use secrets em texto; token via `gh auth token` internamente.
- Se um passo falhar, pare e informe; não registre como concluído.
