---
name: no-metron-ini
description: Non scrivere mai nel metron.ini (niente WRITE_INI per debug/output); usare file nuovi
metadata:
  node_type: memory
  type: feedback
  originSessionId: 6ec55581-eedb-4e7b-95c1-57774d8b8837
  modified: 2026-10-09T14:55:14.567Z
---

Non usare WRITE_INI (o altro) per scrivere dati di debug/output nel metron.ini: e' un file di configurazione importante. Scrivere su un file nuovo.

**Why:** richiesta esplicita di Lino il 2026-10-09 quando avevo fatto scrivere DebugPrezzi nel metron.ini.
**How to apply:** per output su file da BDM usare FILE_APPEND(file, testo) (funzione trovata in Motore.dll, non documentata; CONFERMATO funzionante 2026-10-09, aggiunge da sola il fine riga; FILE_DEL per azzerare). Vedi [[verifica-prezzi-listino]].
