---
name: verifica-prezzi-listino
description: "Progetto confronto prezzi listino PDF Camerette 2026 vs configuratore Metron; dove stanno i prezzi (MorettiSap), come leggere il PDF, procedure DebugPrezzi/SalvaPrezzoPreventivo"
metadata:
  node_type: memory
  type: project
  originSessionId: 6ec55581-eedb-4e7b-95c1-57774d8b8837
  modified: 2026-10-15T06:48:03.933Z
---

Obiettivo (dal 2026-10-09): verificare che i prezzi del listino PDF coincidano con quelli del configuratore grafico Metron. Prova iniziale solo su B00500.

- PDF: D:\Listino_MorettiCompact_Camerette 2026_IT.pdf (634 pag., 170 MB, cifrato AES-256 solo permessi). Si legge con Python + pypdf + cryptography (installati con --user il 2026-10-09; NON usare python -I, esclude i pacchetti utente). Pagina stampata = pagina PDF - 2 (B00500 a pag. stampata 146 = PDF 148). Read tool e API WinRT Windows.Data.Pdf falliscono su questo file.
- Prezzi PDF per modulo: righe Frontali lisci (nobilitato/laccato), Frontali gola (nob/lacc), Ante telaio vetro Basic/Plus. B00500: 485/605, gola 548/668, Basic 970/Plus 1.064.
- SQL Server locale .\TECNE (Integrated Security, solo lettura). Moretti_Lst1 = catalogo Metron senza prezzi (infaus.Prezzo vuoto); composizione in composti.CmpBlob. Prezzi in MorettiSap: condizioni E1KOMG -> E1KOMG_E1KONH (date) -> E1KOMG_E1KONP (KBETR), ZP01 base per materiale, ZVA0/ZVA2/ZVA9 maggiorazioni variante. Moduli B00xxx non hanno prezzo SAP proprio: prezzo = somma elementi + GEST_DELTALST (B00500: -37), logica CG_ElementiPrezzi* in Moretti.bdm. MorettiSap2024: accesso negato.
- In Moretti.bdm aggiunte: globali TST_PREZZO_PREV / TST_PREZZO_PREV_STR, PROCEDURA SalvaPrezzoPreventivo (chiamata in Preventivo, PostPreventivo, StampaPreventivo), PROCEDURA DebugPrezzi + voce menu "Debug prezzi moduli".

- 2026-10-09 B00500 verificato: Metron 485 = PDF lisci nobilitato 485 (scocca 335 = fianco 100+fondo 52+coperchio 49+schiena 74+ripiano 42+2 aste 9; ante 95+92; delta -37 come RigaOrdineAccessorio). DebugPrezzi scrive C:\Tesy\Moretti\BdmSviluppo\DebugPrezzi.txt. STRDADB(struttura,"PREZZO") restituisce gia' il totale del modulo.

- Estrazione "Frontali nobilitati" dal PDF: _analisi_prezzi\listino_pdf\ (estrai_nobilitati.py, prezzi_nobilitati.csv, anomalie_estrazione.txt). Coordinate frammenti pypdf + passo colonne; pagine a coppie (spread, a volte coordinate negative, pagine duplicate tipo 148/149). Pagine composizioni (ARMADI ..., ESEMPI COMPOSIZIONI) danno prezzi di composizione, non per codice.

- VerificaPrezzi (menu "Verifica prezzi listino"): inserimento composto da codice = COMANDO2 ELEMENTO "cod" + POSIZIONA 0 0 0 0 + RIGENERA (come SM_SubCreaImgListino), pulizia DELETE3 idx; ELEMENTO_S+STRUTTURA_S NON inserisce composti. Output VerificaPrezzi.txt, confronto con _analisi_prezzi\listino_pdf\confronta.py. 2026-10-09: B00500-B00512 tutti OK; B00513 (con cassetto) ha bloccato Metron nella RIGENERA di OnRighe (log fermo in CConfiguratore::CalcolaCaratteristiche). Trace: C:\Tesy\Moretti\Logs\trace_AAMMGG.txt e log_AAMMGG_n.txt. Metron usa il listino su SQL01, non la copia locale TECNE.

- 2026-10-12: B00500-B00515 (pag. 146-147) 16/16 OK. Elenco codici: crea_elenco.py -> elenco_codici.csv (Excel) + VerificaPrezzi_elenco.ini (anche in BdmSviluppo, letto con READ_INI2). VerificaPrezzi chiede categoria/famiglia (menu dinamico VP_CATEGORIA con MISURE_EDIT, sezione Misure in Moretti.bdm) e codice di ripartenza; avanzamento: barra di stato ($M4_COLLEZIONE_NAME__) e LogOnSplash NON si vedono durante il giro (tolti); ora monitor aperto dalla procedura con COMANDO2("ESEGUI ASYNC python \"...avvia_monitor.py\""). ATTENZIONE: ESEGUI ASYNC lancia NASCOSTO e Metron ASPETTA la fine del processo (con cmd /c bat Metron si e' bloccato; con start la finestra restava invisibile) -> avvia_monitor.py apre monitora_verifica.py in nuova console visibile (CREATE_NEW_CONSOLE + SW_SHOWNORMAL) e termina subito (finestra console che segue VerificaPrezzi.txt + VerificaPrezzi_stato.txt INIZIO;n/FINE;n e confronta col PDF in tempo reale). ~4-6 s per modulo. 60,4 H.194: 12/12 OK.

- File generati (VerificaPrezzi.txt, _stato, DebugPrezzi.txt, VerificaPrezzi_elenco.ini, confronto.csv) SOLO in _analisi_prezzi\controlli\ (richiesta di Lino 2026-10-12: niente file nella cartella principale BdmSviluppo). Script in _analisi_prezzi\listino_pdf\.

- 2026-10-12 TUTTI MODULI ARMADIO: 213/214 OK, unico diverso B05139 (P.44,4 H.226) PDF 258 / Metron 246 (-12), da analizzare con DebugPrezzi. Elenco ini ora ha anche P{k}=prezzo PDF intero; VerificaPrezzi confronta e scrive VerificaPrezzi_errori.txt; a ogni lancio (senza codice di ripartenza) azzera VerificaPrezzi.txt, _errori, _stato.

- 2026-10-12 NUOVA estrazione (regola di Lino) in crea_elenco.py con PyMuPDF (installato --user): per ogni codice B00000 il primo prezzo SOTTO nella colonna, o a DESTRA sulla stessa riga (tabelline Codice|Nobilitato|Laccato), o sotto l'intestazione "Nobilitato" (celle larghe). Solo numeri AZZURRI #0084b6 (colore prezzi; quote e testi sono #231f20). Codice ripetuto -> vale la prima pagina (dopo ci sono composizioni). Titolo categoria solo se in cima (y<20). Risultato: 3618 codici, 173 categorie, 93 famiglie; i 371 della vecchia estrazione coincidono tutti. confronta.py e monitora_verifica.py ora leggono elenco_codici.csv (prezzi_nobilitati.csv/estrai_nobilitati.py superati).

- VerificaPrezzi scrive anche VerificaPrezzi_dettaglio.txt per ogni codice non corrispondente (PROCEDURA DebugPrezziStrutture, condivisa con DebugPrezzi). B05139: PDF "1 ripiano" 30x44,4x226 = composto Metron (aste commentate), DELTALST -38; elementi = 284 (ZP01 invariati dal 1/9/2025), delta necessario -26 (gemello B00549 30x60x226 ha -27) -> errore probabile del DELTALST in SAP, segnalato a Lino 2026-10-12. Il dettaglio ora riporta anche SOMMA ELEMENTI e DELTALST NECESSARIO (= PDF - (Metron - DELTALST)).

- Menu VP_CATEGORIA: categorie 1..999, famiglie 1000+K (prima 100+K: collisione con 173 categorie, corretto 2026-10-12), 9999 = azzera. Categorie gia' verificate in controlli\VerificaPrezzi_fatte.ini ([Fatte] nome categoria=DATA(0) - n codici, n diversi; NON azzerato a ogni lancio; con chiavi doppie vale la prima). Menu mostra [FATTA] e per le famiglie [k/n fatte]. I giri fatti prima di questa funzione non sono segnati (e il nuovo elenco ha piu' codici per categoria).

- Dettaglio/Debug: solo componenti con codice SAP (GEST_ZMATERIALE non vuoto e non "__"); quelli con [] sono contenitori del configuratore (ScoccaPensilePonte24, AntaArmBattente...) che ripetono i prezzi dei figli (richiesta Lino). Somma componenti con codice = SOMMA ELEMENTI (verificato su B01262 = 1177).
- 2026-10-15 falso errore sulle LIBRERIE OUTLINE SOSPESE (es. B06200 PDF 295 / verifica 252, a mano 295): la verifica inseriva tutto con POSIZIONA 0 0 0 0 (a terra) e il fianco Outline si calcola "a terra" (Ctrl_Posiz_FiaOutline: H_DA_PAVIMENTO>0.1 -> sospeso), differenza 32..44 solo in funzione dell'altezza, uguale sui fianchi sospesi venduti da soli (B06012-21). Corretto: categorie con "SOSPES" nel nome inserite con POSIZIONA 0 410 0 0 (x y z ang, y verticale). Probabile stessa causa per MODULI SOSPESI FLY e altri "sospesi": rilanciare. Non sollevare tutto: le librerie a terra sollevate aprono DISPLAY2(MSG(2021)) che blocca il giro.
- VerificaPrezzi_errori.txt: colonne codice;prezzo_pdf;prezzo_metron;differenza;deltalst_attuale;deltalst_necessario;categoria (dal 2026-10-12).
- 080005651 = Kit Pulizia (RigaOrdineAccessorio aggiunta da OnRighe, 6 EUR, struttura separata): escluso da totale ordine, conteggio strutture e dettaglio (PROCEDURA VerificaPrezziEscluso, elenco ";080005651;"). Il confronto usa comunque il prezzo del modulo. Giri del 12/10 pomeriggio: molti diversi in CABINE MODULAR/WALL, MODULI SOSPESI FLY, PONTI LINEARI P.60,4, ARMADI ESTRAIBILI -> da analizzare.

**Why:** lavoro su piu' sessioni, il percorso per leggere PDF e DB e' costato diversi tentativi.
**How to apply:** ripartire da qui per estendere il confronto a tutti i moduli; regole BDM in [[bdm-lezioni-tecniche]].
