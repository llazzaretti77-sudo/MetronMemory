---
name: guida-linguaggio-bdm
description: "Guida al linguaggio BDM (PDF 29 pagine, 05/10/2026) scritta per Lino: dove sta, versione testo consultabile, cosa contiene"
metadata:
  node_type: memory
  type: reference
  originSessionId: feecbae4-c78f-4961-97f4-ea297a175271
  modified: 2026-10-07T07:58:53.081Z
---

PDF: D:\metron\DocPerLino\Guida al linguaggio BDM.pdf (29 pagine, export di un documento del 05/10/2026, "@Lino").
Versione testo per consultarla/cercarla (Read non apre i PDF qui, manca poppler): D:\metron\DocPerLino\Guida al linguaggio BDM.txt
(estratta con C:\Tesy\Moretti\BdmSviluppo\_generatori_profilo\pdf_in_testo.ps1; tabelle a colonne incollate, testo leggibile).

Fonti della guida: codice del progetto Moretti (BdmSviluppo, ~280.000 righe) + wiki Tesy (accesso riservato). Le parti "da verificare" sono dedotte dal codice.

Contenuti, in ordine: struttura del progetto e di Moretti.bdm (sezioni Globali, Macro, Messaggi, Misure, Elemento, Subroutine, Funzioni, Connessioni, Altimetri); preprocessore (#include, #ifdef TEC/AZI/AGE/RIV);
Elemento (intestazione, ordine dei blocchi, sequenza di esecuzione Assegnazioni->PuntiNotevoli->Posiziona->PuntiNotevoli->Controlli->Gestionale);
Variabili (modi con costanti $MODE_*: Utente, Menu, TabStringa, NotIn, Boolean, NoSave, ShowAlways, SaveAlways...; tag < Grp=, Min=, Max= >; prefissi X, @X, $x, #x, VAR_VAL);
geometria (box base_0..alto_3, Z verso il fronte, forme Blocco/XYZSweep/PathSweep/Cilindro/Import); Posiziona ed escluso; Attrezza/Vesti/Riempi, vani e altimetri/OnSetupLivelli;
controllo di flusso (solo if/else e while, EXITSUB/EXIT, EVAL); procedure e funzioni utente; tabella delle funzioni di sistema; comandi COMANDO/COMANDO2 (SETTA, ELEMENTO_*, ATTREZZA2, VAR_CHANGE, RICALCOLA_UNA, DELETE1, PROCEDURA, RUN_DLL_CMD...);
macro di sistema (eventi On*), valori di ACTION(); menu .mnu e dialog BdmDialog (widget, OnClick, wizard a pagine); esempio completo MobileBase + RipianoMobileBase; buone pratiche; debug (trace in metron.ini, Ctrl+Alt+M, LOGMSG, TRX).

Da usare come riferimento rapido prima di scrivere codice BDM; le lezioni pratiche emerse dopo (piano profilo) sono in [[bdm-lezioni-tecniche]] e completano la guida.
