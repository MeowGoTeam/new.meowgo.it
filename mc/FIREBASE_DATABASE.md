# Firebase Realtime Database — MeowGo Minecraft

Le segnalazioni di `/mc/report` vengono salvate nel Realtime Database del progetto `report-meowgo-it`.

## Struttura

```text
counters/
  BUG: 12
  GIO: 4
  TOR: 8

reports/
  BUG-001/
    id: BUG-001
    nickname: ...
    email: ...
    type: Bug
    title: ...
    description: ...
    status: open
    createdAt: ...
    attachmentPath: ...
```

Gli ID sono separati per categoria: `BUG`, `GIO`, `COS`, `TOR`, `CIT`, `FUR`, `PVP`, `CLA`, `RIC`, `ALT`.

## Sicurezza

- Lettura pubblica: **negata**.
- Creazione report: consentita solo quando il ticket non esiste e passa tutte le validazioni.
- Aggiornamento dello stato/nota/data: consentito solo a utenti Firebase con `auth.token.admin == true`.
- Cancellazione dei report: non consentita dal client.
- Contatori: possono solo aumentare di esattamente 1; il numero massimo è 999999999.
- Nickname: 3–16 caratteri, lettere/numeri/underscore.
- Email: formato valido e massimo 254 caratteri.
- Titolo: 3–100 caratteri.
- Descrizione: 10–4000 caratteri.
- Status iniziale: sempre `open`.

Realtime Database applica queste regole sul server; la validazione client-side della pagina non è considerata una barriera di sicurezza. Per Firebase, `.validate` serve proprio a imporre struttura e formato dei dati dopo che una scrittura è stata autorizzata.

## Admin

La pagina `/mc/admin/` usa Firebase Authentication email/password. La lettura delle segnalazioni richiede il custom claim:

```text
admin=true
```

Il claim deve essere assegnato con Firebase Admin SDK lato server, mai dal browser.

## App Check

Senza autenticazione pubblica per `/mc/report`, il sito rimane esposto a tentativi automatizzati di invio. Per ridurre questo rischio, abilita Firebase App Check per la web app e successivamente l'enforcement su Realtime Database e Storage.
