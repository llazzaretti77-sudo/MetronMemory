---
name: mensola-sottoponte-vano1
description: "MensolaSottoPonteNEW nel vano 1 di PonteAngolo2024SxDx (B01380): regola lunghezza/posizione, risolto 29/10/2026"
metadata:
  node_type: memory
  type: project
  originSessionId: 2b244fac-9be7-45a4-85dc-a2130b73c0c0
  modified: 2026-10-30T08:26:27.167Z
---

MensolaSottoPonteNEW (Armadio.inc, Attrezza) nel vano 1 di PonteAngolo2024SxDx con Verso -1 (dal 30/10 per ogni ConMod, aggiunto dopo B01328 ConMod 0; da confermare): con ConMod 2/3 il vano 1 finisce prima della parete (vano 5 modulo dopo), quindi X = Z ponte - Z mensola - 25 e base_0.x = $vano_0.x + ZPnt - X - $vano_0.z. Per gli altri ponti angolo il vano 1 e' limitato a larghezza vano (max 1027,5). Il vano 0 non va limitato (X=X-25 voluto). Confermato da Lino in Metron.

Vano 0 (mensola in fondo, tutta parete): lunghezza = X ponte - 25, base_0.x = accorcia_sx (parte da 0) anche con ConMod 1/3 (il vano parte dopo il modulo ma la mensola no). Detto da Lino 30/10 dopo B01332; la prima versione "X-18-vano_0.x" era sbagliata. Da confermare.

Verso +1 (B01363, 30/10, CONFERMATO da Lino): ponte e' specchio di Verso -1. Solo vani 1 e 4 (query esclude gli altri). Vano 1: X = Z ponte - Z mensola - 25, base_0.x = accorcia_sx. Vano 4 (-90): X = X ponte - 25, parte da z=25 (arriva al fianco; con z=0 mancavano 25 al fianco): base_0.x = $vano_0.x + 25 - $vano_0.z, base_0.z = $vano_0.z + 7 (con +18 dedotto per specchio c'erano 11 mm di distacco dalla schiena: misurato da Lino).

PntAng2024SxDx4060 (30/10, Verso -1 CONFERMATO da Lino; in BDM niente parentesi con piu' confronti NOME in OR -> "bad expression format", usare flag semplici): non si attrezzava perche' Attrezzabile aveva solo Pont*; aggiunto in lista e applicate le regole Verso -1 (vano 1 e vano 0, per vano 0 qualsiasi ConMod perche' parte a x=25). Verso +1 NON gestito: li' vano 4 ha ang 0 (geometria diversa dal padre).

**Why:** X_ini = Z-25 (mensola a tutta parete) usciva dall'angolo; la mensola del vano 0 occupa z 25..235.
**How to apply:** per posizioni su vani ruotati -90 base_0.x e' offset lungo il vano da $vano_0.x; il debug utile e' FILE_APPEND su $install_dir (variabili inesistenti come ang_y danno errore). Vedi [[bdm-file-ansi-no-edit]].
