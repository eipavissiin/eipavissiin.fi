# eipavissiin.fi

Eipä Vissiin! -festarin sivusto. Yksi hakemisto per vuosi, juuressa
tulevan vuoden sivu.

**Tuotannossa: <https://eipavissiin.fi>** — Netlify rakentaa ja julkaisee
automaattisesti jokaisesta `main`-haaraan työnnetystä commitista.

Ei build-vaihetta, ei riippuvuuksia, ei frameworkkia. Pelkkää HTML:ää,
CSS:ää ja yhtä itse hostattua fonttia.

```
.
├── index.html          2027: save the date
├── css/
│   └── save-the-date.css
├── fonts/              Great Vibes (OFL), itse hostattuna
├── assets/social/      2027:n og-kuva ja ikonit
├── favicon.png
├── 2026/               ← koko vuoden 2026 sivusto, arkistona
│   ├── index.html      festarisivu
│   ├── poster.html     A3-juliste
│   ├── poster-print.html  painovalmis juliste (generoitu)
│   ├── sticker.html    pyöreä tarra
│   ├── rider.html      tekninen rider (noindex)
│   ├── README.md       ← vuoden 2026 dokumentaatio
│   ├── bin/, css/, assets/
│   └── favicon.png
├── bin/eipa-serve
├── netlify.toml        ohjaukset ja headerit
├── robots.txt
└── sitemap.xml
```

## Vuoden 2027 sivu

Teema on **häämessut**, joten juurisivu on kutsukortti: norsunluu,
kullanruskea, kaunokirjoitusotsikko ja paljon tyhjää. Mitään 2026:n
90-luvun ilmettä ei ole tarkoituskaan kantaa mukana — se jää arkistoon.

Otsikkofontti **Great Vibes** on repossa eikä CDN:ssä, ks.
[`fonts/README.md`](fonts/README.md). Skriptifontilla varajärjestelmä on
erityisen ruma: `cursive` on eri fontti joka koneella.

## Vanhat osoitteet

Vuoden 2026 sivusto oli aiemmin juuressa. Siirto `/2026/`-hakemistoon
rikkoisi jaetut linkit, joten vanhat osoitteet ohjataan `netlify.toml`:ssa:

| Vanha | Uusi |
|---|---|
| `/poster.html` | `/2026/poster.html` |
| `/poster-print.html` | `/2026/poster-print.html` |
| `/sticker.html` | `/2026/sticker.html` |
| `/rider.html` | `/2026/rider.html` |
| `/assets/*` | `/2026/assets/*` |

**Juurta ei ohjata.** Painetussa julisteessa ja tarrassa lukee
`EIPAVISSIIN.FI`, ja sen kuuluu viedä tulevaan tapahtumaan eikä
menneeseen. Siksi `/2026/`:n etusivulla on banneri joka kertoo kummasta
vuodesta on kyse.

`/assets/*` on ohjattu ilman `force`-lippua, joten Netlify tarjoaa ensin
oikeasti olemassa olevan tiedoston. 2027:n og-kuva ja ikonit löytyvät
siis normaalisti, ja vain puuttuvat polut ohjataan arkistoon. `/css/*` ja
`/fonts/*` eivät ole ohjauslistalla lainkaan, koska niissä on 2027:n omat
tiedostot.

## Uuden vuoden lisääminen

1. `git mv` nykyinen juurisivu uuteen vuosihakemistoon.
2. Kirjoita absoluuttiset polut (`/css/…`, `/assets/…`) muotoon
   `/<vuosi>/…`. Myös `og:image`, `og:url` ja CSS:n `url()`.
3. Lisää vanhat osoitteet `netlify.toml`:n ohjauksiin.
4. Lisää arkistobanneri vanhan vuoden etusivulle.
5. Päivitä `sitemap.xml`.

## Paikallinen esikatselu

Absoluuttiset polut vaativat palvelimen — tiedoston avaaminen selaimessa
ei riitä.

```sh
cd eipa-vissiin
python3 -m http.server 8000
```

2027: <http://localhost:8000/> · 2026: <http://localhost:8000/2026/>

Huom: `python3 -m http.server` ei lue `netlify.toml`:ia, joten ohjaukset
eivät toimi paikallisesti. Vanhat osoitteet kannattaa testata vasta
julkaistulta sivustolta.

## Julkaisu

- **Repo:** `eipavissiin/eipavissiin.fi` (julkinen)
- **DNS:** Netlify DNS, nimipalvelimet `dns1–dns4.p08.nsone.net`
- **Sertifikaatti:** Let's Encrypt, uusiutuu automaattisesti
- **Ohjaukset:** `www` → apex, HTTP → HTTPS

```sh
git push origin main
```

> **Repo on julkinen.** Netlifyn ilmaistaso ei julkaise organisaation
> omistamia yksityisiä repoja. Mitään salaista täällä ei ole, eikä sellaista
> pidä lisätä.

> **Sähköposti.** Koko DNS on Netlifyllä. Jos joskus haluat
> `@eipavissiin.fi`-osoitteet, MX- ja TXT-tietueet lisätään **Netlifyn**
> DNS-paneeliin, ei rekisteröijälle. Rekisteröijälle lisätyt tietueet eivät
> tee mitään, ja posti hajoaa hiljaisesti.

## Historia

| Vuosi | Teema | Yleisöä |
|---|---|---|
| 2025 | Alakoulun disco, vahaliidut | 47 |
| 2026 | Yläkoulun disco, clipart | 51 |
| 2027 | Häämessut | — |
