# Embedcode voor de mobiele Ning-pagina

Plak onderstaande code in een mobiele pagina van het type **HTML** op
`https://www.nederlanders.fr/main/mobilepage/list`.

Vervang `https://nlfr-m.vercel.app/` door de echte URL zodra de pagina op Vercel
staat.

```html
<div id="nlfrMWrap" style="margin:0;padding:0;">
  <iframe
    id="nlfrM"
    src="https://nlfr-m.vercel.app/"
    title="Nederlanders.fr mobiel"
    style="display:block;width:100%;height:80vh;border:0;background:#fdf9f8;"
    allow="clipboard-write"></iframe>
</div>
<script>
(function () {
  var f = document.getElementById('nlfrM');
  if (!f) return;
  var ORIGIN = new URL(f.src).origin;

  // 1. Hoogte: het paneel vult het scherm, de inhoud scrollt erbinnen zodat de
  //    onderbalk van de app zichtbaar blijft.
  function pasHoogte() {
    var boven = f.getBoundingClientRect().top;
    var h = Math.max(420, window.innerHeight - boven - 8);
    f.style.height = h + 'px';
  }

  // 2. Ledenstatus doorgeven. De app kan de Ning-sessie zelf niet lezen; deze
  //    pagina wel. We sturen ook de eigen URL mee, zodat de app na aanmelden
  //    hierheen terugkeert en niet naar de kale app-URL.
  function stuurProfiel() {
    var bericht = { nlfrPagina: window.location.href };
    try {
      if (window.ning && ning.CurrentProfile && ning.CurrentProfile.author) {
        bericht.nlfrProfile = {
          id: ning.CurrentProfile.author,
          naam: ning.CurrentProfile.fullName || ning.CurrentProfile.author,
          profileUrl: ning.CurrentProfile.profileUrl ||
                      ('https://www.nederlanders.fr/profile/' + ning.CurrentProfile.author)
        };
      }
    } catch (e) {}
    try { f.contentWindow.postMessage(bericht, ORIGIN); } catch (e) {}
  }

  function alles() { pasHoogte(); stuurProfiel(); }

  f.addEventListener('load', function () {
    alles();
    // De app is een moment later pas klaar om te luisteren.
    setTimeout(stuurProfiel, 400);
    setTimeout(stuurProfiel, 1500);
  });
  window.addEventListener('resize', pasHoogte);
  window.addEventListener('orientationchange', function () { setTimeout(pasHoogte, 250); });
  alles();
})();
</script>
```

## Hoe het werkt

- **Vaste hoogte, interne scroll.** Anders dan bij `/actueel` groeit dit paneel
  niet mee met zijn inhoud: de app heeft een eigen onderbalk die vastgepind moet
  blijven. Het iframe vult daarom het scherm en de app scrollt erbinnen.
- **`nlfrPagina`** — de URL van deze Ning-pagina. De app gebruikt hem als
  terugkeeradres in de aanmeld-, inschrijf- en wachtwoordlinks. Zonder dit
  bericht valt de app terug op de referrer en anders op `/m`.
- **`nlfrProfile`** — wie er is aangemeld. Alleen berichten van
  `nederlanders.fr` worden geaccepteerd. Is het lid bekend, dan toont de app
  zijn naam en zijn de reageerknoppen direct actief; de app bewaart dit lokaal,
  zodat een bezoeker die later via de nieuwsbrief binnenkomt niet opnieuw langs
  het aanmeldscherm hoeft.
- **Reageren en plaatsen** gebeurt altijd op Ning zelf: de knoppen openen de
  berichtpagina bij het reactievak (`target="_top"`), waardoor de sessie
  meegaat. De app plaatst nooit zelf iets namens een lid — dat kan een browser
  niet toestaan en zou ook onveilig zijn.

## Als Ning het `<script>`-blok weigert

Sommige mobiele modules strippen scripts. Werkt het bovenstaande niet, plak dan
alleen het iframe met een vaste hoogte:

```html
<iframe src="https://nlfr-m.vercel.app/"
        title="Nederlanders.fr mobiel"
        style="display:block;width:100%;height:88vh;border:0;background:#fdf9f8;"></iframe>
```

De app werkt dan volledig, met één beperking: hij weet niet wie er is aangemeld,
dus elk lid ziet eenmalig het aanmeldscherm. Na één keer aanmelden onthoudt de
app het lid alsnog.

## Controleren of het werkt

1. Open de mobiele pagina op een telefoon terwijl u bent aangemeld.
2. Rechtsboven hoort uw naam in het groen te staan, niet "Meld u aan".
3. Klap een bericht open en controleer of "Reageer op dit bericht" een gevulde
   bordeaux knop is.
4. Meld u af en herlaad: de knop wordt "Meld u aan om te reageren".
