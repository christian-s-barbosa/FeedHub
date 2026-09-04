# Configuração — gerenciar-card-kanban

## Projeto
- **Repo:** `christian-s-barbosa/FeedHub`
- **Projeto (board):** número `1`, id `PVT_kwHOEJaIWM4BiHd6`
- **Dono:** `christian-s-barbosa`
- **PATH gh:** `C:\Program Files\GitHub CLI\gh.exe`

## Campos do projeto (IDs)
- **Status:** `PVTSSF_lAHOEJaIWM4BiHd6zhhAp5M`
  - `Todo` = `f75ad846`
  - `In Progress` = `47fc9ee4`
  - `Test` = `30302c9a`
  - `Done` = `98236657`
- **Due date:** `PVTF_lAHOEJaIWM4BiHd6zhhCSoU`
- **Priority:** `PVTSSF_lAHOEJaIWM4BiHd6zhhCT3o` — `Alta`=`e2b73176`, `Média`=`85296720`, `Baixa`=`be57f6e7`

## Como achar o item de um card (issue → item id)
```powershell
gh project item-list 1 --owner christian-s-barbosa --format json
# procura pelo "number" da issue (issue #N) e usa o "id" (PVTI_...) do item
```

## Milestone por fase
`Fase N — <nome>`; criar se não existir:
`POST /repos/{owner}/{repo}/milestones  {"title":"Fase N — <nome>"}` (REST, bytes UTF-8 se tiver acento).

## Labels por fase
`fase-N` + contexto (ver `.opencode/skills/criar-card-kanban/config.md`).
