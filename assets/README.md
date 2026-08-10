# Kuvamanifesti — mitä `assets/`-kansiossa on

**Kaikki kuvapaikat on täytetty.** Tämä tiedosto kertoo mitä missäkin on,
mistä se on peräisin ja miten sen vaihtaa.

## Miten kuvapaikka toimii

Kuvapaikka on `<span class="slot">`, jonka sisällä on `<img>`:

```html
<span class="slot slot--inline">
  <img src="/assets/clipart/boombox.svg" alt="" width="96" height="99">
</span>
```

Jos johonkin kohtaan haluaa väliaikaisen paikkamerkin, lisää `data-slot`-
attribuutti: `<span class="slot" data-slot="clipart/uusi.svg · 96×99">`.
Silloin ruudulle piirtyy vaaleanpunainen katkoviivalaatikko, jossa lukee
tiedostonimi ja koko, ja `<img>` piilotetaan. Poista attribuutti kun kuva on
paikallaan. Tyylit ovat `site.css`:ssä ja `poster.css`:ssä kohdassa
"PLACEHOLDER-SLOTIT".

---

## Miksi SVG eikä GIF

Alkuperäinen suunnitelma oli GIF, mutta juliste tulostetaan A3-kokoon, jossa
clipart on n. 36 mm leveä. 300 dpi:llä se on ~425 px, ja aikakauden GIF-kuvat
ovat tyypillisesti 100–200 px. Openclipartin SVG:t skaalautuvat terävinä mihin
kokoon vain, joten ne kelpaavat sekä sivustolle että painoon.

Jos haluat *tahallaan* rakeisen fotokopio-ilmeen, se on tyylivalinta — mutta
tee se tietoisesti, älä vahingossa liian pienellä bittikartalla.

---

## Sivuston kuvat

| Tiedosto | Piirtokoko | Missä |
|---|---|---|
| `clipart/sunburst.svg` | 120 × 60 | hero, otsikon yllä |
| `clipart/boombox.svg` | 96 × 99 | hero, otsikon alla |
| `clipart/badge-*.svg`, `clipart/new-badge.svg` | ks. alla | Esiintyjät-osio, otsikko ja bändien nimet |
| `clipart/under-construction.svg` | 120 × 90 | footer |
| `bg/stars-tile.svg` | 140 × 140, toistuva | koko sivun tausta |

## Julisteen kuvat

Korkeus lukitaan `poster.css`:ssä millimetreinä, leveys tulee kuvasuhteesta.

| Tiedosto | Korkeus tulosteessa | Missä |
|---|---|---|
| `clipart/star.svg` | 30 mm | yläreuna vasen |
| `clipart/party-popper.svg` | 30 mm | yläreuna oikea |
| `clipart/boombox.svg` | 36 mm | clipart-rivi |
| `clipart/break-dancer.svg` | 36 mm | clipart-rivi |
| `clipart/discoball.svg` | 36 mm | clipart-rivi |

## Vilkkuvat merkit

| Tiedosto | Teksti | Leveys | Missä |
|---|---|---|---|
| `clipart/badge-best.svg` | BEST! | 70 | Esiintyjät-otsikko |
| `clipart/badge-wow.svg` | WOW! | 64 | Trischloss |
| `clipart/badge-cool.svg` | COOL! | 70 | Jännite |
| `clipart/badge-rules.svg` | RULES! | 76 | Tornion syntymättömät ministerit |
| `clipart/badge-love.svg` | LOVE! | 70 | Mielikarkki |
| `clipart/badge-hot.svg` | HOT! | 64 | Pahennustakki |
| `clipart/new-badge.svg` | NEW! | 44 | Piehingin perkele |

Korkeus on aina 20 px. Vaihtaminen: vaihda `src` ja `width` — `width`
**täytyy** vastata SVG:n omaa leveyttä, muuten merkki venyy.

Merkit ovat koristeita, joten `alt=""`. Ruudunlukija ohittaa ne, eikä
bändin nimen perään lueta "WOW".

Uuden merkin tekeminen: kopioi jokin `badge-*.svg`, vaihda teksti, värit
(`fill`, `stroke`) ja `animation`-kesto. Kestot on tahallaan eri pituisia
(0.8–1.7 s), jotta merkit eivät vilku tahdissa — se näyttää liian
järjestelmälliseltä.

**Vilkkuminen pysähtyy** käyttöjärjestelmän "vähennä liikettä"
-asetuksella. Älä poista `@media (prefers-reduced-motion: reduce)`
-lohkoa: vilkkuva sisältö on osalle ihmisistä oikeasti ongelma.

Julisteessa merkkejä ei ole — paperilla ei vilku mikään.

## QR-koodi

`qr-instagram.svg` — osoitteeseen `https://instagram.com/eipavissiinfestival`.
Julisteen alareunassa, 28 mm.

Vektorina eikä bittikarttana, koska JPEG-pakkauksen artefaktit mustan ja
valkoisen rajalla haittaavat lukemista pienessä koossa.
Virheenkorjaustaso **Q** (n. 25 %), koska julistetta todennäköisesti
valokopioidaan.

Kaksi asiaa joita ei saa muuttaa:

- **Valkoinen tausta.** Musta koodi tummalla pohjalla ei luota lainkaan.
- **Quiet zone.** Reunan 4 moduulin tyhjä alue sisältyy viewBoxiin. Jos
  rajaat sen pois, koodi lakkaa toimimasta vaikka näyttää ehjältä.

28 mm on lähellä alarajaa: nyrkkisääntö on että koodi luetaan noin
kymmenkertaiselta etäisyydeltä, eli tästä n. 28 cm:stä.

Uudelleenluonti, jos osoite muuttuu:

```sh
python3 -m venv /tmp/qrvenv && /tmp/qrvenv/bin/pip install qrcode
/tmp/qrvenv/bin/python - <<'EOF'
import qrcode
qr = qrcode.QRCode(error_correction=qrcode.constants.ERROR_CORRECT_Q, border=4)
qr.add_data("https://instagram.com/eipavissiinfestival"); qr.make(fit=True)
m = qr.get_matrix(); n = len(m)
path = "".join(f"M{x},{y}h1v1h-1z" for y,r in enumerate(m) for x,v in enumerate(r) if v)
open("assets/qr-instagram.svg","w").write(
  f'<svg xmlns="http://www.w3.org/2000/svg" width="{n}mm" height="{n}mm" viewBox="0 0 {n} {n}">'
  f'<rect width="{n}" height="{n}" fill="#fff"/><path d="{path}" fill="#000"/></svg>')
EOF
```

**Tarkista aina lopputulos puhelimella ennen painoa.** Rakenteellisesti
kelvollinen QR voi silti olla väärä.

## Vielä tyhjä: `assets/buttons/`

88 × 31 -napit tukijoille tai kaverifestareille. Nappirivit on **kommentoitu
pois** sekä `index.html`:n footerista että `poster.html`:n alaosasta — poista
kommenttimerkit kun nappeja on, tai poista lohkot kokonaan jos tukijoita ei
tule. Valmiita nappeja ja tyhjiä pohjia: <https://cyber.dabamos.de/88x31/>

## Generoidut kuvat

| Tiedosto | Koko | Lähde |
|---|---|---|
| `../favicon.png` | 64 × 64 | `favicon-src.svg` |
| `social/apple-touch-icon.png` | 180 × 180 | `favicon-src.svg` |
| `social/og-image.png` | 1200 × 630 | `og-image.svg` |

Uudelleenrenderöinti (repon juuresta):

```sh
cd assets
rsvg-convert -w   64 -h  64 favicon-src.svg -o ../favicon.png
rsvg-convert -w  180 -h 180 favicon-src.svg -o social/apple-touch-icon.png
rsvg-convert -w 1200 -h 630 og-image.svg    -o social/og-image.png
```

> **Miksi `og-image.svg` on `assets/`-kansion juuressa eikä `social/`:ssa:**
> librsvg lataa ulkoisia kuvaviittauksia vain samasta kansiosta tai sen
> alikansioista. `social/`:sta katsottuna `../clipart/...` ei latautuisi, ja
> clipart jäisi hiljaisesti pois renderöidystä kuvasta — virheilmoitusta ei
> tule, kuva vain on tyhjä.
>
> **Fontti:** `og-image.svg` käyttää yleistä sans-serifiä, ei Impactia, jotta
> se renderöityy samannäköisenä myös koneella jolla Impactia ei ole. Otsikko
> ei siis ole pikselintarkasti sama kuin sivustolla.

---

## Mistä nämä ovat peräisin

Kaikki arkistokuvat ovat **Openclipartista** ja **CC0-lisenssillä** (public
domain) — vapaasti käytettävissä myös kaupallisesti, ilman nimimainintaa.
Lähteet on kirjattu tähän silti, jotta ne voi tarkistaa ja korvata.

| Tiedosto | Openclipart-tunnus | Alkuperäinen nimi |
|---|---|---|
| `clipart/sunburst.svg` | [349170](https://openclipart.org/detail/349170/) | half-starburst-rainbow-rays |
| `clipart/boombox.svg` | [334706](https://openclipart.org/detail/334706/) | 1980s-boombox |
| `clipart/under-construction.svg` | [340982](https://openclipart.org/detail/340982/) | under-construction |
| `clipart/star.svg` | [215675](https://openclipart.org/detail/215675/) | gold-fivepointed-star |
| `clipart/party-popper.svg` | [329042](https://openclipart.org/detail/329042/) | party-popper |
| `clipart/break-dancer.svg` | [310285](https://openclipart.org/detail/310285/) | break-dancing-kid |
| `clipart/discoball.svg` | [346657](https://openclipart.org/detail/346657/) | disco-ball |

Itse tehdyt (ei ulkoista lisenssiä):

| Tiedosto | Miksi itse tehty |
|---|---|
| `clipart/badge-*.svg`, `clipart/new-badge.svg` | Tekstillisiä merkkejä — sisältö halutaan hallita itse. Vilkkuvat CSS-animaatiolla, joka pysähtyy `prefers-reduced-motion`-asetuksella. |
| `bg/stars-tile.svg` | Tiilen pitää toistua saumattomasti; arkistokuvat eivät toistu. |
| `favicon-src.svg`, `og-image.svg` | Koosteita yllä olevista. |

> **Huom. XML-kommentit SVG:ssä.** SVG on XML, eikä XML-kommentin sisällä saa
> esiintyä kahta peräkkäistä tavuviivaa. Jos kirjoitat kommenttiin CSS-muuttujan
> sen oikeassa muodossa, tiedosto lakkaa jäsentymästä ja selain jättää koko
> kuvan lataamatta ilman näkyvää virhettä. Tämä ehti jo kerran tapahtua
> `stars-tile.svg`:lle.

## Vaihtaminen

1. Lataa uusi SVG Openclipartista: `https://openclipart.org/download/<id>/`
2. Tallenna `assets/clipart/`-kansioon.
3. Päivitä `src` ja `width`/`height` HTML:ssä. Pidä kuvasuhde oikeana —
   `site.css`:n `img { height: auto }` laskee korkeuden leveydestä, joten
   väärä `height` ei venytä kuvaa, mutta varaa väärän tilan latauksen ajaksi.
4. Julisteessa korkeus tulee `poster.css`:stä, joten sinne ei tarvitse koskea.

Tarkista lopuksi että tiedosto on kelvollista XML:ää:

```sh
python3 -c "import xml.dom.minidom,sys; xml.dom.minidom.parse(sys.argv[1])" tiedosto.svg
```


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
