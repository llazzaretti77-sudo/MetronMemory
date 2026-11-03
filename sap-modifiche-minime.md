---
name: sap-modifiche-minime
description: "Preferenze di Lino su SAP: non modificare ne' duplicare funzioni esistenti (Z_SD_BAPI_SALESORDER_CHANGE), strumenti automatici e non manuali, dire chiaramente cosa si tocca"
metadata:
  node_type: memory
  type: feedback
  originSessionId: d0ecf493-8481-4ae3-bbd8-9b37532a0d02
  modified: 2026-10-23T14:15:23.960Z
---

Lino rifiuta di modificare la funzione originale usata da Metron (Z_SD_BAPI_SALESORDER_CHANGE) e non gradisce funzioni "gemelle" nel gruppo funzioni di Metron; preferisce soluzioni autocontenute (un solo report) o strumenti locali.

**Why:** il gruppo funzioni e' in produzione e condiviso con altri lavori (richieste di colleghi, es. SWITCH_PF); ha detto "non lo farò" quando ha capito che l'opzione implicava modificare la BAPI originale, e "non mi piace la idea di creare una funzione gemella".
**How to apply:** prima di proporre un intervento in SAP dire esplicitamente quali oggetti esistenti vengono toccati e quali nuovi creati; offrire sempre un'alternativa che non tocchi oggetti esistenti. Lino vuole uno strumento che spieghi da solo la causa di un errore ("non voglio poi usare uno strumento manuale"), non solo riprodurlo: un messaggio uguale a quello di SAP non basta. Passi SAP da dare uno per volta, con nomi esatti di menu/transazioni e senza presumere che le voci esistano (le versioni italiane cambiano). Vedi [[diagnosi-errori-sap-ord]].
