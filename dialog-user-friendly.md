---
name: dialog-user-friendly
description: "Come l'utente vuole le dialog dei wizard: visuali, guidate, niente dati tecnici; colori leggibili su sfondo scuro"
metadata:
  node_type: memory
  type: feedback
  originSessionId: feecbae4-c78f-4961-97f4-ea297a175271
  modified: 2026-10-07T07:48:26.819Z
---

L'utente boccia le dialog "a tabella" (coordinate X/Z, saetta, "lato prima/dopo"): vuole wizard guidati e visuali.
Cosa ha funzionato: disegno quotato della sagoma con lettere (L, P, T, F, A, B, R1, R2) e caselle con lo stesso bollino colorato;
scelta di sagome/spigoli cliccando immagini (galleria, bottoni numerati) che aggiornano l'anteprima; passi facoltativi per i casi avanzati;
messaggi d'errore in chiaro sotto le caselle; dialog centrate, pulsanti Indietro/Avanti/Annulla visibili.
Le dialog BdmDialog hanno sfondo grigio scuro: testo colorato (blu/verde) non si legge -> usare immagini-bollino (tag_*.jpg) e testo normale.

**Why:** feedback ripetuto su piano libero e piano profilo ("non e' user friendly", "le scritte blu e verdi si vedono poco").
**How to apply:** ogni nuova dialog: prima il disegno che spiega, poi poche caselle legate al disegno; niente termini tecnici interni. Vedi [[piano-scrivania-profilo]].
