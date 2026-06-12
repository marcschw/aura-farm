# CLAUDE.md — „AURA FARM“ 🌱✨
### Gamifizierte Pop-Culture-Checkliste für einen MGIN-Lehrer (Bioinformatik-Zweig, HTL Wien)

> Build-Auftrag für einen anderen Thread / Claude Code. Diese Datei ist die **Spec + der komplette Content-Katalog**. Alles Nötige steht hier drin — bau die App genau danach.

---

## 1 · Was das wird (one-liner)

Eine **mobile-first Web-App**, mit der ein Lehrer abhakt, welche Popkultur er sich „geben“ sollte, um seine 17–19-jährigen Schüler:innen (Bioinformatik-Zweig, ~50/50 Geschlecht) zu verstehen. Jeder Eintrag zeigt: **wie wichtig**, **wie viel Zeit**, **wofür man's im Unterricht braucht**, **wie viel man konsumieren muss** (1 Folge / charakteristische Episode / ganze Staffel / nur Konzept), und gibt beim Abhaken **Aura** (Punkte). Bei **Games** zusätzlich **Sub-Checks**: was man im Spiel ausprobiert haben sollte + kurze Anleitung. Quests, Level, Badges, Streaks. Slang der Zielgruppe ist bewusst ins UI eingebaut (ironisch, abschaltbar).

**Tonalität:** selbstironischer Lehrer, der „aura-farmt“. Das UI spricht Gen-Z-Slang, aber augenzwinkernd — nicht cringe-overload.

---

## 2 · Tech-Constraints (hart)

- **Single-File-Prinzip:** ein `index.html` mit eingebettetem CSS + JS + dem JSON-Katalog. Zero-Build, läuft per Doppelklick und auf jedem Webhost.
- **Keine Datenbank, kein Login, kein Backend.** Persistenz **nur über `localStorage`** (Häkchen, Sub-Check-Häkchen, Aura, Level, Streak, Settings). Beim Laden hydrieren, bei jeder Änderung speichern. Try/catch drumherum.
- **Mobile-first**, aber im Desktop-Browser genauso schön. Touch-Targets ≥ 44px.
- **Installierbar als App** (PWA): zusätzlich ein minimales `manifest.webmanifest` + Inline-SVG-App-Icon + optional ein winziger Service Worker fürs Offline-Caching. Wenn SW zu viel Aufwand: manifest + Icon reichen für „Add to Home Screen“.
- **Bilder ressourcenschonend:** KEINE schweren Bild-Fetches/Generierungen. Pro Eintrag ein **Emoji-„Poster“** + ein **CSS-Gradient**, eingefärbt nach Medium/Tier. Optional 1 generiertes SVG-Hero-Icon fürs Branding. Das war's.
- Alles **offline-fähig** außer den externen „Open“-Links.

---

## 3 · Visuelles Design

- **Vibe:** „Farm-meets-Y2K-gradient“. Dunkler Hintergrund, neon-gradient Akzente (lila→cyan→lime), dicke abgerundete Cards, große verspielte Zahlen für Aura. Leichtes Glow/Sparkle bei Aktionen.
- **App-Name groß:** `AURA FARM 🌱` mit Untertitel `der Pop-Culture-Grind`.
- **Tier-Farben:** S = lime/gold (essential), A = cyan, B = lila, C = grau (mid/skippable).
- **Medium-Icons (Emoji):** serie 📺 · film 🎬 · game 🎮 · app 📱 · musik 🎵 · meme 🤡 · reality 🌹 · gameshow 🎙️ · youtube ▶️ · influencer 🤳 · buch 📚 · anime 🍥 · podcast 🎧 · sonstiges ✨ · rapport 🧸 · pack 📦
- **App-Icon (SVG):** ein leuchtender Sämling/Sprout 🌱 in einem Aura-Glow-Kreis mit Sparkles, lila→lime Gradient. Crisp, flat, erkennbar bei 48px. Inline-SVG + im Manifest referenziert.
- Mikro-Animationen: Häkchen → kurzer Confetti/Sparkle-Burst + Aura-Counter zählt hoch.

---

## 4 · Gamification-System

### Aura (= die Punkte)
- Jeder Eintrag hat `points` (5–50). **Wichtigeres = mehr Aura.** S-Tier 50, A 30, B 20, C 10, Packs 15.
- Abhaken → `+points Aura`, mit Slang-Toast. Uncheck → Aura wieder abziehen.
- **Sub-Checks** (nur Games): jeder Sub-Check gibt kleine Aura (`5`). Alle Sub-Checks eines Spiels erledigt → das Spiel gilt als „fully experienced“ → **Bonus-Aura + „Pro-Gamer“-Stempel** auf der Card.

### Level (Schwellen an Gesamt-Aura)
| Aura | Titel |
|---|---|
| 0 | `NPC Teacher` |
| 150 | `Lowkey Aware` |
| 400 | `Got Rizz` |
| 800 | `Sigma Educator 🗿` |
| 1400 | `Final Boss of Aura 🔥` |

Level-Up triggert Vollbild-Glow + Slang-Message.

### Quests (Item-Bundles → Bonus-Aura + Badge)
- **🎬 Showrunner** — die 5 Franchise-Skins: Stranger Things, Wednesday, Cassandra, Squid Game, Are You The One? → *+100, Badge „Showrunner“.* (= unsere Unterrichts-Skins.)
- **🤡 Brainrot Certified** — Italian Brainrot + Skibidi + „6-7“ + Sigma → *Badge „Certified Brainrot Scholar“.*
- **📊 Statistician's Playlist** — Spotify Wrapped + Panini + Gangnam Style + F1 → *Badge „Aura Farmer of Numbers“.*
- **🤖 AI Arc** — ChatGPT + Cassandra + Brainrot + KPop Demon Hunters → *Badge „Prompt Wizard“.*
- **🎮 Touch Grass... Later** — alle Games „fully experienced“ (alle Sub-Checks) → *Badge „Certified Gamer“.*
- **🧸 Nostalgia Arc** — alle Nostalgie-Packs → *Badge „Real One“.*
- **⚡ Speedrun** — alle Items mit ≤15 Min → *Badge „No Time To Grass“.*

### Streak
- „Daily Grind“: an aufeinanderfolgenden Tagen ≥1 Item abhaken → 🔥-Counter.

### Badges
- Pro abgeschlossener Kategorie + pro Quest + „Pro-Gamer“ pro voll gespieltem Game. Badge-Regal im Profil.

### „Cringe Mode“ Toggle
- Schalter oben: **Brainrot Mode (Slang an)** ↔ **Lehrerzimmer Mode (normal)** — falls Kolleg:innen draufschauen. In `localStorage`.

---

## 5 · Features / UX

- **Listenansicht** mit Cards. Pro Card: Emoji-Poster, Titel, Tier-Badge, Aura, Medium-Chip, **Dosis** (wie viel ansehen), **Zeit** (Min), 1-Zeilen **Lehr-Use** („📚 …“), Checkbox, „Open ↗“ falls `url`.
- **Games:** Card ist **aufklappbar** → zeigt die **Sub-Checks** als eigene kleine Checkboxen, jeweils mit `task` + aufklappbarer `how`-Anleitung (Tooltip/Akkordeon). Fortschritt „3/5 erlebt“.
- **Packs:** Card listet im Detail-Sheet alle enthaltenen Titel auf (eine Sammel-Checkbox).
- **Filter (Chips):** nach Medium. Schnellfilter „Nur ESSENTIAL“, „≤15 Min“, „Noch offen“, „Nur Games“.
- **Sortierung:** `Zeit (Speedrun)` ↑ / `Aura (Grind)` ↓ / `Wichtigkeit (Tier)` / `Vibe (random)`.
- **Suchfeld.**
- **Fortschritts-Header:** Gesamt-Aura groß, Level-Titel, Streak 🔥, Progressbar zum nächsten Level, % erledigt.
- **Detail-Sheet** beim Tap: längere Lehr-Notiz, Unterrichts-Block, Link, (Games: Sub-Checks).
- **Reset-Button** (mit „sicher?“, löscht `localStorage`).

### Dosis-Enum (Anzeige + Zeit-Sortierung)
`quick` (5–15 min) · `episode` (1 charakteristische Folge ~25–55 min) · `film` (~120 min) · `season` (Staffel, Stichprobe reicht) · `konzept` (nur wissen worum's geht ~10 min) · `sample` (mal reinschauen) · `play` (Games — siehe Sub-Checks).

---

## 6 · Datenmodell (Item-Schema)

```json
{
  „id“: „string“,
  „title“: „string“,
  „medium“: „serie|film|game|app|musik|meme|reality|gameshow|youtube|influencer|buch|anime|podcast|sonstiges|rapport|pack“,
  „tier“: „S|A|B|C“,
  „points“: 50,
  „estMinutes“: 40,
  „dose“: „quick|episode|film|season|konzept|sample|play“,
  „doseLabel“: „1 charakteristische Folge reicht“,
  „teachUse“: „kurz: wofür im Unterricht“,
  „block“: „Web|OOP|DB|GUI|KI|Biostat|Bioinf|Allgemein|Rapport“,
  „platform“: „Netflix|Spotify|YouTube|…“,
  „url“: „https://… oder null“,
  „packItems“: ["Titel A","Titel B"],
  „subChecks“: [
    {"id":"...", „task“:"Überlebe die erste Nacht", „how“:"Bau vor Sonnenuntergang ein Shelter, sonst spawnen Monster.", „points“:5}
  ]
}
```

**Link-Regel:** `url` gesetzt → „Open ↗“. `null` → nur `platform`-Label + optional Google-Such-Button (`https://www.google.com/search?q=<title>`). Niemals rateigene Streaming-Deep-Links erfinden. `packItems` nur bei `medium:"pack"`. `subChecks` nur bei Games.

---

## 7 · DER CONTENT-KATALOG (komplett, direkt einbauen)

> Vollständig aus der Zielgruppen-Recherche 2026 (JIM-Studie / Jugend-Internet-Monitor). S=unbedingt, A=stark, B=nice, C=skippable/Nostalgie, Packs=gebündelte Longtails. „teachUse“ = MGIN-Andockpunkt.

siehe content.json

---

## 8 · Slang-Mikrocopy (Brainrot Mode ON)

- **Item abgehakt:** „Ate ✅ +50 Aura, no cap“ · „W. Du cookst.“ · „Lowkey essential.“
- **Game „fully experienced“ (alle Sub-Checks):** „Pro-Gamer-Stempel 🎮 du hast das ge-mastert.“
- **Sub-Check abgehakt:** „+5 Aura. Step closer to certified.“
- **Item ge-skippt / mid:** „Marked as mid. 8h gespart, real one.“
- **Pack erledigt:** „Pack geclapped 📦 +15.“
- **Quest fertig:** „Quest cleared 🔥 du hast hart gefarmt.“
- **Level-Up:** „GLOW UP! 🗿 Du bist jetzt {Titel}.“
- **Streak:** „Daily Grind: {n} 🔥 — keep it, real one.“
- **Leerer Zustand:** „0 Aura?? Bro… geh nicht Gras anfassen, geh Wednesday schauen.“
- **Tier-Badges:** S = „ESSENTIAL (no cap)“ · A = „big W“ · B = „solid“ · C = „mid / for the lore“.
- **Sort-Optionen:** „Speedrun (Zeit)“ · „Grind (Aura)“ · „Tier“ · „Vibe (random)“.
- **Lehrerzimmer Mode:** dieselben Texte normal („Erledigt ✓ +50 Punkte“).

---

## 9 · Akzeptanzkriterien (Definition of „W“)

1. Einzelnes `index.html`, offline, speichert Häkchen + Sub-Check-Häkchen + Aura/Level/Streak/Settings in `localStorage`, übersteht Reload.
2. Installierbar (Manifest + SVG-Icon), schön auf dem Handy.
3. Filter nach Medium, Sortierung Zeit/Aura/Tier, Suche, Schnellfilter (essential / ≤15 Min / offen / nur Games).
4. Aura/Level/Quests/Badges/Streak funktionieren; Slang-Toggle wechselt alle Texte.
5. **Games aufklappbar mit Sub-Checks** (task + how-Anleitung), Fortschritt „x/n erlebt“, Pro-Gamer-Stempel bei voll.
6. **Packs** zeigen ihre `packItems` im Detail-Sheet.
7. Externe Links neuer Tab; gesperrte Items → Plattform-Label statt totem Link.
8. Keine schweren Bild-Assets (Emoji + Gradients + 1 SVG-Icon).
9. Der KOMPLETTE Katalog aus §7 ist eingebaut.

---

## 10 · Stretch (optional)

- „Random Quest“-Button („gib mir ein 15-Min-Item für jetzt“).
- Export/Import des Fortschritts als JSON-String (copy/paste, weil keine DB).
- „Unterrichts-Filter“: nur Items eines MGIN-Blocks (Web/OOP/DB/GUI/KI/Biostat/Bioinf).
- Konfetti bei Level-Up & bei „Pro-Gamer“.
