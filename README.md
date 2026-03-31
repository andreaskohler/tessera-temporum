# Tessera Temporum

> *Tessera* — the Roman token used to establish connections between people across time and distance.

A historical knowledge mapping application. See what was happening in Tang Dynasty China while Charlemagne ruled in Europe. Watch the Abbasid golden age flourish during Europe's Dark Ages. Trace philosophical lineages from Socrates through Aristotle to Aquinas. Map the parallel clocks of civilizations that never knew each other existed.

---

## What it does

Tessera Temporum places ~480 historical figures from across world history on a shared interactive timeline. Every person, every dynasty, every epoch rendered in parallel — so you can read history sideways, not just forward.

**Timeline view**
- Zoomable horizontal timeline from 3000 BC to the present
- Lifespan bars colored by discipline (Philosophy, Politics, Military, Painting, Architecture, Literature, Music, Science, Religion)
- Office/reign bars overlaid showing exact time in power — Putin's two presidential terms, Grover Cleveland's two non-consecutive presidencies, all correct
- Hover tooltips with birth/death dates, time in office, age at entry, party affiliation, Wikipedia link
- Sort by birth year or office start — see US presidents in succession order, Soviet leaders in correct sequence

**Multi-civilization epoch rows**
- 10 independent civilization clocks: Western, China, Islamic World, India, Japan, Byzantine, Mesopotamia, Egypt, Africa, Americas
- Toggle any combination — see Tang Dynasty China alongside Carolingian Europe alongside Abbasid Baghdad
- Epoch boundary lines cascade down through all person rows

**Sidebar filters (6 independent dimensions)**
- Field → sub-discipline (e.g. Art → Impressionism, Baroque, Cubism)
- Region → Continent → Country → Historical period (3 levels)
- Dynasty → Country group → Dynasty (chronologically sorted)
- Role/Office → Head of State / Military Command / Religious Leadership / Government / Intellectual
- Epoch (Western)
- Show Epochs (civilization selector)

**Toolbar**
- Office View toggle — split multi-term politicians into one row per term
- Sort: Birth ↔ Sort: Office
- Role dropdown — context-sensitive list of roles in current view (President, General Secretary, Pope, etc.)
- Wikipedia links on all hover popovers and detail panel

---

## Coverage

| Region | Figures |
|--------|---------|
| England / Britain | 47 monarchs (Æthelstan → Charles III) + modern PMs |
| Russia | Tsars (Ivan III → Nicholas II) + Soviet leaders + Presidents |
| China | Emperors (Qin Shi Huang → Puyi) + CCP leaders |
| USA | All 45 presidents (Washington → Biden/Trump) |
| France | Kings + all Republic presidents (de Gaulle → Macron) |
| Germany | Holy Roman Emperors + modern Chancellors |
| Africa | Mali, Songhai, Zulu, Nubian, Ethiopian empires |
| Americas | Maya, Aztec, Inca + Independence leaders |
| Philosophy | Ancient Greece through 20th century, East and West |
| Painting | Renaissance through Surrealism |
| Architecture | Imhotep through Zaha Hadid |
| Literature | Homer through García Márquez |
| Music | Bach through Shostakovich |
| Popes | Key pontiffs from Leo I to Francis |
| Military | Sun Tzu, Hannibal, Saladin, Rommel, Zhukov and more |

---

## Architecture

**Current: Static single-file prototype**

```
chronos.html        — complete app, ~240KB, zero dependencies, zero build step
```

Open it in a browser. That's it.

**Planned: Flask + PostgreSQL + GCP**

```
frontend/           — Vue or vanilla JS
backend/
  app.py            — Flask API
  models.py         — SQLAlchemy (Person, Term, Dynasty, Relationship)
  migrations/       — Alembic
Dockerfile
cloudbuild.yaml     — GCP Cloud Build → Cloud Run
```

GCP stack:
- **Frontend**: Cloud Storage (static hosting)
- **Backend**: Cloud Run (Flask container)
- **Database**: Cloud SQL (PostgreSQL)
- **Secrets**: Secret Manager
- **CI/CD**: Cloud Build triggered by GitHub push

---

## Running locally

```bash
# Just open the file
open chronos.html

# Or serve it (avoids any browser restrictions)
python3 -m http.server 8080
# → http://localhost:8080/chronos.html
```

---

## Deploying to GCP (static)

```bash
# Set your project
gcloud config set project YOUR_PROJECT_ID

# Deploy to Cloud Storage
gsutil cp chronos.html gs://tessera-temporum/index.html
gsutil acl ch -u AllUsers:R gs://tessera-temporum/index.html

# Enable static website hosting on the bucket
gsutil web set -m index.html gs://tessera-temporum
```

With GitHub Actions (see `.github/workflows/deploy.yml`), every push to `main` deploys automatically.

---

## Data model

Each person has:

```javascript
{
  id, name, birth, death,
  region,           // England, Russia, China, USA...
  fields,           // [Philosophy, Politics, Military, Painting, Music...]
  schools,          // [Stoicism, Impressionism, Baroque, Romanticism...]
  subregions,       // [Tudor, Han, Early Republic...]
  dynasty,          // House of Windsor, Ming Dynasty, Romanov...
  party,            // Republican, Democratic, Labour...
  roles,            // [Head of State, Military Commander, Religious Leader...]
  terms: [          // explicit office periods
    { role, org, start, end }
  ],
  orgs: [           // display strings
    { name, role }
  ],
  bio,              // one-paragraph biography
  wiki              // Wikipedia title (if different from name)
}
```

---

## Planned features

- [ ] Network/graph view — force-directed relationship graph (taught, influenced, contemporary, opposed)
- [ ] Table view — sortable, exportable CSV/XLS
- [ ] Add Person AI panel — type a name, Anthropic API returns structured data, confirm and save
- [ ] Flask backend with PostgreSQL
- [ ] GCP deployment pipeline
- [ ] Vice Presidents, Foreign Ministers, other roles
- [ ] Japanese figures, Islamic scholars, Indian rulers
- [ ] More African and South American coverage

---

## Name

A *tessera* in ancient Rome was a small token — a tile, a tablet, a token of recognition. Two people would break a tessera in half and keep one piece each. Years or generations later, the matching halves proved a connection, a relationship, a debt of hospitality. The tessera was the original linked record.

*Temporum* — of time.

---

*Built with Claude. No frameworks, no build steps, no npm install. Just history.*
