---
name: bdm-file-ansi-no-edit
description: "MAI usare gli strumenti Edit/Write sui file BDM (ANSI 1252): distruggono le lettere accentate; modificare solo con script Python/PowerShell in cp1252; come riconoscere e riparare il danno"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 6ec55581-eedb-4e7b-95c1-57774d8b8837
  modified: 2026-10-15T06:48:06.609Z
---

I file BDM del progetto (Moretti.bdm, *.inc, Dialog*.txt...) sono in ANSI Windows-1252 con CRLF. Gli strumenti Edit/Write li leggono come UTF-8: al salvataggio ogni lettera accentata diventa U+FFFD (byte EF BF BD; Lino lo vede come "modalitÃ¯Â¿Â½"), e alcune righe vicine possono finire in UTF-8 valido ("piÃ¹"). Il codice continua a funzionare, quindi il danno passa inosservato.

Danni trovati e riparati (12-13/10/2026):
- Moretti.bdm: 146 righe + 13 in UTF-8 (mie modifiche dal 9/10), copia sana C:\Tesy\Moretti\Bdm\Moretti.bdm del 7/10 -> 171 byte >= 0x80 come la copia sana.
- Armadio.inc: 504 righe (danno del 9/10 15:23), copia sana BdmSviluppo\Armadio.inc.bak_20261005_ctrlfm -> 519.
- Cabina.inc: 60 righe (danno del 6/10 11:07), copia sana BdmSviluppo\Cabina.inc.bak_20261005 -> 63.
- Scansione di 120 file della collezione in BdmSviluppo (.inc .bdm .txt .mnu .ini .dat, escluse LibOLD e _analisi_prezzi): nessun altro danno.
- Ancora rovinati e NON toccati (in attesa di conferma di Lino): C:\Tesy\Moretti\Bdm\Armadio.inc e Cabina.inc (copie identiche di quelle danneggiate). BdmSviluppo\Moretti.bdm.bak del 12/10 14:04 e' rovinato.
- Da far confermare a Lino: in Armadio.inc manca la riga del 5/10 `if( UN_PADRE("Cabina*")) {TpVanoSl = 0  }` (riga ~26746), senza accenti, quindi tolta dopo il 5/10 e non legata al danno.

Riparazione (script ripara_accenti.py, da riscrivere se serve): leggere i due file in byte e dividere su CRLF; per ogni riga del file rovinato che contiene EF BF BD, sostituire ogni EF BF BD con un segnaposto e cercare nella copia sana le righe con lo stesso "scheletro" (i caratteri >= 0x80 sostituiti dallo stesso segnaposto); se il candidato e' unico, rimettere la riga sana. Seconda passata: righe con C3 + 80..BF -> decode utf-8, encode cp1252, accettate solo se la riga risultante esiste nella copia sana. Prova a vuoto prima, copia del file rovinato nello scratchpad, poi scrittura. Controllo finale: nessun EF BF BD, nessuna sequenza C3 80..BF (contare sui byte, non con regex sulla stringa esadecimale: da' falsi positivi), byte >= 0x80 uguali alla copia sana, nessun LF senza CR, elenco delle righe diverse dalla copia sana per verificare che siano solo modifiche volute.

Script di modifica sicura usato dal 15/10 (bdm_sostituisci.py, nello scratchpad: riscriverlo se manca): args file.bdm vecchio.txt nuovo.txt (testi UTF-8 convertiti in cp1252+CRLF); il vecchio deve comparire 1 volta; rifiuta se cambiano i byte >= 0x80 attesi, se compare EF BF BD o LF senza CR. Prima di ogni modifica copia del file nello scratchpad.

**Why:** la lettera originale non e' ricavabile dal file rovinato: serve una copia sana (backup .bak_AAAAMMGG in BdmSviluppo, cartella C:\Tesy\Moretti\Bdm_20261006\).
**How to apply:** per modificare file BDM usare uno script Python (open(f,'rb'), testo cp1252, scrivere 'wb' mantenendo CRLF) che sostituisce testo vecchio -> nuovo controllando che il vecchio compaia una volta sola; dopo ogni modifica gli stessi controlli di sopra (byte >= 0x80 invariati salvo accenti voluti). Vedi [[bdm-lezioni-tecniche]] e [[verifica-prezzi-listino]].
