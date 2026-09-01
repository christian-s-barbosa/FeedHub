# Configuração do board — criar-card-kanban

## Projeto
- **Repo:** `christian-s-barbosa/FeedHub`
- **Projeto (board):** número `1`, id `PVT_kwHOEJaIWM4BiHd6`
- **Dono (assignee sempre):** `christian-s-barbosa`
- **PATH gh:** `C:\Program Files\GitHub CLI\gh.exe`

## Campos do projeto (IDs)
- **Status:** `PVTSSF_lAHOEJaIWM4BiHd6zhhAp5M` — opções: `Todo`, `In Progress`, `Test`, `Done`
- **Due date:** `PVTF_lAHOEJaIWM4BiHd6zhhCSoU` (DATE)
- **Priority:** `PVTSSF_lAHOEJaIWM4BiHd6zhhCT3o` (SINGLE_SELECT) — opções: `Alta`, `Média`, `Baixa`

> Se algum ID mudar (recriar o board/campo), re-execute:
> `gh project field-list 1 --owner christian-s-barbosa` e atualize aqui.

## Convenções
- **Milestone por fase:** `Fase N — <nome>` (ex.: `Fase 1 — Levantamento de Requisitos`).
- **Épico (issue pai) por fase:** `Fase N — <nome>`, label `fase-N`.
- **Labels:** `fase-N` (fase) + `contexto` (abaixo). Labels são de repo (criar se não existirem).

| Fase | Label contexto |
|------|----------------|
| 1 | `levantamento` |
| 2 | `especificacao` |
| 3 | `arquitetura` |
| 4 | `design` |
| 5 | `desenvolvimento` |
| 6 | `testes` |
| 7 | `deploy` |
| 8 | `manutencao` |

## Como obter o ID de uma opção de Priority
```powershell
$q = 'query { node(id: "<PROJECT_ID>") { ... on ProjectV2 { field(name:"Priority") { ... on ProjectV2SingleSelectField { options { id name } } } } } }'
gh api graphql -F query=@arquivo  # ou inline
```
Guarde o `id` da opção (`Alta`/`Média`/`Baixa`) para usar no `item-edit`.

## Criar issue via REST (dica: acento no body/título — enviar bytes UTF-8)
```powershell
$body = @{ ... } | ConvertTo-Json
$bytes = [System.Text.Encoding]::UTF8.GetBytes($body)
Invoke-RestMethod -Method Post -Uri ".../issues" -Headers $h -Body $bytes -ContentType "application/json; charset=utf-8"
```
