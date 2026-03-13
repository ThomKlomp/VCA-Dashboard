# VCA Uren Dashboard

Een lichtgewicht, GitHub-gehost analytics dashboard dat XPS (workforce planning) en Moneybird (facturatie) combineert voor inzicht in VCA-uren.

## ✨ Wat doet het?

- 📊 **Overzicht** – KPI-kaarten: totale VCA-uren, omzet, aantal medewerkers met VCA, verlopen/bijna-verlopen certificaten
- 👷 **Medewerkers** – VCA-status per medewerker (type, behaaldatum, vervaldatum, geldigheidsvoortgang)
- 🧾 **Facturen** – Alle Moneybird-factuurregels met "VCA" in de omschrijving, inclusief uren en bedragen

## 🚀 Gebruik

### Optie 1 – Direct openen (geen server nodig)
1. Download `index.html`
2. Open in je browser
3. Klik op ⚙ **API Configuratie** en vul je sleutels in
4. Klik **Opslaan & Laden**

> 💾 Sleutels worden opgeslagen in `localStorage` van je browser — ze verlaten je computer niet.

### Optie 2 – GitHub Pages
1. Fork deze repository
2. Ga naar **Settings → Pages → Branch: main / root**
3. Open je dashboard op `https://jouwgebruikersnaam.github.io/vca-dashboard/`

---

## 🔑 API Configuratie

### XPS (XPSLogic)
| Veld | Beschrijving |
|------|-------------|
| **API Key** | Jouw XPS Bearer token (te vinden in XPS onder Instellingen → API) |
| **Base URL** | Doorgaans `https://secure.xpslogic.com/rps/root/api/v1` |
| **VCA veldnaam** | De naam van de vaardigheid/certificaat in XPS, bijv. `VCA` |

### Moneybird
| Veld | Beschrijving |
|------|-------------|
| **API Token** | Persoonlijk token via [moneybird.com/profile/tokens](https://moneybird.com/profile/tokens) |
| **Administratie ID** | Zichtbaar in de URL van je Moneybird-administratie |
| **VCA zoekterm** | Tekst waarnaar gezocht wordt in factuuromschrijvingen, bijv. `VCA` |

---

## 🔌 XPS API – endpoints

Het dashboard probeert automatisch de juiste endpoints te vinden. XPS gebruikt:

```
GET /persons?limit=500          → medewerkers ophalen
GET /employees?limit=500        → alternatief endpoint
```

Medewerkers worden gefilterd op de VCA-vaardigheid in het veld `skills`, `certificates`, `attributes` of `qualifications`.

**Neem contact op met XPS support** om te bevestigen welke endpoints en veldnamen in jouw installatie worden gebruikt, en pas `parseXPSEmployee()` in de code aan indien nodig.

---

## 🛠 Aanpassen

De logica voor het parsen van XPS-data staat in de functie `parseXPSEmployee()` in `index.html`. Pas deze aan op basis van de werkelijke response van jouw XPS-installatie:

```javascript
function parseXPSEmployee(p, vcaField) {
  // p = één medewerker-object van de XPS API
  // Zoek hier naar het juiste VCA-veld in jouw XPS response
}
```

### Moneybird
Factuurregels worden gefilterd op de zoekterm in `detail.description`. Als je VCA als aparte **grootboekrekening** hebt ingericht, kun je filteren op `ledger_account_id` in plaats van op de omschrijving.

---

## 📁 Bestanden

```
vca-dashboard/
├── index.html      # Het volledige dashboard (HTML + CSS + JS in één bestand)
└── README.md       # Deze documentatie
```

---

## ⚠️ CORS

De Moneybird API staat cross-origin requests toe vanuit de browser. De XPS API mogelijk niet — in dat geval heb je een kleine proxy nodig. Gebruik bijv. een Cloudflare Worker of draai het dashboard lokaal.

---

## 📄 Licentie

MIT — gebruik vrij, aanpassen welkom.
