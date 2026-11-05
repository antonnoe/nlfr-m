# CLAUDE.md

Repo: nlfr-m

## Wat dit is
De mobiele pagina van Nederlanders.fr: één zelfstandig bestand (index.html) dat als iframe op een mobiele Ning-pagina staat en ook los werkt als landingspagina voor lezers uit de nieuwsbrief. Drie schermen met een onderbalk: Start, Berichten (met volledige reactiedraad) en Actueel. Bron: README.md.

## Live adres
https://nlfr-m.vercel.app/ (EMBED.md noemt dit als in te vullen Vercel-adres; de embedcode staat op https://www.nederlanders.fr/main/mobilepage/list)

## Opbouw
- index.html: de hele app, gebundeld (React en react-dom via unpkg).
- vercel.json: cleanUrls en beveiligingskoppen (X-Content-Type-Options, Referrer-Policy).
- README.md: uitleg en installatie.
- EMBED.md: embedcode voor de mobiele Ning-pagina (iframe, hoogte-sync, postMessage met nlfrProfile en nlfrPagina).

## Techniek
Statische HTML met React uit unpkg, zonder build en zonder package.json.

## Externe diensten en namen van omgevingsvariabelen
- nlfr-berichten.vercel.app: /api/berichten en /api/reacties?post=<id>.
- nlfr-menu.vercel.app/actueel: paneel Actueel.
- nlfr-nieuwsbrief.vercel.app/api/nieuwsbrief: wordt in index.html aangeroepen.
- Ning (nederlanders.fr) voor aanmelden, reageren en plaatsen.
- Google Fonts en unpkg.
- Omgevingsvariabelen: geen.

## Hoe het live gaat
De repo wordt in Vercel geïmporteerd en gedeployed, zonder build-instellingen en zonder omgevingsvariabelen. Daarna wordt de embedcode uit EMBED.md met de Vercel-URL op de mobiele Ning-pagina geplaatst.

## Regels en besluiten
Bron: README.md.
- index.html is een gebundeld bestand en wordt niet met de hand aangepast. Wijzigingen gebeuren in de ontwerpomgeving en worden daarna opnieuw gebundeld.
- Reageren en plaatsen gebeurt altijd op Ning zelf.

## Stand per 5 november 2026: verhuisd naar InfoPoche
- Besluit van Anton (20-10-2026, uitgevoerd 05-11-2026): nlfr-m verwijst door naar InfoPoche (https://infopoche.fr, repo antonnoe/if-mobiel). InfoPoche heeft het forum met alle reacties en het nieuws, en meer.
- `index.html` is nu een kleine doorverwijspagina: los geopend stuurt hij meteen door naar InfoPoche; in het kader op nederlanders.fr/m toont hij een kaart met de knop "Open InfoPoche" (een pagina in een kader mag de bezoeker niet zelf wegsturen).
- De oude gebundelde app staat ongewijzigd in `archief/nlfr-m-oud.html` (te bekijken op /archief/nlfr-m-oud). Terugzetten kan door dat bestand weer `index.html` te maken.
- Nog te doen door Anton of via Claude in Chrome: de mobiele Ning-pagina /m zelf laten doorsturen, zodat ook de tussenkaart vervalt.

Laatst nagelopen: 5 november 2026
