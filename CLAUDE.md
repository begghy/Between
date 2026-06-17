# Between — Istruzioni di progetto

## Cos'è
Between è un prototipo di piattaforma di coordinamento per la transizione
ospedale → casa (caso studio: post-ictus), integrata in **FSE Km Zero (Regione
Veneto)**. Tre attori: **Paziente, Caregiver, Operatore sanitario**, con livelli
di accesso differenziati. NON è uno strumento clinico: solo coordinamento e
comunicazione. Autenticazione via Namirial (SPID / CIE). Progetto di tesi MA
Interaction Design, SUPSI.

## Obiettivo di questo repo
Costruire un'app **navigabile e cliccabile** partendo dallo storyboard esistente
`onboarding.html`, mantenendo **identici** stile, UI, UX e copy. L'app vive in un
file unico `index.html`.

Lo storyboard (19 schermate) è la **base di partenza, non il limite**. L'app
crescerà nel tempo con **nuove schermate, nuove sezioni interne e nuove funzioni
e navigazioni**. Ogni aggiunta deve seguire lo stesso sistema (token, componenti,
STRINGS, convenzioni di seguito). Tratta `onboarding.html` come la fondazione
visiva e di UX, e questo file come la fonte di verità che si aggiorna man mano
che l'app si espande.

## File
- `onboarding.html` — storyboard di riferimento (19 schermate in fila, 3 sezioni).
  **NON modificare.** È la fonte di verità per stile, componenti e copy, e serve
  per l'export in Figma via html.to.design.
- `index.html` — l'app navigabile (da costruire). Riusa CSS, `:root`, oggetto
  `STRINGS` e il markup delle schermate **copiandoli** da `onboarding.html`.

## Regole non negoziabili
1. **Niente testo hardcoded.** Ogni stringa visibile passa da `STRINGS` (chiavi
   `it` ed `en`) applicate via `data-i18n="chiave"`. IT/EN sempre allineati.
2. **Non toccare i design token** del `:root`. Riusa le classi e i componenti
   esistenti — non reinventarli e non aggiungere nuovi colori/raggi/font.
3. **Frame 390×844**, stile glassmorphism iOS. Mantieni status-bar e
   home-indicator come nello storyboard.
4. **Font dell'app**: `-apple-system, BlinkMacSystemFont, "SF Pro Display",
   "Helvetica Neue", sans-serif`. Codici in `ui-monospace`.
   I font del *book* (Cabinet Grotesk, Bespoke Serif, Sentient) NON si usano qui.
5. **Git prima di ogni modifica strutturale.** Step piccoli e verificabili,
   un commit per step.

## Design token (:root) — già definiti, non cambiare
`--bg-screen #0a1628` · `--bg-page #080f1e` · `--text #f8f7f4`
`--muted rgba(248,247,244,.62)` · `--muted2 rgba(248,247,244,.45)`
`--accent #4a7fd4` · `--glass rgba(255,255,255,.08)` · `--glass-border .18`
ambra `#ffb020` (solo accenti) · `--r-card 26` · `--r-btn 16` · `--r-inp 14`
`--frame-w 390` · `--frame-h 844`

## Vocabolario componenti (riusare questi)
`iphone-frame`, `screen-inner`, `screen-bg`, `status-bar` (`sb-right`,
`signal-dots`, `battery`), `scroll-area`, `home-indicator`, `screen-title`,
`subtitle`, `label-caps`, `section-kicker`, `btn-fill`, `btn-ghost`,
`link-muted`, `auth-btn` (`auth-btn-spid`/`auth-btn-cie`, `auth-icon`,
`auth-label`), `role-card` (`role-check`, `role-avatar`), `code-box`,
`data-row`, `feature-line`, `notif-row` (`notif-ring`), `mood-btn`,
`tab-bar` (`tab`), `dots`/`dots--4`, `glass`, `splash-brand`/`splash-hero`/
`splash-body`, `avatar-round`, `initials`, `op-badge`, `nav-back`.

## Architettura di index.html — tre modalità nello stesso file
- **App mode (default):** una schermata alla volta. Router JS che mostra/nasconde
  le schermate, **deeplink via hash** (`#a1`, `#a2`, … `#c6`), tasto **back** del
  browser funzionante. Elementi cliccabili cablati al flusso.
- **Import mode:** `?section=a|b|c` preservato — nasconde le altre sezioni per
  l'export html.to.design (stesso comportamento dello storyboard).
- **Dev navigator:** il toggle "SEZIONE" A/B/C/ALL resta visibile **ora** per lo
  sviluppo. Avvolgilo in un commento `<!-- DEV ONLY: rimuovere nel finale -->`.
- Marca lo shell navigabile con `data-shell="app"`.

## Mappa schermate
Legenda: **[base]** = già nello storyboard · **[nuova]** = da progettare seguendo
il sistema. Aggiorna questa mappa ogni volta che aggiungi una schermata.

**A — Paziente:** A1 Splash (SPID/CIE) → A2 Ruolo → A3 Attivazione codice →
A4 Piano attivo → A5 Notifiche → A6 Benvenuto → A7 Home — tutte **[base]**.
Sezioni interne via tab: Home **[base]**, Piano **[nuova]**, Messaggi **[nuova]**,
Profilo **[nuova]**.
**B — Caregiver:** B1 Splash → B2 Accesso → B3 Collegamento → B4 Relazione →
B5 Riepilogo accesso → B6 Home — **[base]**. Sezioni interne: **[nuove]**.
**C — Operatore:** C1 Splash → C2 Accesso → C3 Attivazione paziente →
C4 Configurazione piano → C5 Codice generato → C6 Dashboard — **[base]**.
Sezioni interne: **[nuove]**.

## Estensibilità — aggiungere schermate, funzioni e navigazioni
Lo storyboard è la base; aspettati di aggiungere. Quando crei qualcosa di nuovo:
- **ID/hash coerenti.** Schermate di flusso: continua lo schema sezione+numero
  (`#a8`, `#b7`…). Sezioni interne di un ruolo: sotto-namespace
  (`#a-home`, `#a-piano`, `#a-messaggi`, `#a-profilo`).
- **Stringhe.** Ogni testo nuovo va in `STRINGS` `it`+`en`. Mai hardcoded.
- **Componenti.** Riusa il vocabolario esistente. Se serve un componente nuovo,
  costruiscilo come variante coerente coi token (stessi glass/raggi/colori),
  non come stile isolato.
- **Shell.** Marca le schermate interne dell'app con `data-shell="app"` così
  vengono avvolte da tab bar e navigazione.
- **Registra** ogni schermata nuova nella mappa qui sopra e nel backlog.
- **Retro-compatibilità.** `?section=a|b|c` e il dev navigator devono continuare
  a funzionare anche dopo le aggiunte.

## Ordine di build
1. Motore di navigazione + flusso **Paziente** cliccabile end-to-end (A1→A7).
2. Sezioni interne Paziente: Home **[base]** + Piano / Messaggi / Profilo
   **[da progettare]**, navigabili via tab.
3. Flusso **Caregiver** (B1→B6) + sue sezioni interne.
4. Flusso **Operatore** (C1→C6) + sue sezioni interne.
5. **Nuove funzioni e schermate** dal backlog, man mano che le definiamo.
6. Edge case, rifiniture, **rimozione del dev navigator** per la versione finale.

## Backlog / da progettare
Lista viva delle aggiunte (si aggiorna in corsa):
- Sezioni interne Paziente: **Piano** (dettaglio appuntamenti/terapie/documenti),
  **Messaggi** (canale ULSS condiviso + canale privato col professionista),
  **Profilo** (identità SPID, gestione caregiver).
- Sezioni interne Caregiver e Operatore — da definire.
- _(Altre funzioni e schermate: da aggiungere qui quando decise.)_

## Come testare
Apri `index.html` nel browser, oppure `python -m http.server` nella cartella.
Verifica a ogni step: ogni elemento cliccabile porta dove deve, il tasto back
funziona, `?section=b` mostra solo il Caregiver, IT/EN si aggiornano ovunque.
