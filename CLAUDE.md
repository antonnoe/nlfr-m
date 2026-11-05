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

Laatst nagelopen: 5 november 2026
