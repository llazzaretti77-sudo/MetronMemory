---
name: fuori-misura-zt-604
description: "Moduli armadio P.60,4 (frontale 22): massimi GEST_LRG da SAP (MARA-BREIT) a 600 vs Zt 604 in BDM; fix ArmadioBattente Diff_Z_Zt=24; come funzionano FM_*_VAL_min/max"
metadata:
  node_type: memory
  type: project
  originSessionId: 90c7f58f-c3fa-4d4a-96d8-482d7f539575
  modified: 2026-10-30T15:03:46.451Z
---

Dal 2026-10-22: frontale armadio passato da 18 a 22 mm, profondita' totale modulo 604 (era 600).

- Fuori misura: `FuoriMisura` (Cameretta.inc ~1308, chiamata da Gestionale) legge min/max da GEST_MNLN/MNSP/MNLR e GEST_LNG/SPS/LRG (var_db.inc 266+), li copia in FM_X/Y/Z_VAL_min/max e mette il menu "( min-max )". In Armadio FM_VAR_Z = "Zt" (Calcola_Z_Zt). Il massimo GEST_LRG in SAP e' MARA-BREIT (non una caratteristica di classificazione; in classificazione ci sono solo MNLN/MNLR/MNSP). Mappatura BREIT=LRG confermata da Lino.
- Elenchi in _analisi_prezzi\controlli\: moduli_armadio_P600_gest_lrg.csv (352 codici linguetta "Moduli Armadio P.600", Moretti_Lst1.legami CodCaratt=1 CodValori=13), breit_600_da_portare_a_604.csv (209 codici con BREIT=600 listino P.60,4). 580 = moduli senza frontali/fianchi: giusti cosi'. 66 codici senza materiale SAP = annullati (composti.FiltroModalita = xxxx). Modifica massiva in SAP (MM17/LSMW) a carico di Lino/consulente.
- BDM: Zt dell'ArmadioBattente era 600 perche' Calcola_Z_Zt fa Zt = Z + ConAnte*Diff_Z_Zt (580+20). Fix applicato in Armadio.inc, Assegnazioni di ArmadioBattente: `Diff_Z_Zt = 24` dopo SistCostr_Armadio. Verificato funzionante da Lino. Altri elementi (ArmadioBattenteSospeso, Tampone, ecc., Zt 600) NON toccati: da valutare se hanno ante da 22.
- DB: SQL Server .\TECNE; MorettiSap.MATMAS_E1MARAM (C_BREIT, C_LAENG, C_HOEHE), CLFMAS_E1AUSPM classificazione.

- 2026-10-30 stesso fix su ArmadioBattenteSospeso (ramo Moduli Sospesi FLY: valore Lst1 368 = P.60,4 e 380 = P.44,4; 18+18 moduli ArmadioBattenteSospeso, 8 fianchi FiancoArmaBat NON toccati): Diff_Z_Zt=24 in Assegnazioni + default Zt 604. P.44,4 ha Z=420 (menu ZArmBat "440") -> Zt 444; BREIT SAP 600->604 (18 cod.) e 440->444 (18 cod.). Backup .bak\Armadio.inc.bak_20261030_sospeso_zt604. Non ancora provato da Lino in Metron.

**Why:** capire dove correggere (SAP vs BDM) il massimo di larghezza dei moduli.
**How to apply:** se compaiono altri fuori misura su moduli con frontale 22, controllare BREIT in SAP e Diff_Z_Zt dell'elemento. Vedi [[verifica-prezzi-listino]] e [[bdm-file-ansi-no-edit]].
