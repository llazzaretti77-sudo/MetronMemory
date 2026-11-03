---
name: sap-bapi-classificazione
description: "Aggiornamento DELTALST in classificazione materiali SAP via report ABAP ZUPD_DELTALST + BAPI_OBJCL_CHANGE; trappola VALUE_NEUTRAL, parametri, stato produzione"
metadata:
  node_type: memory
  type: project
  originSessionId: 27035517-f0f0-42fe-945f-3cf3aeb0734a
  modified: 2026-10-14T15:46:21.719Z
---

Nessun connettore SAP disponibile: i dati si preparano da file e si caricano in SAP con un report ABAP (ZUPD_DELTALST). Sorgente: C:\Tesy\Moretti\BdmSviluppo\ZUPD_DELTALST.abap.txt. Input D:\DELTALST_input.csv (codice;valore_nuovo;valore_attuale, 88 codici da VerificaPrezzi_errori.xlsx). Output: D:\DELTALST_log.csv e D:\DELTALST_backup.csv (valori di tutte le caratteristiche della classe PRIMA della modifica, sovrascritto a ogni lancio: copiarlo prima di rilanciare). Alternativa senza programma: CLMM (42 passaggi, uno per valore nuovo), file raggruppato D:\DELTALST_per_CLMM.xlsx.

Parametri: classe UTILITY, tipo classe 300 (NON 001), caratteristica DELTALST, P_NUM disattivato (DELTALST e' CHAR anche se numerica), P_TEST attivo = rollback.

Trappola: con BAPI_OBJCL_CHANGE su caratteristiche CHAR va aggiornato anche VALUE_NEUTRAL (porta il vecchio valore e ha la precedenza); senza, il salvataggio non cambia nulla e nessun errore. Le tabelle *NEW sostituiscono tutta la classificazione: si rilegge con BAPI_OBJCL_GETDETAIL, si cambia solo DELTALST, si riscrive tutto; il report confronta le altre caratteristiche prima/dopo (INVARIATE/DIVERSE).

In produzione non si crea ne' si modifica codice (Repository bloccato): il programma e' stato trasportato da test (cambio pacchetto da $TMP, richiesta, rilascio SE09, import Basis STMS).

Dati: DB MorettiSap (.\TECNE) ha la classificazione in CLFMAS_E1KSSKM (classi) e CLFMAS_E1AUSPM (valori, C_ATNAM/C_ATWRT) ma e' una copia IDoc, non aggiornata in tempo reale: non serve per verificare dopo il caricamento. Il database "Morettispa" non esiste (accesso negato). Il test non era allineato con la produzione (valori diversi); in MorettiSap i 88 valori attuali coincidono col file.

**Stato 2026-10-14:** test: B01328 195->218 OK, controlli OK. Produzione: B01404 88->74 SALVATO/INVARIATE (prova); poi prova in modalita Test sulle 88 righe: 88 TEST OK, 87/88 SAP=attuale del file (B01404 gia a 74). DA FARE: lancio reale sulle 88 righe (P_TEST disattivato) e verifica del log (88 SALVATO + INVARIATE).

**Why:** perso tempo in diagnosi: la BAPI restituisce solo avvisi, il valore restava invariato; test e produzione avevano dati diversi.
**How to apply:** per altre caratteristiche CHAR riusare il report; rileggere sempre il valore dopo il salvataggio; prima provare su un codice e in modalita Test. Vedi [[verifica-prezzi-listino]].
