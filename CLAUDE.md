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

## Stand per 26 november 2026: kaart met twee stappen
- Op nederlanders.fr/m (in het kader) toont `index.html` nu "Bericht plaatsen op Nederlanders.fr" met twee knoppen: "1. Inloggen" (/m/signin) en "2. Bericht plaatsen" (/profiles/blog/new), plus "Nieuw? Word lid" (/m/signup) en een link naar InfoPoche. Alle links hebben target="_top"; alleen een tik navigeert, de pagina stuurt de bezoeker in het kader niet meer zelf weg.
- Waarom twee stappen: de mobiele inlogpagina van NING negeert een opgegeven bestemming (gemeten) en landt na het inloggen altijd op /m. Ingelogd werkt het plaatsformulier /profiles/blog/new op de telefoon wel, dankzij een script in NING Aangepaste code. Daarom: eerst inloggen, daarna terug op /m en dan plaatsen.
- Los geopend (window.top === window.self) gedrag ongewijzigd: meteen location.replace naar https://infopoche.fr/.
- Ingelogde weergave (26-11-2026): in het kader luistert `index.html` naar postMessage van https://www.nederlanders.fr (of https://nederlanders.fr) met een geldig `nlfrProfile` (naam of id, zie EMBED.md). Dan toont hij "Je bent ingelogd als <naam>" (textContent) met één gouden knop "Bericht plaatsen"; de inlog- en lid-worden-regels verdwijnen.
- Terugval: geen bericht, geen geldig profiel of een andere origin (bijvoorbeeld als NING het embedscript weghaalt) laat de uitgelogde twee-stappenweergave staan. Die is de standaard, dus geen flits.
- Externe adressen: nederlanders.fr (/m/signin, /m/signup, /profiles/blog/new) en infopoche.fr. Verandert een adres, dan werkt alleen die knop niet meer; de pagina zelf blijft werken.
- Naam in de ingelogde weergave: NING levert de naam als HTML-entiteiten plus titel ("Anton No&euml;, beheerder en gastheer"); `index.html` decodeert die veilig met DOMParser, toont alleen het deel vóór de eerste komma via textContent en toont zonder bruikbare naam alleen "Je bent ingelogd" (nooit het id).
- Tegelscherm (26-11-2026): de kaart is nu een startscherm met tegels (kop "Nederlanders.fr", twee kolommen, eerste tegel goud). Uitgelogd (standaard): Inloggen (/m/signin), Bericht plaatsen (/profiles/blog/new), Berichten (https://infopoche.fr/#/forum, subregel "met alle reacties"; sinds 26-11-2026 niet meer de mobiele NING-pagina), InfoPoche (https://infopoche.fr/), Word lid (/m/signup), Nieuwsbrief (https://laposta.nl/f/ssysinmqgflb), plus een regel naar Café Claude (https://cafeclaude.fr/). Ingelogd: Bericht plaatsen, Berichten (zelfde doel en subregel), InfoPoche, Mijn profiel (/m?id=3295325:MobilePage:236780), Nieuwsbrief, Café Claude. Tegels zijn a-elementen met target="_top"; de logica van postMessage, naam en doorsturen bij los openen is ongewijzigd. De eerdere weergave met twee knoppen is daarmee vervangen.
- Melding na uitloggen (26-11-2026): het embedscript stuurt ook `nlfrVan` (pad van de vorige nederlanders.fr-pagina). Begint dat met `/profiles/blog/new` of `/profiles/blog/edit` en is er geen geldig `nlfrProfile`, dan staat in de uitgelogde weergave boven de tegels de melding "Je wilde een bericht plaatsen. Log eerst in, daarna tik je op Bericht plaatsen." (met Franse regel eronder) en een volle-breedte-knop Inloggen (/m/signin). Alleen vaste tekst, `nlfrVan` zelf wordt nergens getoond; ingelogd of ander pad: niets verandert.
- Betaalpagina (26-11-2026): ingelogd staat direct onder het tegelraster een link "Advertentie, vacature of vastgoed? Eerst betalen ↗" (/page/betaalpagina-berichten, target="_top") met een gedempte regel dat die berichten alleen na betaling worden geplaatst; uitgelogd met `nlfrVan` die met /page/betaalpagina-berichten begint toont de melding "Je wilde een advertentie plaatsen..." met de knop Inloggen (zelfde opmaak als de bericht-melding, vaste tekst).

- Uitloggen (26-11-2026): ingelogd staat onderaan een omlijnde knop "Uitloggen" (`#uitlog`, target="_top"). Het embedscript op de NING-pagina Start stuurt `nlfrUit` = het afmeldadres uit NING's eigen menu (`a[href*="/main/authorization/signOut"]`, met de sessiesleutel `xg_token`, daarom niet vast in de code). Alleen https op nederlanders.fr met pad `/main/authorization/signOut` wordt gebruikt; anders blijft de knop weg (dan uitloggen via het ☰-menu van NING).

Laatst nagelopen: 26 november 2026
