# AI Context – Cloud Identity Summit Website

> Dieses Dokument am Anfang einer neuen KI-Session hochladen, um den vollständigen Projektstand zu vermitteln.

---

## Projekt-Übersicht

**Projekt:** Migration der Cloud Identity Summit Website von WordPress zu Astro Static Site  
**Live-URL:** https://identitysummit.cloud  
**Staging-URL:** https://identitysummit.z1.web.core.windows.net  
**GitHub Repo:** https://github.com/GregorReimling/website-identitysummit  
**Lokales Verzeichnis:** `C:\Users\GregorReimling\OneDrive - BuildClouds\StaticWebsites\identitysummit`

---

## Tech Stack

| Komponente | Details |
|---|---|
| Framework | Astro 6 (Static Output) |
| Node.js | 22 (required by Astro 6) |
| Fonts | Syne (Display, 800) + DM Sans (Body, 300/400/500) |
| Hosting | Azure Blob Storage Static Website (`identitysummit` Storage Account) |
| Region | `germanywestcentral` |
| Resource Group | `identitysummit_rg` |
| Subscription | `27a2c2c1-3244-4d2b-81fb-295710fdbe6d` |
| CI/CD | GitHub Actions mit OIDC / User Assigned Managed Identity |
| Managed Identity | `identity-summit-deployer` |
| Agenda/Speaker | Sessionize Event ID: `zypgzww2` |

---

## Azure / GitHub Actions Setup

- **Managed Identity:** `identity-summit-deployer` (User Assigned, separiert von reimling.eu)
- **Role:** `Storage Blob Data Contributor` auf Storage Account Scope
- **Federated Credential:** `repo:GregorReimling/website-identitysummit:ref:refs/heads/main`
- **GitHub Secrets:** `AZURE_CLIENT_ID`, `AZURE_TENANT_ID`, `AZURE_SUBSCRIPTION_ID`
- **Workflow-Datei:** `.github/workflows/deploy.yml`
- **Deploy-Ziel:** `$web` Container im Storage Account `identitysummit`
- **Node.js:** 24 (FORCE_JAVASCRIPT_ACTIONS_TO_NODE24: true gesetzt)

---

## Projektstruktur

```
src/
  layouts/
    BaseLayout.astro        ← Haupt-Layout (HTML Head, Fonts, global.css)
    Layout.astro            ← Alternatives Layout (von Agenda/Speaker genutzt)
  components/
    nav.astro               ← Navigation (2 Ebenen, Past Identity Summits Dropdown)
    cityhero.astro          ← Hero mit Skyline-Foto + Countdown
    footer.astro            ← Footer-Komponente
  pages/
    index.astro             ← Home (Hero + Event Info + Past Events)
    agenda.astro            ← Sessionize GridSmart Embed
    speaker.astro           ← Sessionize SpeakerWall Embed
    sponsors.astro          ← Gold/Silver/Bronze aus Content Collection
    team.astro              ← Orga-Team (noch zu bauen)
    arrival.astro           ← Arrival & Hotels (TBD content)
    tickets.astro           ← Tickets (TBD content)
    impressum.astro         ← Impressum (§5 TMG)
    datenschutz.astro       ← Datenschutzerklärung
    code-of-conduct.astro   ← CoC
    sponsors/
      call-for-sponsors.astro ← Sponsoring-Pakete als Bilder
    archive/
      [year]/               ← Dynamische Routen (noch zu bauen)
  content/
    team/
      gregor.json
      rene.json
      thomas.json
    sponsors/
      2026.json             ← Gold: adesso, Glück und Kanja, Token2
    agenda/
      2026.json             ← Placeholder (echte Agenda via Sessionize)
    speaker/
      placeholder.json
  content.config.ts         ← Astro 6 Content Collections (glob loader)

public/
  images/
    frankfurt-skyline.jpg   ← Hero-Bild (Foto: dagobert1980, Pixabay)
    adesso/
      adesso-logo-2025.png
    guk/
      guk-logo.png          ← Glück und Kanja
    token2/
      token2-logo.png
    sponsoring/
      CIS-Sponsorship-2026-Websiteversion-0001.png  ← bis 0006
  styles/
    global.css              ← CSS Custom Properties, Reset
  favicon.svg
```

---

## Design System

```css
--bg-base:    #09090f;   /* Page Background */
--bg-card:    #0f0f1a;   /* Cards */
--bg-raised:  #141425;   /* Raised Elements */
--border:     rgba(120,120,200,0.15);
--accent-1:   #6c8fff;   /* Electric Blue – Links, Nav Active */
--accent-2:   #a259ff;   /* Violet – Gradient End, Badges */
--accent-3:   #00e5c0;   /* Teal – Tags, Status */
--text-high:  #f0f0ff;
--text-mid:   #9898b8;
--text-low:   #4e4e72;
--font-display: 'Syne', sans-serif;
--font-body:    'DM Sans', sans-serif;
```

**Skyline-Konzept:** Hero-Bild wechselt pro Jahr via CSS `--hero-city-image`. Overlay + Farben bleiben konstant.

---

## Event-Daten 2026

| | |
|---|---|
| Datum | 3. November 2026 |
| Stadt | Frankfurt, Germany |
| Adresse | noch nicht bekannt |
| Format | Hybrid (in-person + Stream) |
| Eintritt | Kostenlos |

---

## Sponsoren 2026

**Gold:**
- adesso SE → https://www.adesso.de → `adesso-logo-2025.png`
- Glück und Kanja → https://www.glueckundkanja.com → `guk-logo.png`
- Token2 → https://www.token2.com → `token2-logo.png`

**Silver / Bronze:** noch offen

---

## Orga-Team

| Name | Rolle | Links |
|---|---|---|
| Gregor Reimling | Co-Organizer, Azure MVP, Chief Azure Technologist @ adesso | reimling.eu |
| René de la Motte | Co-Organizer | linkedin.com/in/renedelamotte |
| Thomas Naunheim | Co-Organizer, Identity Expert | cloud-architekt.net |

---

## Sessionize Integration

- **Event ID:** `zypgzww2`
- **Agenda:** `https://sessionize.com/api/v2/zypgzww2/view/GridSmart`
- **Speaker Wall:** `https://sessionize.com/api/v2/zypgzww2/view/SpeakerWall`
- **Session List:** `https://sessionize.com/api/v2/zypgzww2/view/SessionList`
- **Speaker List:** `https://sessionize.com/api/v2/zypgzww2/view/SpeakerList`

---

## Offene To-Dos

- [ ] `/team` Seite bauen (Komponente TeamCard, Fotos der drei Orgas einbinden)
- [ ] `/archive/[year]` dynamische Routen aufbauen (2020–2025)
- [ ] Sponsoring-Bilder nach `public/images/sponsoring/` kopieren
- [ ] Hero-Countdown debuggen (war defekt, Fix eingespielt – verifizieren)
- [ ] Favicon durch CIS-eigenes Icon ersetzen
- [ ] `@astrojs/sitemap` für SEO hinzufügen
- [ ] `package.json` name von `stellar-shepherd` auf `cloud-identity-summit` ändern
- [ ] Arrival-Seite mit echten Infos befüllen sobald Location bekannt
- [ ] Tickets-Seite mit Registrierungslink befüllen
- [ ] Design-Review (Sessionize Widget Dark Mode Overrides verfeinern)
- [ ] Domain-Migration: `identity-summit.cloud` → `identitysummit.cloud`

---

## Wichtige Konventionen

- **Dateinamen:** immer lowercase, keine Leerzeichen, Bindestriche statt Underscores
- **Komponenten-Imports:** exakte Groß-/Kleinschreibung beachten (Linux ist case-sensitive)
- **Bestehende Dateien:** immer als Basis nehmen und nur notwendige Stellen ändern
- **Git:** Gregor führt git-Befehle selbst aus (kein direktes Pushen durch KI)
- **CMD:** Gregor arbeitet in Windows CMD – keine Bash-Subshells (`$(...)`)
- **Build-Test:** immer `npm run build` vor jedem commit

---

## Workflow für neue Sessions

1. Dieses Dokument hochladen
2. Ggf. relevante `.astro`-Dateien hochladen wenn Änderungen daran geplant sind
3. KI kennt dann den vollständigen Kontext ohne weitere Erklärungen
