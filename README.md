# Eipä Vissiin! 2026

Yksipäiväisen DIY-festarin yksisivuinen sivusto ja A3-juliste.
Ilme: myöhäisen 90-luvun koulun disco, koottu clipartista.

Ei build-vaihetta, ei riippuvuuksia, ei frameworkkia. Pelkkää HTML:ää ja CSS:ää.

```
eipa-vissiin/
├── index.html          yksisivuinen sivusto
├── poster.html         A3-juliste (297 × 420 mm), tulostettava
├── css/
│   ├── site.css        sivuston tyylit (paletti muuttujissa ylhäällä)
│   └── poster.css      julisteen tyylit, mitat mm:einä
├── assets/
│   ├── README.md       ← KUVAMANIFESTI + clipart-lähteet
│   ├── clipart/        clipart-kuvat
│   ├── buttons/        88×31-napit
│   ├── bg/             taustatiili
│   └── social/         og-kuva
├── netlify.toml
├── robots.txt
└── .gitignore
```

## Paikallinen esikatselu

Absoluuttiset polut (`/css/site.css`) vaativat palvelimen — pelkkä
tiedoston avaaminen selaimessa ei riitä.

```sh
cd eipa-vissiin
python3 -m http.server 8000
```

Sivusto: <http://localhost:8000/> · Juliste: <http://localhost:8000/poster.html>

## Mitä pitää täyttää

Kaikki paikkamerkit on merkitty sanalla `TODO`. Lista:

```sh
grep -rn 'TODO' index.html poster.html netlify.toml robots.txt
```

Kuvapaikat erikseen:

```sh
grep -rn 'data-slot' index.html poster.html
```

Valmiina: päivämäärä, paikka, osoite, kellonajat, ikäraja, line-up, hinta,
maksutavat, UKK, sähköposti ja Instagram. Sivusto ja juliste on synkattu
keskenään.

Jäljellä:

1. **Saavutettavuustiedot** — `index.html`, Perille-osion `Saavutettavuus`.
   Kaksi TODO-riviä (esteetön sisäänkäynti, esteetön wc). Tarkista Tukikohdasta.
   Tämä on käytännössä ainoa kohta jota joku lukee tarkkaan ennen kuin päättää
   tuleeko, joten älä jätä sitä arvailun varaan.
2. **Aikataulu** — koko osio on **piilotettu** (`hidden`-attribuutti
   `<section id="aikataulu">`-tagissa ja sitä seuraavassa `<hr>`:ssä), koska
   yksi bändi puuttuu eikä alustavaa aikataulua ole. Navin Aikataulu-linkki on
   kommentoitu pois. Palautusohjeet ovat `index.html`:ssä osion yläpuolella.

   > Osiota ei ole kommentoitu pois, koska sen sisällä on jo kommentti.
   > HTML-kommentit eivät mene sisäkkäin — ulompi päättyisi ensimmäiseen
   > `-->`:iin ja merkkaus hajoaisi. `hidden` on tässä oikea työkalu: se
   > piilottaa osion myös ruudunlukijalta.
3. **Domain** — `index.html` (og-tagit), `poster.html` (`.poster__url`),
   `robots.txt`.
4. **FB-eventti** — `index.html`, Yhteystiedot.
5. **Tukijoiden 88×31-napit** — kommentoitu pois molemmista tiedostoista.
6. **Kuvat** — ks. [`assets/README.md`](assets/README.md). Kaikki
   clipart-paikat ovat vielä tyhjiä.

## Värien vaihtaminen

Paletti on kokonaan CSS-muuttujissa kummankin tiedoston alussa
(`:root`-lohko). Vaihda `--navy`, `--hotpink`, `--cyan` jne. niin koko sivu
ja juliste vaihtavat sävyä. Fontit ovat samassa lohkossa.

## Julisteen tulostaminen

Avaa `poster.html`, paina **Tulosta / PDF** -nappia (tai Ctrl/Cmd + P).

Tulostusdialogissa:

- **Kohde:** Tallenna PDF:nä
- **Koko:** A3
- **Marginaalit:** Ei mitään
- **Taustagrafiikat:** päälle ← tämä unohtuu helposti, ja ilman sitä
  julisteesta tulee valkoinen

Työkalupalkki ja ruudun skaalaus eivät tulostu.

### Painovalmis versio

Jos viet julisteen painoon ja he pyytävät leikkuuvaraa (bleed), muuta
`css/poster.css`:

```css
@page { size: 303mm 426mm; margin: 0; }   /* A3 + 3 mm joka reunalle */
.poster { width: 303mm; height: 426mm; padding: 17mm 15mm; }
```

Kopiokoneelle menevään versioon tätä ei tarvita. Valkoinen reuna A3:lla on
täysin ajanmukainen.

## Julkaisu Netlifyyn

`netlify.toml` on valmiina: ei build-komentoa, julkaisukansio on repon juuri.

```sh
git init && git add -A && git commit -m "Eipä Vissiin! 2026 -sivusto"
gh repo create eipa-vissiin --private --source=. --push
```

Netlifyssä: **Add new site → Import an existing project** → valitse repo →
asetukset tulevat `netlify.toml`:sta → Deploy.

Domain: **Domain management → Add a domain**. Netlify hoitaa Let's Encrypt
-sertifikaatin automaattisesti kun DNS osoittaa oikein.

Ilmaistaso riittää tähän moninkertaisesti — sivusto on muutama sata kilotavua
staattisia tiedostoja eikä siinä ole yhtään funktiota tai buildia.

## Huomioita

- **Ei JavaScriptiä** paitsi julisteen tulostusnappi. UKK toimii natiivilla
  `<details>`-elementillä.
- **Saavutettavuus.** Ilme on 1997 mutta merkkaus ei: oikeat otsikkotasot,
  skip-linkki, näkyvä fokus, `prefers-reduced-motion` pysäyttää tickerin.
  Jos vaihdat paletin, tarkista kontrastit — teksti on tarkoituksella
  valkoisilla paneeleilla juuri siksi.
- **Kävijälaskuri footerissa on koriste**, ei laske mitään. Se on merkitty
  `aria-label`illa koristeeksi.
- **Ticker on CSS-animaatio**, ei `<marquee>`. Näyttää samalta, ei ole
  poistettu selaimista.
