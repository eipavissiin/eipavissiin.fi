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

Käy nämä läpi järjestyksessä:

1. **Päivämäärä, kellonajat, paikka, osoite, ikäraja** — `index.html` (hero +
   pikatiedot + In English) ja `poster.html`.
2. **Esiintyjät** — `index.html` `.lineup`-lista, `poster.html`
   `.poster__lineup`. Ykkösnimi saa luokan `is-head` / `lineup__item--headline`.
3. **Aikataulu** — `index.html`, taulukko `.grid95`.
4. **Liput** — hinnat ja lipunmyynnin osoite (Kide.app, Tiketti, Holvi…).
5. **Saavutettavuus ja turvallisempi tila** — nämä kannattaa kirjoittaa
   itse eikä kopioida. Ne ovat käytännössä ainoa osa sivua jota joku lukee
   tarkkaan ennen kuin päättää tuleeko.
6. **Yhteystiedot ja somelinkit.**
7. **Domain** — `index.html` (og-tagit), `poster.html` (`.poster__url`),
   `robots.txt`.
8. **Kuvat** — ks. [`assets/README.md`](assets/README.md).

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
