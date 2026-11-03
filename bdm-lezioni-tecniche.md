---
name: bdm-lezioni-tecniche
description: "Regole BDM scoperte a caro prezzo (a/b*c, VAR_VALUE_SET non aggiornato, contorni YSweep, punti doppi, EXITSUB, dialog Check/Button, PowerShell)"
metadata:
  node_type: memory
  type: feedback
  originSessionId: feecbae4-c78f-4961-97f4-ea297a175271
  modified: 2026-11-03T13:45:58.160Z
---

1. **Espressioni a/b*c: BDM le valuta come a/(b*c)** (CONFERMATO 2026-10-07: era la vera causa degli archi di SG20 dritti in Metron; dopo la correzione "funziona perfettamente"). Scrivere sempre (a*c)/b o mettere parentesi esplicite. Controllare con una ricerca regex ogni nuovo codice.
2. Non usare VAR_VALUE_SET(ATTUALE(),nome,...) / VAR_VAL(ATTUALE(),nome) per variabili che poi vengono lette per nome (es. passate con @PtX1 = PtX1): le dialog vedevano i valori giusti ma il 3D la sagoma precedente. Usare assegnazioni dirette per nome con procedure "a scala" (if( i==1 ) { PtX1 = ... }).
3. Contorni YSweep (base_n / arc_n, arc_n = punto sull'arco tra base_n e base_n+1): li percorro nello stesso verso delle sagome originali PuntiSagomaScrivania_* (da (0,0) lungo il fianco sinistro). Non e' provato che il verso opposto fosse un problema (la causa vera era il punto 1), ma tenerlo uguale alle originali e' piu' sicuro.
4. Mai due base_ consecutivi coincidenti (tratto lungo zero): togliere i doppioni.
5. EXITSUB() solo al primo livello di una procedura chiamata dalla sezione dell'elemento; nelle procedure annidate usare if.
6. Per le sagome scrivania esistono gia' le procedure parametriche Sub_PntSagScr_SG14 / Sub_PntSagScr_SR14 (X1,Z1) / Sub_PntSagScr_SR12 (Z2) in Complementi.inc: riusarle (anche chiamandole e poi sostituendo singoli base_/arc_).
7. I punti base_ sono memorizzati in coordinate globali: non rileggerli (base_0.x), tenere copie in variabili.
8. DialogXxx.txt viene riletto a ogni apertura; il codice BDM no: dopo aver cambiato CorsoBdm.inc l'utente deve ricaricare la collezione (sintomo: dialog nuova con variabili vuote).
9. BdmDialog: il valore di ritorno di una Check non e' affidabile come ==1 (nel codice esistente e' letto solo come vero/falso); meglio ricavare lo stato dai valori. Funziona invece un Button con Result = 0 e OnClick che riassegna le $wk_ delle Edit (la dialog resta aperta e le caselle si aggiornano). Testo colorato illeggibile sullo sfondo scuro: usare immagini-bollino.
10. Numeri negativi da/verso le Edit: formattare e leggere il segno a mano (PwFmt / PwNum nel wizard del profilo).
11. Array locali con indice variabile (tp_qx[i] = ..., tp_qx[tp_fl[k]]) NON affidabili: errore "token <tp_qx_##0> not found". Usare variabili globali a nome dinamico: VAR_VALUE_SET(-1,STRCAT("wk_x_",STR(i)),v) / VAR_VAL(-1,...). Gli array restituiti dalle funzioni ($wk_tp[i] da WITH_ALIAS) funzionano.
0. I file BDM sono ANSI 1252: MAI modificarli con gli strumenti Edit/Write (rovinano gli accenti), solo con script cp1252 -> [[bdm-file-ansi-no-edit]].
12. PowerShell: comandi con "//" a inizio token vengono bloccati -> scrivere script .ps1 col Write tool e lanciarli; file BDM in ANSI 1252 + CRLF; precedenze: virgola negli argomenti di New-Object, '-f' lega solo alla stringa a sinistra ('{0:X' + $w + '}' -f $c sbaglia), "-1" passato a funzioni diventa stringa (tipizzare [double]).
13. Nelle dichiarazioni di variabile il modo Utente vuole sempre le parentesi: `Utente ( )` (o `Utente ( path )`, `Utente ( 0-2 )`); `Utente` da solo da' "Errore di parsing: Aspettavo (".
14. CHIEDI(-1,"var"): Annulla NON ferma la procedura e il valore restituito non e' documentato. Tecnica usata in VerificaPrezzi: menu impostato prima a un valore sentinella (0) -> se resta 0 = annullato; il valore restituito con Ok serve da riferimento per le CHIEDI successive (r != rOk = annullato). ATTENZIONE: se il valore attuale di una variabile Menu non esiste fra le voci, Metron DISABILITA il tasto Annulla -> la sentinella deve essere una voce vera del menu (es. 0 "--- nessuna scelta / annulla ---"). Da confermare che funzioni (2026-10-12).

15. Variabili globali $ nel wizard di un elemento (2026-11-03, pannello sagomato): $wk_ed_L creata per la prima volta dentro una procedura chiamata (GOSUB PnEdDaElemento) e poi letta in un'altra -> "token (numeric) <$wk_ed_L> not found", mentre $wk_ed_ok/$wk_ed_np create al primo livello del wizard funzionavano. Regola prudente: creare TUTTE le $ al primo livello della procedura del wizard (valore iniziale), poi assegnarle dove serve. (Nella stampa, chiamata da macro, le $ create nelle procedure annidate invece funzionano.)
16. OnClick delle dialog BdmDialog (GOSUB di una procedura): il codice gira FUORI dall'elemento (errore con IDX=<-1>) e le variabili LOCALI non restano da una riga all'altra ("token (numeric) <wp_mxl> not found" subito dopo wp_mxl = ...). Nelle procedure chiamate dagli OnClick usare SOLO variabili globali $wk_... (create al primo livello del wizard, vedi 15), niente ATTUALE() ne' variabili dell'elemento; anche i numeri dal testo con una procedura a globali (PnEdNum al posto di PwNum).

**Why:** ognuno di questi punti ha causato giri di prove fallite sul piano profilo ([[piano-scrivania-profilo]]).
**How to apply:** controllare questa lista prima di scrivere nuovo codice BDM; la guida generale e' in [[guida-linguaggio-bdm]].
