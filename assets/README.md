# Kuvamanifesti — mitä `assets/`-kansioon kuuluu

Jokainen kuvapaikka sivustolla ja julisteessa on merkitty näin:

```html
<span class="slot" data-slot="clipart/boombox.gif · 90×90">
  <img src="/assets/clipart/boombox.gif" alt="" width="90" height="90">
</span>
```

Niin kauan kuin `data-slot`-attribuutti on paikallaan, ruudulle piirtyy
vaaleanpunainen katkoviivalaatikko jossa lukee tiedostonimi ja koko.

**Kun olet pudottanut oikean kuvan paikalleen: poista pelkkä `data-slot`-attribuutti.**
Luokka `slot` jää — se hoitaa asemoinnin. Kuva ilmestyy näkyviin samalla.

Etsi kaikki jäljellä olevat paikat komennolla:

```sh
grep -rn 'data-slot' index.html poster.html
```

---

## Tarvittavat tiedostot

### `assets/clipart/` — sivusto

| Tiedosto | Koko (px) | Missä | Ehdotus |
|---|---|---|---|
| `sunburst.gif` | 90 × 90 | hero, otsikon vasen puoli | säteilevä tähti / aurinko |
| `boombox.gif` | 90 × 90 | hero, otsikon oikea puoli | mankka tai kasettisoitin |
| `new-blink.gif` | 40 × 20 | esiintyjät-otsikon vieressä | vilkkuva "NEW!" |
| `under-construction.gif` | 100 × 34 | footer | kaivinkone / liikennemerkki |

### `assets/clipart/` — juliste (isommat, tulostuslaatu)

| Tiedosto | Koko (px) | Missä |
|---|---|---|
| `star-1.gif` | 160 × 160 | julisteen ylävasen |
| `star-2.gif` | 160 × 160 | julisteen yläoikea |
| `boombox.gif` | 200 × 200 | clipart-rivi (sama tiedosto kuin sivustolla, isompi kuva käy molempiin) |
| `dancers.gif` | 200 × 200 | clipart-rivi |
| `discoball.gif` | 200 × 200 | clipart-rivi |

> **Huom. tulostuskoko.** `poster.css` rajaa clipartin korkeuden: yläreunan
> kuvat enintään **30 mm**, clipart-rivin kuvat enintään **36 mm**. Leveys
> skaalautuu mukana. Rajaus on pakollinen — juliste on kiinteä 420 mm korkea,
> ja ilman kattoa iso GIF työntää alaosan sivun ulkopuolelle.
>
> 36 mm on 300 dpi:llä ~425 px. Taulukon px-koot ovat *asettelua varten*; ota
> julisteeseen isoin saatavilla oleva versio, tai mieluiten SVG (Openclipart),
> joka skaalautuu terävänä mihin kokoon vain.
> Jos ajat aidolla 90-luvun GIF-rakeisuudella, se on tyylivalinta — pidä se
> tietoisena päätöksenä, älä vahinkona.

### `assets/buttons/` — 88 × 31 napit

| Tiedosto | Koko (px) | Missä |
|---|---|---|
| `tuki-1.gif` | 88 × 31 | footer + juliste |
| `tuki-2.gif` | 88 × 31 | footer + juliste |
| `tuki-3.gif` | 88 × 31 | footer + juliste |

Nämä ovat klassiset "web-napit". Käytä yhteistyökumppaneille, kaverifestareille
tai tee omat. Poista ylimääräiset `<span class="slot">`-lohkot jos tarvitset
vähemmän kuin kolme.

> **Tällä hetkellä nappirivit on kommentoitu pois** sekä `index.html`:n
> footerista että `poster.html`:n alaosasta. Poista kommenttimerkit kun
> nappeja on. Jos tukijoita ei tule, voit poistaa lohkot kokonaan.

### `assets/bg/`

| Tiedosto | Koko (px) | Missä |
|---|---|---|
| `stars-tile.gif` | 100–200 px neliö, saumaton | koko sivun tausta |

Jos et lisää tätä, CSS piirtää tähdet itse eikä mikään hajoa.

### `assets/social/` ja juuri

| Tiedosto | Koko | Missä |
|---|---|---|
| `og-image.png` | 1200 × 630 | some-jaon esikatselukuva |
| `favicon.ico` | 32 × 32 (juureen, ei tähän kansioon) | selaimen välilehti |

Nopein tapa `og-image.png`:iin: avaa `poster.html`, ota kuvakaappaus ja rajaa
1200 × 630.

---

## Mistä clipartia

### 1. Openclipart — <https://openclipart.org>

**Paras yleislähde.** ~182 000 kuvaa, kaikki **CC0 / public domain** — saa
käyttää mihin vain, myös kaupallisesti, ilman nimimainintaa. SVG-muodossa eli
skaalautuu julisteeseen terävänä.

Hyviä hakusanoja: `boombox`, `cassette`, `disco ball`, `dancing`, `star burst`,
`retro`, `roller skate`, `sunglasses`, `music note`, `lightning`.

### 2. GifCities — <https://gifcities.org>

**Aito tavara.** Internet Archiven hakukone yli 4,5 miljoonaan animoituun
GIF-kuvaan, jotka on kaivettu GeoCities-sivustoista. Tämä on kirjaimellisesti
sitä samaa clipartia jota käytit aikoinaan. Arkisto päivitti hakukoneen 2025,
ja nykyään haku on semanttinen — voit kuvailla mitä etsit ("dancing figure with
sunglasses") pelkkien tiedostonimien sijaan, ja rajata koon mukaan.

Klikkaus vie alkuperäiselle Wayback Machine -sivulle, mistä tiedoston saa
talteen.

**Tekijänoikeus:** nämä ovat arkistoituja tiedostoja, eivät vapautettuja
teoksia. Kukaan ei tiedä kuka ne teki. Pienen DIY-festarin julisteessa riski on
käytännössä olematon, mutta se ei ole sama asia kuin lupa — pidä tämä tiedossa
jos julisteesta joskus tulee myyntituote. Turvallisin veto: **ilme GifCitiesistä,
tiedostot Openclipartista.**

### 3. Wikimedia Commons — <https://commons.wikimedia.org>

Sekalainen mutta iso. Lisenssi vaihtelee kuvakohtaisesti — suodata
`Public domain` tai `CC0`, muuten joudut merkitsemään tekijän.

### 4. Public Domain Vectors — <https://publicdomainvectors.org>

Osin sama aineisto kuin Openclipartissa, joskus helpompi selata.
Kaikki public domain.

### 5. Alkuperäiset clipart-CD:t — Internet Archive

Hae arkistosta `clipart` + `CD-ROM`. Sieltä löytyy skannattuja 90-luvun
clipart-kokoelmia (Corel, Broderbund, Microsoft Publisher). Juuri oikea
ilme, mutta lisenssitilanne on sama harmaa alue kuin GifCitiesissä.

### 6. Muut aikakauden palikat

- **88×31-nappien arkisto:** <https://cyber.dabamos.de/88x31/> — tuhansia
  aitoja nappeja, myös tyhjiä pohjia omien tekemiseen.
- **Under construction -kuvat:** <http://www.textfiles.com/underconstruction/>
  — Jason Scottin kokoelma, satoja variaatioita.

---

## Nyrkkisääntö tyyliin

90-luvun koulun disco -juliste ei ollut suunniteltu, se oli **koottu**.
Se ilme syntyy näistä:

- **Liikaa fontteja.** Kolme on vähintään. WordArt kaartaa ja varjostaa.
- **Clipart ei liity toisiinsa.** Mankka, delfiini ja pizzaviipale samassa
  julisteessa, koska ne sattuivat olemaan samalla CD:llä.
- **Mitään ei kohdisteta.** Pieni kallistus on parempi kuin suora.
- **Tyhjä tila täytetään.** Jos jää rako, siihen tulee tähti.
- **Värit eivät sovi yhteen.** Se on piirre.
- **Fotokopiojälki.** Jos julisteen tulostaa mustavalkoisena ja kopioi pari
  kertaa, se näyttää oikeammalta kuin täydellinen väritulostus.
