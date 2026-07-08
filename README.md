# dossier-normativo-ocf

Repository di appoggio per la **routine settimanale di controllo normativo OCF**.

La routine monitora le fonti ufficiali della normativa rilevante per l'Organismo di
vigilanza e tenuta dell'Albo unico dei Consulenti Finanziari (OCF) e per gli intermediari,
scarica i documenti ufficiali, ne verifica le variazioni settimana su settimana tramite
hash del contenuto e, **solo in caso di modifica**, invia una notifica via email.

## Fonti monitorate

1. **Consob — TUF e regolamenti attuativi**
   <https://www.consob.it/web/area-pubblica/tuf-e-regolamenti-consob>
2. **Consob — Storico modifiche Regolamento Intermediari (n. 20307/2018)**
   <https://www.consob.it/web/area-pubblica/storico-modifiche-intermediari3>
3. **OCF — Normativa e Regolamento Albo**
   <https://www.organismocf.it/portal/web/portale-ocf/normativa>
4. **Banca d'Italia — Vigilanza / Normativa (Testo Unico Bancario e disposizioni di vigilanza)**
   <https://www.bancaditalia.it/compiti/vigilanza/normativa/index.html>

## Come funziona

- Ad ogni esecuzione i PDF ufficiali vengono scaricati verbatim (nessuna rielaborazione del
  testo) e archiviati in [`documenti/`](./documenti/).
- Per ogni documento viene calcolato lo hash **SHA-256** del contenuto e confrontato con la
  versione dell'esecuzione precedente registrata in [`state.json`](./state.json).
- Le versioni storiche restano nella cronologia git: ogni modifica di un documento produce un
  nuovo commit del relativo PDF.
- Su Google Drive, cartella **Normativa OCF**, è mantenuto il file **`indice.md`** con l'elenco
  dei documenti, la fonte di ciascuno e la data dell'ultimo controllo.
- Se **nessun** documento è cambiato rispetto alla settimana precedente, non viene inviata
  alcuna email.
- Se **anche un solo** documento è cambiato, viene inviata una email (oggetto:
  *"Aggiornamento normativo settimanale"*) che riassume cosa è cambiato, in quale documento e
  da quando si applica, se la fonte lo specifica.

## File

- `state.json` — stato tra le esecuzioni: elenco documenti, fonte, URL, hash SHA-256, dimensione,
  data ultimo controllo e data ultima modifica rilevata.
- `documenti/` — copia verbatim dei PDF ufficiali monitorati (archivio versionato).
- `indice.md` — indice leggibile dei documenti (mantenuto anche su Google Drive).

## Note operative

- Il **TUF (D.lgs. 58/1998)** e i **Decreti Ministeriali** richiamati dalla pagina OCF sono
  testi consolidati pubblicati su Normattiva come pagine HTML (non PDF stabili): se ne monitorano
  le pagine di riferimento anziché lo hash di un file.
- Il connettore Google Drive disponibile accetta solo contenuto inline: i PDF ufficiali (fino a
  ~3,7 MB) non possono esservi caricati come binari. L'**archivio durevole dei PDF è quindi il
  repository git** (`documenti/`); su Drive resta l'`indice.md` con i collegamenti alle fonti
  ufficiali per la consultazione uno per uno.
