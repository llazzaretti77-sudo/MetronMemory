---
name: mensola-personalizzata
description: "Elemento MensolaProfilo (mensola personalizzata dalle forme del listino Mensole pag. 596-601: incavi, raggi, supporti, pannello al MQ) e dove sono generatori, regole e immagini"
metadata:
  node_type: memory
  type: project
  originSessionId: 02340508-0c2b-4bab-8e26-cf2a84e0c759
  modified: 2026-11-03T17:06:45.750Z
---

Richiesta di Lino (2026-11-03): wizard per creare una MENSOLA PERSONALIZZATA partendo dalle mensole del listino (pag. 596-600, supporti pag. 601) con incavi (scassi) e raggi modificabili e scelta dei supporti. Generato il 2026-11-03, NON ancora provato in Metron; l'elemento va aggiunto al catalogo da Lino (voce nuova "Mensola personalizzata", il wizard si apre all'inserimento e poi con "Configura...").

Scelte di Lino: incavi con le regole dei piani scrivania (R1 min 5; R1<50 -> R2 = 0; R1>=50 -> R2 0 o >=25, A e B >=150); tipo squadrata/sagomata scelto nel wizard (non automatico, il wizard mostra un consiglio); supporti: 2 fino a L 1343, 3 fino a 1920, 4 fino a 2496, 5 oltre, su tutte le forme; voce nuova nel catalogo.

Catalogo (Moretti_Lst1, composti): le mensole di listino sono composti di Mensola / MensolaAngolo con variabili fisse e SET_NAME (in ordine va cmp_name: cambiare le variabili non cambia il codice). Supporto Metron: 3 = a scomparsa (B02154, RADICE SFR_REGG_SCOMP), 5 = Prism = HUMMEL (B02155), 9 = WINGS (B02156). Personalizzate = PianoScrivania PANN_MQ=1: B02157/58 squadrata (Generico 1, RADICE PANNMQ), B02161/62 sagomata (Generico 2, PANNMQST), dal gestionale di GrafPianoScrivania2025.

Elemento: MensolaProfilo -> GrafMensolaProfilo (gestionale PANNMQ / PANNMQST, GEST_X=Z GEST_Y=X GEST_Z=Y) -> graf_MensolaProfilo (YSweep, alias TopProfiloGraf, MnMensola=1), piu' ReggiMensola SP1..SP6 posati da MnPosaSupp (quote come posa_SupportoMensola: scomparsa y+sp/2 z-5, HUMMEL y-54,8 z-7,1, WINGS y-157,8 z-7,1; distanza dalle estremita 70 per 3/5, 30 per 9; mensole ad angolo anche sul muro laterale, ang_y 90 / -90 se specchiata, supporto dell'angolo in comune; fino a 40x40 uno per muro). Forme (presets_profilo.ps1, flag mensola): MLIN (SM10 RE 0, SM11 RE 25), MANG (SM31/32/33, MnA bracci, MnRE, MnRI >= 50), MQUA (SM21/22 quarto di cerchio); muro sul retro z=0, angolo a sinistra, Specchia 1 = angolo a destra. Variabili MnForma, MnRE, MnRI, MnA, Supporto, MnSupN, Generico, VAR_Y_Top = spessore 25/38.
Regole del wizard (pag. 600): L 300-2900, P 65-1200 (sagomata L>=350 P>=190), peso <= 65 kg (16,5 kg/mq sp. 2,5; 26 kg/mq sp. 3,8; superficie della forma senza incavi), RE 0 o >=25 (<= P/2, angolo <= A/2), RI 50-500, A 190..min(L,P)-100, raggi dei punti a mano 0 o >=25.
Wizard WizardMensola (wizard_mensola.txt): Mensola_1 (miniature delle 7 forme che rimettono le misure di listino, angolo sx/dx, L P A RE RI, contorno listino/punti a mano), Mensola_2 (spessore, tipo, supporti con figure del listino, superficie/peso, numero supporti), Profilo_P_N, Profilo_R_N x2 (titoli "incavo"). Dialog da dlg_mensola_block.ps1, immagini mn_* + pv_/rie_ indici 8/9/10 da render_mensola.ps1 (figure ritagliate dal PDF con mensola_ritagli.py in _generatori_profilo\mensola_img). Stampa: graf_Mensola* cercato dopo graf_Pannello*, titolo MENSOLE PERSONALIZZATE, codice MENS_<forma>_LxP, note forma/tipo/supporti (patch_mensola_stampa.ps1).
Generatori: mensola_els.ps1 (derivato dal solo piano profilo dentro $els), agganci in patch_mensola_gen.ps1. Backup prima: scratchpad 02340508...\bk_mensola.
ATTENZIONE: gen_profilo.ps1 e' condiviso con il pannello sagomato della sessione b6fffd0d ([[pannello-sagomato]]): non lavorarci in contemporanea.
Vedi [[piano-scrivania-profilo]], [[dialog-user-friendly]], [[bdm-lezioni-tecniche]].
