---
name: github-sagomemetron
description: "Repository GitHub privato SagomeMetron con l'editor sagoma esterno (solo l'editor, mai il BDM); docs al posto della wiki; come fare il push"
metadata:
  node_type: memory
  type: reference
  originSessionId: b6fffd0d-c5a6-43c4-aa36-b68db2043d5b
  modified: 2026-11-03T15:32:03.425Z
---

Repository: https://github.com/llazzaretti77-sudo/SagomeMetron (privato, branch main). Cartella locale: C:\Tesy\Moretti\BdmSviluppo\_generatori_profilo\editor (EditorSagoma.cs, Woodwop.cs, EditorSagoma.ini di partenza, build_editor.ps1 -Out, README.md, docs\ 8 pagine, esempi\). Caricato il 2026-11-03 (commit c88dbe6 + 3bc7f9c).

- Lino vuole su GitHub SOLO l'editor esterno a Metron, NON il codice BDM (la cartella BdmSviluppo non e' un repo git: non crearne uno).
- Wiki GitHub non disponibile (repo privato, piano Free): la documentazione sta in docs\ e va aggiornata nello stesso commit quando cambia l'editor.
- Git = quello di Visual Studio 2022: C:\Program Files\Microsoft Visual Studio\2022\Professional\Common7\IDE\CommonExtensions\Microsoft\TeamFoundation\Team Explorer\Git\cmd\git.exe (niente gh). Il push dal PowerShell dello strumento fallisce (non puo' chiedere le credenziali): lanciarlo nel terminale dell'app (run_in_terminal), il login GitHub si apre nel browser.
- .gitattributes `* -text` (ini/mpr in cp1252), .gitignore esclude gli .exe.

Vedi [[pannello-sagomato]].
