---
name: anta-ang-battente-fm-profondita
description: "AntaArmAngBattente in PonteAngolo2024SxDx (B01380) con fm sulla profondita' Z: PosAnAng per Verso -1 ConMod 3 non deve usare X_VanoInt; risolto 30/10/2026"
metadata:
  node_type: memory
  type: project
  originSessionId: 54caacdf-f33c-48e2-b674-d61a633d06e4
  modified: 2026-10-30T13:11:47.810Z
---

AntaArmAngBattente (Armadio.inc, Vesti, ramo PonteAngolo2024SxDx, $larghezza>=1400, Verso -1, ConMod 3, caso senza 1561.5): PosAnAng era X_VanoInt, che dipende da Z, ma e' una posizione lungo X. Con fm Z 2669.5 -> 2400 l'anta si spostava di 269.5 (Lino misurava 271). Ora PosAnAng = $larghezza - 1045.5. Confermato da Lino in Metron.

Lo stesso errore resta nel blocco PntAng2024SxDx4060 (~riga 13042, Verso -1 ConMod 3): NON toccato, non segnalato da Lino.

**Why:** nel ponte Verso -1 X e' l'ala lunga, Z la profondita' dell'altra ala; X_VanoInt segue Z.
**How to apply:** se un fm sulla profondita' sposta ante angolo, cercare PosAnAng calcolato da X_VanoInt. Debug utile: FILE_APPEND con cod, Zpadre, larg, Pos. Vedi [[mensola-sottoponte-vano1]] e [[bdm-file-ansi-no-edit]].
