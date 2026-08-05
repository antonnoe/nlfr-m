# nlfr-m

De mobiele pagina van Nederlanders.fr: één zelfstandig bestand (`index.html`)
dat als iframe op een mobiele Ning-pagina wordt geplaatst en ook los werkt als
landingspagina voor lezers die uit de nieuwsbrief komen.

## Wat het doet

Drie schermen met een onderbalk:

- **Start** — welkom, de twee snelste ingangen, wat er nu speelt, en de rest van
  de site.
- **Berichten** — alle recente berichten mét hun volledige reactiedraad. Dit is
  de reden dat deze pagina bestaat: de mobiele weergave van Ning laat reacties
  weg. Openklappen haalt de complete draad op, ook de oudere reacties die niet
  in de activiteitenfeed staan.
- **Actueel** — de bestaande nieuwspagina (`nlfr-menu.vercel.app/actueel`) als
  paneel, met hoogte-sync.

## Waar de gegevens vandaan komen

Alles via `nlfr-berichten.vercel.app`:

- `/api/berichten` — berichten, reacties uit de activiteitenfeed, en het echte
  aantal reacties per bericht.
- `/api/reacties?post=<id>` — de volledige draad van één bericht, pas opgehaald
  bij openklappen.

Zie de repo `nlfr-berichten` voor hoe die koppeling werkt.

## Aanmelden

De Ning-sessie is voor deze pagina onzichtbaar; dat blokkeert de browser. Twee
wegen:

1. Staat de pagina als iframe op Ning, dan stuurt de embedcode
   `nlfrProfile` (wie is aangemeld) en `nlfrPagina` (het terugkeeradres) door.
   De pagina bewaart het lid lokaal, zodat hij bij een later bezoek via de
   nieuwsbrief nog steeds herkend wordt.
2. Is er geen profiel bekend, dan opent een paneel met drie duidelijke wegen:
   aanmelden, wachtwoord kwijt, of inschrijven — alle drie met een `target` die
   naar de Ning-pagina terugleidt.

Reageren en plaatsen gebeurt altijd op Ning zelf; de knoppen openen de
berichtpagina bij het reactievak, waardoor de sessie meegaat.

## Installatie

1. Repo importeren in Vercel → Deploy. Geen build-instellingen, geen env-vars.
2. De embedcode op de mobiele Ning-pagina plaatsen (zie `EMBED.md`), met de
   Vercel-URL ingevuld.

## Bewerken

`index.html` is een gebundeld bestand — niet met de hand aanpassen. Wijzigingen
worden in de ontwerpomgeving gedaan en daarna opnieuw gebundeld.
