# DistanceKit

Web app in una singola pagina HTML per calcolare distanze e tempi di percorrenza **andata e ritorno** tra un punto di partenza e più luoghi fissi — e per trovare la **zona ottimale** da cui raggiungerli tutti.

Nessun build, nessuna installazione, nessun backend: un solo file da aprire nel browser.

![Type](https://img.shields.io/badge/type-Single%20File%20HTML-orange)
![Runtime](https://img.shields.io/badge/runtime-Browser-blue)
![Server](https://img.shields.io/badge/server-not%20required-success)
![License](https://img.shields.io/badge/license-MIT-green)

## Funzionalità

- **Andata e ritorno separati** per ogni destinazione, con evidenza delle differenze dovute a sensi unici.
- **Fino a 8 luoghi fissi** + un punto di partenza, geocodificati per indirizzo o nome.
- **4 modalità**: auto, bicicletta, a piedi, trasporti pubblici (solo Google).
- **Mappa per ogni tratta**, con percorso reale disegnato sulle strade.
- **Zona ottimale**: dati i luoghi e una priorità 1-10 per ciascuno, cerca il punto che minimizza la distanza stradale ponderata verso tutti. Confronta il risultato con il semplice centroide in linea d'aria. Funziona con entrambe le fonti dati; con Google si può opzionalmente calcolare anche le distanze via Route Matrix, necessario per i trasporti pubblici (il costo stimato in elementi è mostrato prima di avviare).
- **Traffico per fascia oraria** (solo Google): 6 fasce del prossimo lunedì, dalle 07:00 alle 20:00, basate sui dati storici del traffico.

## Due fonti dati

| | OpenStreetMap / OSRM | Google Maps |
|---|---|---|
| API key | opzionale (CARTO, solo per lo sfondo mappe) | richiesta |
| Costo | gratuito | a consumo |
| Geocoding | Nominatim | Google Geocoding |
| Traffico per fascia oraria | ✕ | ✓ |
| Trasporti pubblici | ✕ | ✓ |

Si passa dall'una all'altra con un click. La zona ottimale usa sempre OSRM.

## Uso

Scarica il repository e apri `distance-checker.html` nel browser. È tutto.

Lo sfondo delle mappe Leaflet (percorsi OSRM e zona ottimale) viene da CARTO, che da fine 2026 richiede una API key gratuita: richiedila su [carto.com/basemaps/apikey](https://carto.com/basemaps/apikey) (non serve un account) e incollala nel pannello **CARTO Basemaps API Key**. Anche questa resta salvata solo nel browser. Senza key le mappe usano le tile standard di OpenStreetMap, che richiedono un Referer valido: aprendo la pagina da `file://` possono non caricarsi, quindi in quel caso servi la pagina da un web server locale (vedi sotto).

Per usare Google Maps serve una API key con **Maps JavaScript API**, **Geocoding API** e **Routes API** abilitate (se la key ha restrizioni sulle API, devono comparire tutte e tre nell'elenco), da incollare nel pannello dedicato (resta salvata solo nel browser, in `localStorage`). Distanze, tempi e percorsi usano la Routes API (classi `RouteMatrix` e `Route` delle Maps JS API), che sostituisce le Distance Matrix e Directions API, non più attivabili sui progetti nuovi dal 1° marzo 2025. Le key con restrizione per referrer HTTP non funzionano da `file://`: servire la pagina da un web server locale, per esempio

```bash
python -m http.server 8000
```

e aprire `http://localhost:8000/distance-checker.html`.

## Analytics (opzionale)

La pagina carica `analytics.js`, escluso dal repository. Per abilitare Google Analytics copia il template e inserisci il tuo ID di misurazione:

```bash
cp analytics_template.js analytics.js
```

Senza questo file la pagina funziona normalmente.

## Note

- Costruito con [Leaflet](https://leafletjs.com/) e le API pubbliche di [OSRM](https://project-osrm.org/) e [Nominatim](https://nominatim.org/). Il calcolo della zona ottimale valuta ~120 posizioni candidate ma usa il servizio `/table` di OSRM, che restituisce un'intera matrice di distanze per richiesta: con 5 luoghi bastano una quindicina di richieste in tutto, e il risultato arriva in pochi secondi.
- I tempi OSRM non tengono conto del traffico.
- Interfaccia in italiano.

## Licenza

[MIT](LICENSE) © 2026 Luca Busi
