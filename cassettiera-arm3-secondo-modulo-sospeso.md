---
name: cassettiera-arm3-secondo-modulo-sospeso
description: "CassettieraInternaArm3 non attrezzabile nel 2o ArmadioBattenteSospeso affiancato: causa FondoArmaBatSosp lungo, risolto 30/10"
metadata:
  node_type: memory
  type: project
  originSessionId: 3fc7d64c-e6d2-4e7c-8fee-349b5831aa48
  modified: 2026-10-30T14:55:25.909Z
---

Con due ArmadioBattenteSospeso affiancati la base del 1o modulo si allunga (X_Fnd_Fly, esc_fnd_Fly=1 sul 2o) e il vano del 2o non proponeva CassettieraInternaArm3 (il ripiano sì).

**Causa:** FondoArmaBatSosp (alias Scocca;Ponte) copre anche il 2o modulo e in OnQueryBdmLocal query_type==2 accettava la cassettiera sul suo vano 0 (rifiutava solo RipianoArmaBat). Il fondo non ha AttrFLYFondo, quindi nessun livello da OnSetLiv_CasIntArmFLY.

**Fix (30/10/26, confermato funzionante da Lino):** in FondoArmaBatSosp, dopo la riga `$query_p2==1 & HAS_ALIAS(...RipianoArmaBat)`, aggiunto `if( NOME($query_p1)=="CassettieraInternaArm3" ) { $QueryResult__ = 0 }`. Backup: .bak\Armadio.inc.bak_20261030_cassfondo.

**How to apply:** se un altro attrezzo non compare nel 2o modulo di una coppia sospesa, controllare se il vano del fondo lungo lo sta accettando in query_type==2. Vedi [[bdm-file-ansi-no-edit]] per la modifica del file.
