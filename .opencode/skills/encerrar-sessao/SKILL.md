---
name: encerrar-sessao
description: Use when a mentoring session ends (user says "/ends", "fim de sessão", "encerrar", "até logo" or asks to registrar progresso). Guides the user to fill the reflexão of the session and saves a dated copy of PROGRESSO.md to doc/vault/historico progressao/. Trigger keywords: fim de sessão, encerrar, progresso, historico progressao, registrar progresso.
---

# Encerrar sessão — registrar progresso

Ao **fim de cada sessão de mentoria**, registre o progresso seguindo este fluxo.
Você, mentor, aplica os campos via perguntas (ver "Postura pedagógica" no
AGENTS.md) — o usuário responde; você registra. Não invente respostas.

## Passos

1. **Ler o template.** Leia `doc/vault/tamplates/PROGRESSO.md` para se basear na estrutura.
2. **Perguntar, uma coisa por vez.** Entreviste o usuário para preencher:
   - Auto-avaliação de autonomia (nível 1–5 + justificativa);
   - Avaliação do mentor (nível + discordância, se houver);
   - Reflexão da sessão (aprendi, travei, etapa mais longa, mais difícil,
     fiz sozinho, faria diferente);
   - Objetivo para a próxima sessão (foco + como medir).
3. **Criar a cópia datada.** Gere o conteúdo do `PROGRESSO.md` preenchido e
   salve no caminho:
   ```
   doc/vault/historico-progressao/YYYY_mm_dd_Progresso.md
   ```
   - `YYYY_mm_dd` = data de hoje (ex.: `2026_09_01_Progresso.md`).
   - Use o formato de data local no início do arquivo.
4. **Evitar sobrescrever.** Se o arquivo do dia já existir, anexe um sufixo:
   `YYYY_mm_dd_Progresso_2.md`, `_3`, etc.
5. **Confirmar.** Mostre o caminho salvo e um resumo de 1 linha do que ficou
   registrado. Não altere o `PROGRESSO.md` mestre (é o template).

## Estrutura do arquivo gerado

Siga a estrutura do template `PROGRESSO.md`, substituindo os campos `___` pelas
respostas. Mantenha o título `# Progresso — Mentor Sênior (FeedHub)` e a data.
