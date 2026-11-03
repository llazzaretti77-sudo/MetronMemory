---
name: ripiano-prod-doppio-cabine
description: "Prezzo cabine +96/+192 (es. B00537 1207 vs PDF 1100): RipianoProdArmaBat con radice anche se il padre non e' CodVendita; fix in Armadio.inc 28/10/26"
metadata:
  node_type: memory
  type: project
  originSessionId: 3c00ebdb-f13f-445f-8648-b12b8d4533f0
  modified: 2026-10-28T14:08:43.177Z
---

2026-10-28: prezzi cabine (ANGOLI E CABINE H.226/H.258) non tornavano col PDF: ripiano 626x401,5 (SAP 080007333, 96 EUR) contato doppio, RipianoArmaBat + RipianoProdArmaBat.

Causa: nel Gestionale di `Elemento RipianoProdArmaBat` (Armadio.inc ~24665) il blocco `UN_PADRE("Cabina*") & X==626 & Z==401.5 -> perRADICE="CAB_RIP_SQ"` non controllava CodVendita del padre, annullando PosaCodiceProduttivo (Cameretta.inc 4066: figlio hide + perRADICE vuoto se padre CodVendita=0).

Fix: aggiunto `& VAR_VAL(idx_pdr,"CodVendita")>0`. Confermato funzionante da Lino. Backup `.bak\Armadio.inc.bak_20261028_ripprod`.

**Why:** regola di Lino: il figlio Prod porta il codice solo se il padre e' codice di vendita.
**How to apply:** se un prezzo e' maggiorato di multipli esatti del prezzo di un pannello, cercare figli "Prod" con radice forzata senza controllo CodVendita. Vedi [[verifica-prezzi-listino]] e [[bdm-file-ansi-no-edit]].
