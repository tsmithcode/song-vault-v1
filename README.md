# 3-Song Project Platform (Working Title)

A low-code, creator-first platform that helps artists, producers, and writers organize a **3-song project** into one clean package:

- Audio (final mixes + optional stems)
- Lyrics
- Credits (artist, producer, mix, master)
- Ownership & splits
- Exportable project summary (PDF/CSV-ready)

The core idea: **your files stay in your own storage (e.g., Google Drive)** and this platform simply **maps URLs + metadata** into a structured, studio-ready view.

---

## Table of Contents

- [Concept](#concept)
- [Features](#features)
- [User Flow](#user-flow)
- [System Architecture](#system-architecture)
- [Tech Stack](#tech-stack)
- [Screens & Routes](#screens--routes)
- [Data Model](#data-model)
- [Setup & Configuration](#setup--configuration)
  - [1. Framer](#1-framer)
  - [2. Outseta + Stripe](#2-outseta--stripe)
  - [3. Tally](#3-tally)
  - [4. Airtable](#4-airtable)
  - [5. Google Drive (User Files)](#5-google-drive-user-files)
- [Development Guidelines](#development-guidelines)
- [Roadmap](#roadmap)
- [License](#license)

---

## Concept

This project is a **guided 3-song intake and library experience**:

1. Charge **$1** to validate serious users.
2. Onboard them into a **3-song “project shell”**.
3. Provide a focused **Song Workspace** for each track.
4. Keep all heavy assets in the **user’s own Google Drive**.
5. Deliver a clean **summary** that labels, documents, and de-risks their music.

The UX is intentionally narrow: **exactly three tracks per project**, optimized for clarity, speed, and repeatability.

---

## Features

- **$1 beta signup** for a single 3-song project.
- **Creator onboarding wizard**:
  - Step 1: Creator profile (name, country, role).
  - Step 2: Project basics (title, genre, intended use).
  - Step 3: Track shells (3 titles, content type).
- **Creator Home (Mission Control)**:
  - “Current project” card with progress.
  - Library of all past 3-song projects.
- **Project Console**:
  - Project overview (artist, genre, use case).
  - Next best action (e.g., “Upload audio for Song 1”).
  - Table of 3 songs with Audio/Lyrics/Splits statuses.
- **Song Workspace** (per track):
  - Audio section (final mix + optional stems, Drive URLs supported).
  - Credits & technical data (DAW, tempo, key, mix/master engineers).
  - Lyrics section (collapsible).
  - Ownership & splits summary.
  - Readiness & export section.
- **Data ownership by design**:
  - Files live in user’s **Google Drive** (or any external storage).
  - Platform stores and presents **links + metadata**, not the files.

---

## User Flow

1. **Public Landing `/`**
   - Learn what the platform does.
   - Click **Start for $1**.

2. **Pricing `/pricing`**
   - See “3-Song Project Beta” plan.
   - Click **Continue – Pay $1**.

3. **Signup & Payment (Outseta/Stripe)**
   - Create account (email + password).
   - Pay $1 securely.
   - Redirect to onboarding.

4. **Onboarding Wizard**
   - `/onboard/project-setup?step=1` → Creator identity.
   - `/onboard/project-setup?step=2` → Project basics.
   - `/onboard/project-setup?step=3` → Name 3 tracks.

5. **Creator Home `/creator/home`**
   - See current project and library.
   - Click **Continue project**.

6. **Project Console `/creator/projects/[slug]`**
   - Review project status and song table.
   - Open a **Song Workspace**.

7. **Song Workspace `/creator/projects/[slug]/songs/[song-id]`**
   - Add audio/Drive URLs, credits, lyrics, splits.
   - Complete all 3 songs.
   - (Future) Export PDF/CSV summary.

---

## System Architecture

**Low-code architecture, minimal custom backend:**

- **Framer**  
  - UI, routing, layout, theming.  
  - Framer CMS for `Creators`, `Projects`, `Songs`.

- **Outseta + Stripe**  
  - Authentication (sign up / login / logout).  
  - Billing for the $1 beta plan.  
  - Basic CRM and account management.

- **Tally**  
  - Multi-step intake forms (onboarding, metadata updates).  
  - Pushes data into Airtable (or other sources).

- **Airtable**  
  - Primary structured data store for submissions.  
  - Source-of-truth for projects/songs in v0.  
  - Eventually synced into Framer CMS.

- **Google Drive (User-owned)**  
  - Users upload audio/stems to their own Drive.  
  - Platform stores **share URLs only** and maps them to projects/songs.

---

## Tech Stack

**Frontend & CMS**

- Framer (Waveform template as base)
- Framer CMS collections:
  - `Creators`
  - `Projects`
  - `Songs`

**Auth & Billing**

- Outseta (hosted auth + billing)
- Stripe (payment processing, $1 subscription or one-time)

**Forms & Data Intake**

- Tally forms (onboarding + song metadata forms)
- Tally → Airtable integration

**Storage & Links**

- Airtable base (`3_Song_Projects`) for structured records
- Google Drive for user file storage (Drive URLs stored in Airtable/Framer)

---

## Screens & Routes

All routes are conceptual; adapt to your production domain.

### 1. Public Landing — `/`

- Purpose: explain value, push to pricing.
- Primary CTA: **Start for $1** → `/pricing`.

### 2. Pricing / $1 Offer — `/pricing`

- Plan: “3-Song Project Beta” – **$1**.
- Included:
  - 3 songs per project
  - Rights snapshot
  - Private portal
- CTA: **Continue – Pay $1** → Outseta checkout.

### 3. Login — `/login`

- Outseta login widget.
- Link back to **Start for $1** for new users.

### 4. Signup & Payment — (Outseta/Stripe UX)

- Step 1: Create account
- Step 2: Pay $1
- Step 3: Confirmation → redirect to `/onboard/project-setup?step=1`

### 5. Onboarding Wizard

- `/onboard/project-setup?step=1` – Creator identity  
- `/onboard/project-setup?step=2` – Project basics  
- `/onboard/project-setup?step=3` – Name 3 tracks

### 6. Creator Home — `/creator/home`

- Current project card (progress bar, status).
- Library of all projects for this creator.

### 7. Project Console — `/creator/projects/[slug]`

- Project header (title, artist, genre, intended use).
- Next best action (usually upload audio).
- Song table:
  - Audio / Lyrics / Splits statuses.
  - Button per row: **Open Song Workspace**.

### 8. Song Workspace — `/creator/projects/[slug]/songs/[song-id]`

Sections:

1. Audio (final mix + stems; supports Drive URLs).
2. Credits & Tech (artist, producer, DAW, tempo, key, mix/master).
3. Lyrics (collapsible, full text).
4. Ownership & Splits (summary).
5. Export & Review (readiness + project completion status).

---

## Data Model

### Creators

| Field         | Type      | Description                        |
|--------------|-----------|------------------------------------|
| name         | Text      | Display name / stage name          |
| slug         | Slug      | URL-friendly handle                |
| email        | Text      | For mapping to Outseta/Airtable    |
| country      | Text/Enum | Country or region                  |
| primaryRole  | Enum      | Artist / Producer / Writer / All   |
| avatar       | Image     | Optional profile image             |

### Projects

| Field          | Type         | Description                             |
|----------------|--------------|-----------------------------------------|
| title          | Text         | Project title                           |
| slug           | Slug         | URL key                                 |
| creator        | Reference    | → Creators                              |
| genre          | Text/Enum    | Primary genre                           |
| intendedUse    | Text/Enum    | Demo / Label pitch / Distribution / etc |
| status         | Enum         | In setup / In review / Complete / etc   |
| year           | Text/Number  | Release year                            |
| labelImprint   | Text         | Label or “Independent”                  |
| goalSentence   | Text/Rich    | 1-line goal                             |
| songCount      | Number       | Usually 3                               |
| progressPercent| Number       | 0–100 overall                           |
| lastUpdated    | DateTime     | Last update timestamp                   |

### Songs

| Field          | Type         | Description                             |
|----------------|--------------|-----------------------------------------|
| title          | Text         | Track title                             |
| slug           | Slug         | URL key                                 |
| project        | Reference    | → Projects                              |
| trackNumber    | Number       | 1–3                                     |
| type           | Enum         | Explicit / Clean / Instrumental         |
| status         | Enum         | Not started / Audio only / etc          |
| audioUrl       | URL          | Final mix (can be Google Drive URL)     |
| stemsUrl       | URL          | Optional stems (Drive URL or zip)       |
| tempoBpm       | Number       | BPM                                     |
| key            | Text         | Musical key                             |
| daw            | Text         | DAW used                                |
| artistName     | Text         | Display artist                          |
| producerName   | Text         | Producer                                |
| mixEngineer    | Text         | Mix engineer                            |
| masterEngineer | Text         | Mastering engineer                      |
| lyrics         | Rich text    | Full lyrics                             |
| splitsSummary  | Rich text    | Ownership/splits summary                |
| rightsStatus   | Enum         | Draft / Confirmed                       |
| reviewNotes    | Rich text    | Internal notes (optional)               |

---

## Setup & Configuration

### 1. Framer

1. Create a **new Framer project**.
2. Start from the *Waveform* template or similar music-focused layout.
3. Define CMS collections:
   - `Creators`
   - `Projects`
   - `Songs`
4. Build pages using the route structure in [Screens & Routes](#screens--routes):
   - Public: `/`, `/pricing`, `/login`
   - Authenticated: `/creator/home`, `/creator/projects/[slug]`, `/creator/projects/[slug]/songs/[song-id]`
5. Connect components to CMS fields:
   - Project cards ↔ `Projects`
   - Song rows & workspaces ↔ `Songs`

### 2. Outseta + Stripe

1. Create an **Outseta** account.
2. Connect **Stripe** within Outseta.
3. Create a **$1 plan**:
   - Name: `3-Song Project Beta`
   - Price: `$1` (one-time or first-month).
4. Embed Outseta:
   - Add login widget to `/login`.
   - Add signup/checkout button to `/pricing`.
5. Configure redirect after checkout → `/onboard/project-setup?step=1`.

### 3. Tally

1. Create an **Onboarding Tally form** that matches:
   - Step 1 (Creator)
   - Step 2 (Project)
   - Step 3 (Song shells)
2. Create a **3-Song Intake form** (optional future):
   - Audio, lyrics, splits per track.
3. Enable Airtable integration:
   - Map Tally fields → Airtable base `3_Song_Projects`.

### 4. Airtable

1. Create base: **`3_Song_Projects`**.
2. Configure tables:
   - `Submissions` (raw Tally data).
   - `Projects` (normalized view).
   - `Songs` (normalized view).
3. Initially, sync **manually** into Framer CMS (copy/paste or CSV).
4. Later, add Whalesync/Make/Zapier to automate Airtable → Framer.

### 5. Google Drive (User Files)

1. In audio fields/forms, label clearly:
   - “Paste your Google Drive share link here.”
2. In documentation and UI, explain:
   - Users upload audio/stems to their **own Drive**.
   - This platform stores **links only**.
3. The `audioUrl` and `stemsUrl` fields simply map those Drive URLs into players/buttons.

---

## Development Guidelines

- **Branching model**
  - `main` – production configuration & docs.
  - `dev` – experimental Framer/Airtable/Tally configs and scripts.
- **Configuration**
  - Do not commit secrets (Outseta keys, Stripe keys, etc.).
  - Store environment-specific IDs (Airtable base IDs, Tally form IDs) in a separate config doc or `.env` if you add custom scripts.
- **Change management**
  - When changing CMS schema, update:
    - README
    - Airtable base
    - Tally forms
- **Testing**
  - Always run through full flow on a test account:
    - Landing → Pricing → $1 signup → onboarding → Home → Project → Song.

---

## Roadmap

Short term:

- [ ] Add export generator (PDF/CSV metadata pack).
- [ ] Implement “Next best action” logic per project.
- [ ] Add “Recently updated” and “Favorites” filters to Library.

Medium term:

- [ ] Airtable → Framer CMS sync (Whalesync/Make/Zapier/custom).
- [ ] Admin/reviewer view for internal notes & approvals.
- [ ] Public share links per project (opt-in).

Long term:

- [ ] Multi-project memberships (beyond a single 3-song set).
- [ ] API for labels/managers to pull data directly.
- [ ] Optional integrations with PROs, distributors, and sync libraries.

---

## License

Choose a license that matches your goals (MIT, Apache-2.0, commercial, etc.).

You can drop this README.md straight into your GitHub repo and update:
	•	The project name when you lock it in (e.g., SongVault, TrackLedger, Project3).
	•	Any details specific to your Framer project, Outseta plan name, or Airtable base IDs.
