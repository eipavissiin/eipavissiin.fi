# Eipä Vissiin! 2026

Yksipäiväisen DIY-festarin yksisivuinen sivusto ja A3-juliste.
Ilme: myöhäisen 90-luvun koulun disco, koottu clipartista.

**Julkaistu: <https://eipavissiin.fi>** — Netlify rakentaa ja julkaisee
automaattisesti jokaisesta `main`-haaraan työnnetystä commitista.

Ei build-vaihetta, ei riippuvuuksia, ei frameworkkia. Pelkkää HTML:ää ja CSS:ää.

```
eipa-vissiin/
├── index.html          yksisivuinen sivusto
├── poster.html         A3-juliste (297 × 420 mm), tulostettava
├── sticker.html        pyöreä tarra, leikkauskoko 60 mm
├── css/
│   ├── site.css        sivuston tyylit (paletti muuttujissa ylhäällä)
│   ├── poster.css      julisteen tyylit, mitat mm:einä
│   └── sticker.css     tarran tyylit, mitat mm:einä
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
3. **FB-eventti** — `index.html`, Yhteystiedot.
4. **Tukijoiden 88×31-napit** — kommentoitu pois molemmista tiedostoista.
   Ainoat vielä tyhjät kuvapaikat.

Clipart on paikallaan: kaikki muut kuvapaikat on täytetty CC0-lisensoiduilla
SVG-kuvilla Openclipartista, ja favicon sekä some-jakokuva on generoitu niistä.
Ks. [`assets/README.md`](assets/README.md).

### Fontti, joka pitää tarkistaa ennen painoa

Otsikot käyttävät fonttipinoa `Impact, Haettenschweiler, Arial Black,
sans-serif`. **Impact ei kuulu Linuxin vakiofontteihin.** Jos tulostat
julisteen koneella jolle sitä ei ole asennettu, otsikko latautuu geneerisellä
sans-serifillä, joka on selvästi leveämpi — ilme muuttuu ja pahimmillaan
teksti ei mahdu riville.

Tarkista ennen kuin viet julisteen painoon:

```sh
fc-list | grep -i -E 'impact|arial black'
```

Jos tulos on tyhjä, joko asenna `ttf-mscorefonts-installer` tai vaihda
otsikkofontiksi vapaasti lisensoitu Impact-tyylinen leikkaus (esim. Anton,
SIL OFL) ja liitä se mukaan repoon `@font-face`-määrittelyllä. Silloin
juliste renderöityy samanlaisena joka koneella.

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

## Tarran tulostaminen

Avaa `sticker.html`, paina **Tulosta / PDF**. Samat asetukset kuin
julisteessa, paitsi koko: **66 × 66 mm**, marginaalit ei mitään,
taustagrafiikat päälle.

Mitat:

| | |
|---|---|
| **66 mm** | arkki eli leikkuuvara (bleed). Tausta ulottuu tänne asti. |
| **60 mm** | leikkauslinja. Tähän tarra leikataan. |
| **50 mm** | turva-alue. Kaikki teksti pysyy tämän sisällä. |

Apuympyrät näkyvät vain ruudulla — tulosteeseen ja PDF:ään ne eivät tule.

**Miksi leikkuuvara.** Pyöreän tarran leikkuri heittää aina hieman. Ilman
3 mm:n varaa reunaan tulee valkoinen sirppi, ja ilman turva-aluetta teksti
leikkautuu. Jos tarrapaino kysyy leikkuuvaraa, vastaus on 3 mm joka reunalla.

**Ympyrä kaventuu reunoja kohti.** Keskellä käytettävissä on 50 mm, mutta
20 mm keskustasta ylös tai alas enää `2 × √(25² − 20²) = 30 mm`. Siksi
leveimmät rivit ovat keskellä ja lyhyimmät ylhäällä ja alhaalla. Jos
muutat tekstejä tai kokoja, tarkista että rivi mahtuu **sillä korkeudella**
eikä vain keskellä.

### Painovalmis versio

Jos viet julisteen painoon ja he pyytävät leikkuuvaraa (bleed), muuta
`css/poster.css`:

```css
@page { size: 303mm 426mm; margin: 0; }   /* A3 + 3 mm joka reunalle */
.poster { width: 303mm; height: 426mm; padding: 17mm 15mm; }
```

Kopiokoneelle menevään versioon tätä ei tarvita. Valkoinen reuna A3:lla on
täysin ajanmukainen.

## Julkaisu

Sivusto on tuotannossa osoitteessa <https://eipavissiin.fi>.

- **Repo:** `eipavissiin/eipavissiin.fi` (julkinen)
- **Netlify:** julkaisee automaattisesti jokaisesta `main`-haaraan työnnetystä
  commitista. Ei build-komentoa, julkaisukansio on repon juuri — asetukset
  tulevat `netlify.toml`:sta.
- **DNS:** Netlify DNS. Nimipalvelimet `dns1–dns4.p08.nsone.net`.
- **Sertifikaatti:** Let's Encrypt, uusiutuu automaattisesti. Kattaa sekä
  `eipavissiin.fi`:n että `*.eipavissiin.fi`:n.
- **Ohjaukset:** `www` → apex, HTTP → HTTPS. Ensisijainen osoite on apex,
  koska julisteessa lukee `EIPAVISSIIN.FI` ilman www:tä.

Muutosten julkaisu:

```sh
git push origin main
```

> **Repo on julkinen.** Netlifyn ilmaistaso ei julkaise organisaation
> omistamia yksityisiä repoja — se vaatii Pro-tason. Mitään salaista täällä
> ei ole, mutta älä lisää sellaista myöhemminkään.

> **Sähköposti.** Koko DNS on Netlifyllä. Jos joskus haluat
> `@eipavissiin.fi`-osoitteet, MX- ja TXT-tietueet lisätään **Netlifyn**
> DNS-paneeliin, ei rekisteröijälle. Rekisteröijälle lisätyt tietueet eivät
> tee mitään, ja posti hajoaa hiljaisesti.

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
