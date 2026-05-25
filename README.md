# Teknisk dokumentation – [Klubhuset]

## Om projektet

Dette projekt er lavet som en del af vores eksamensprojekt i Tema 10.
Vi har lavet et dynamisk website med HTML, CSS og JavaScript, hvor indholdet bliver hentet fra et Rest API.
Vi har brygt dynamic routes til at lave undersiderne, så sidernes indhold bliver hentet som JSON fra Supabase. I Supabase har vi med afsæt i kundens credit-list oprettet Klubhusets datastruktur og API. Vi har kodet komponenter, herunder producer-cards, artist-card og brugt slot elementer.

Sitet består af flere sider, hvor brugeren kan:

- se en liste med artister
- bruge filtrering og sortering
- klikke sig videre til en detaljeside om artister
- læse om producerne på deres detaljesider

## Links

- GitHub repository: [https://github.com/team9-eksamen/klubhuset]
- GitHub Pages: [indsæt link]
- Figma: [https://www.figma.com/design/qsvK6OaNfMxNEf3aLr1CRu/2sem_t10_eksamen?node-id=18-10&t=fVdRkJEylo8qD7DZ-1]
- Trello: [https://trello.com/invite/b/69ef4d0510cf442fbe119024/ATTIb70297cee4a8ef3987e9f6e6c58a5394B3062D73/t10team9eksamen]

---

## Projektstruktur

Projektet er opdelt i Astro Pages, Layout, CSS og Components.

```
project/src/
├── pages/
│   ├── index.astro
│   ├── listview.astro
│   ├── mads.astro
│   └── details/[id].astro
├── css/
│   ├── custom.css
│   └── general.css
├── layouts/
│   ├── Layout.astro
│   ├── ArtistLayout.astro
│   ├── ProducerLayout.astro
│   └── form.js
├── components/
│   ├── AccordionContent.astro
│   ├── GetInTouch.astro
│   ├── IndexCard.astro
│   ├── ListCard.astro
│   ├── MicroLeft.astro
│   ├── MicroRight.astro
│   └── Welcome.astro
└── README.md
```

### Filbeskrivelser

- **Layout.astro** – implementerer vores html-skelet på astro pages
- **index.astro** – forside
- **listview.astro** – Henter data fra Supabase og viser en liste med artisterne på siden.
- **mads.astro** – viser detaljer om en valgt producer
- **details/[id].astro** – viser detaljer om en valgt artist
- **custom.css** – styrer designet
- **general.css** – styrer designet

---

## Hvordan koden fungerer

Astro-filerne – styrer det dynamiske indhold på de forskellige sider vha. JavaScript i fence

### index.astro

Bruges på forsiden.

<!-- ikke noget dynamisk på vores index, fordi producere er ikke oprettet i API? /Te -->

Her bliver indhold vist dynamisk, fx links eller kategorier.

### listview.astro

Henter data fra Rest API'et og viser en liste med artisters album på siden.

**Flow:**

1. Siden loader
2. JavaScript kører
3. Data hentes fra Rest API
4. Data bliver gennemgået med loop
5. HTML bliver indsat i DOM'en
6. Brugeren kan klikke på en artist

### details.js

Bruges til detaljesiden for artistens album. Den læser et id fra URL'en og henter derefter den rigtige artist fra Rest API'et.

Det gør det muligt at genbruge den samme HTML-side til mange artister. I stedet for at lave én side per artist/album, bruger vi ét id i URL'en til at vise det rigtige indhold.

### mads.astro

<!-- You good med det her?? /Te -->

Denne fil bruges til Klubhusets producere. På sigt skal den ligesom på listview for artister hente data fra et API for producerne, der viser udvalgte informationer for den valgte producer.

---

## Navngivning

Vi har navngivet vores filer, variabler og funktioner så de så tydeligt som muligt er selvforklarende.

### Eksempler på variabler

```javascript
// OBS CamelCase eller ændre i teknisk doku //TE
const valgtgenre;
const valgtproducer;
const allAlbums;
```

### Eksempler på funktioner

```javascript
btnactive(e);
filterproducer(e);
filtergenre(e);
sortactive(e);
```

// OBS CamelCase eller ændre i teknisk doku //TE
Vi har brugt camelCase i JavaScript, fordi det gør koden mere ensartet og lettere at læse.

---

## Kommentarer i koden

Vi har kommenteret de steder i koden, hvor det giver mening.
Fx ved funktioner, fetch-kald og steder hvor der sker DOM-manipulation.
Vi har prøvet ikke at skrive kommentarer til helt åbenlyse ting, men kun dér hvor det hjælper forståelsen.

**Eksempel:**

```javascript
// Henter album fra Rest API'et
export async function getStaticPaths() {
  const endpoint =
    "https://cslftqskycbszssbrjfq.supabase.co/rest/v1/artister?select=*";

  const options = {
    headers: { apikey: "sb_publishable_uJBiElMI8evEQetR2TtT2g_2o4KGmfP" },
  };
  const response = await fetch(endpoint, options);
  const data = await response.json();

  return data.map((project) => {
    return {
      params: { id: project.titel_album },
      props: { project },
    };
  });
}
```

---

## Data og JSON-struktur

Vi henter data fra et API i JSON-format.

**Et objekt kan fx se sådan ud:**

```json
{
  "id": 1,
  "titel_album": "Albumnavn",
  "artist": "Artistnavn",
  "description": "Short description",
  "genre": "Progressive rock",
  "year": "1977",
  "producer": "Mads Mølgaard",
  "role": ["Technician", "Co-producer"],
  "albumcover": "billede.jpg",
  "spotify": "link til spotify"
}
```

### Felter vi bruger

- **id** – bruges til at sende brugeren videre til detaljesiden
- **titel_album** – Navn på album
- **artist** – Navn på Artist/Band
- **description** – Kort beskrivelse af albummet
- **genre** – Type af genre
- **year** – Udgivelsesår
- **producer** – Navn på produceren
- **role** – producerens rolle (fx Technician, Co-producer, Master)
- **albumcover** – Thumbnail af albumcover
- **spotify** – Embedcode fra spotify med lydbider af sange fra albummet

---

## Git og branches

Vi har brugt GitHub til at samarbejde om projektet.

Vi har arbejdet med branches, så vi ikke sad og ændrede i det samme på samme tid.

Vi navngav branchene med feature først og navnet på den, der lavede branchen til sidst.

### Eksempler på branches

- `producer_filter_funktion_Camille`
- `teknisk_dokumentation_Te`
- `link_id_glitch_tekst_spotify_Camille`

### Workflow

1. Lave en branch med feature-navn og eget navn til sidst
2. Kode en feature
3. Committe ændringer
4. Pushe til GitHub
5. Merge til main når det virkede
6. Lav ny branch, hvis mangler/opdatering

Det gjorde det nemmere at holde styr på, hvem der lavede hvad.

---

## Bæredygtighed

<!-- Ej udfyldt endnu /TE -->
<!-- EHHHHHEHEHE skal vi lige opdatere efter tjek/TE -->

Vi har tænkt bæredygtighed ind i projektet ved at holde page weight under 250 kb samt en enkel informationasarkitektur.

**Tiltag:**

- Ingen videoer
- Ingen tunge frameworks
- Genbruge af kode
- Optimerede billeder

---

## Udfordringer undervejs

<!-- Ej udfyldt endnu /TE -->

En af vores udfordringer var at få data fra Rest API’et vist korrekt på siderne.

Det var også lidt svært at få id med videre i URL’en til detaljesiden.

**Løsninger:**

- Console.logge data undervejs
- Teste fetch-kald separat
- Bruge URLSearchParams
- Dele opgaverne mere tydeligt i gruppen

---

## Mulige forbedringer

<!-- Ej udfyldt endnu /TE -->

Hvis vi skulle arbejde videre med projektet, kunne vi forbedre det ved at tilføje:

- Søgefunktion
- Error handling
- Loading state

---

## Gruppemedlemmer

- Te Rahbæk
- Camille Elleholm Oddershede
- Oliver Lukas Hartmann Jakobsen
