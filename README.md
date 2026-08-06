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
- **Zona ottimale**: dati i luoghi e una priorità 1-10 per ciascuno, cerca il punto che minimizza la distanza stradale ponderata verso tutti. Confronta il risultato con il semplice centroide in linea d'aria.
- **Traffico per fascia oraria** (solo Google): 6 fasce del prossimo lunedì, dalle 07:00 alle 20:00, basate sui dati storici del traffico.

## Due fonti dati

| | OpenStreetMap / OSRM | Google Maps |
|---|---|---|
| API key | non serve | richiesta |
| Costo | gratuito | a consumo |
| Geocoding | Nominatim | Google Geocoding |
| Traffico per fascia oraria | ✕ | ✓ |
| Trasporti pubblici | ✕ | ✓ |

Si passa dall'una all'altra con un click. La zona ottimale usa sempre OSRM.

## Uso

Scarica il repository e apri `distance-checker.html` nel browser. È tutto.

Per usare Google Maps serve una API key con **Maps JavaScript API** e **Distance Matrix API** abilitate, da incollare nel pannello dedicato (resta salvata solo nel browser, in `localStorage`). Le key con restrizione per referrer HTTP non funzionano da `file://`: servire la pagina da un web server locale, per esempio

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

- Costruito con [Leaflet](https://leafletjs.com/) e le API pubbliche di [OSRM](https://project-osrm.org/) e [Nominatim](https://nominatim.org/), soggette a rate limit: il calcolo della zona ottimale fa molte richieste ed è volutamente lento.
- I tempi OSRM non tengono conto del traffico.
- Interfaccia in italiano.

## Licenza

[MIT](LICENSE) © 2026 Luca Busi
