# 🎧 DJVAULT v2.0 — Music Manager for DJs

> Single-file web app. Drop your audio files, get automatic BPM / key / energy / genre detection, harmonic mixing suggestions, and AI-generated setlists. No backend, no installation.

---

## What's New in v2.0

Merged with the **DJ FLOW** analysis engine — the app now does the work for you :

| Feature | What it does |
|---|---|
| Auto BPM detection | Energy-based onset detection + autocorrelation on the audio waveform |
| Auto energy | RMS computation, visualized as a colored bar in the track list |
| Auto brightness | Spectral approximation via zero-crossing rate |
| Auto genre | Inferred from BPM + brightness + energy (Techno, House, DnB, Industrial...) |
| Camelot key suggestion | Pre-fills a key for harmonic-mixing workflows |
| Harmonic match suggestions | Click ⚡ on any track → top 8 most compatible tracks |
| Auto-Setlist generator | Greedy nearest-neighbor algorithm builds a flow-aware setlist with transition scores |

---

## Core Features (from v1)

- Drag & drop import — MP3, FLAC, WAV, AAC, OGG, M4A, OPUS
- ID3 tag reading — title, artist, BPM, genre, album, year auto-filled
- Filter by BPM range, Camelot key, genre, custom tags — stackable
- Custom tags — `peak time`, `warm up`, `dark`, `closing`, anything you want
- Playlists — create, multi-select, batch-add
- Sort by title / artist / BPM / energy / date
- Global search — title, artist, label, tags
- Export CSV (full library or filtered view)
- Export M3U (compatible Rekordbox, VirtualDJ, Serato, Traktor)
- 100% localStorage — your data stays in your browser

---

## Installation — Single File

Just open `index.html` in any browser. Pas de fichier CSS, pas de fichier JS, pas de build, aucune dépendance autre que `jsmediatags` chargé depuis un CDN.

### Utilisation locale
```bash
# Méthode 1 — Double-clic sur index.html
# Méthode 2 — Pour une compatibilité maximale avec l'import de fichiers :
python3 -m http.server 8080
# Puis ouvrir http://localhost:8080
```

### Mise en ligne via GitHub Pages
1. Créer un nouveau repo GitHub (public)
2. Uploader `index.html` et `README.md`
3. Settings → Pages → Source : `main` branch → Save
4. L'app est live à `https://TONUSERNAME.github.io/NOMDUREPO`

---

## How to Use

### Importing
- Drag tes fichiers audio sur la drop zone (sidebar)
- Ou clic sur `+ IMPORTER` dans la toolbar
- Chaque track est auto-analysée : BPM, énergie, brillance, genre, clé
- Progression affichée en temps réel

### Edit a Track
- Clic sur l'icône ✎
- Override n'importe quelle valeur auto-détectée
- Ajouter des tags custom (Entrée pour valider chacun)

### Get Match Suggestions
- Clic sur l'icône ⚡ d'une track
- Top 8 des tracks les plus compatibles, classées par score 0–100
- Pondération : BPM (40%) + clé (30%) + énergie (20%) + bonus genre (10%)
- Ajout direct à une playlist depuis la modale

### Generate an Auto-Setlist
- Clic sur `⚡ AUTO-SETLIST` dans la toolbar
- L'algo part de la track au BPM le plus bas et construit une séquence en prenant à chaque étape la track la plus compatible
- Chaque transition est notée : `Parfaite` (≥80), `Bonne` (≥60), `Difficile` (<60)
- Sauvegarde du résultat en playlist en un clic

### Filter & Search
- Tous les filtres se cumulent : BPM range + multi-genre + multi-clé + multi-tag
- La recherche match titre, artiste, label, tags

---

## Compatibility Score — How It Works

```
score = 0.4 × bpm_score    (diff BPM : 0-2 = 1.0, 3-6 = 0.85, 7-12 = 0.6, 13-20 = 0.35, >20 = 0.1)
      + 0.3 × key_score    (distance Camelot : 0 = 1.0, 1 = 0.9, 2 = 0.6, 3+ = 0.3)
      + 0.2 × energy_score (1 - diff_énergie_normalisée)
      + 0.15 si même genre
```

Score élevé = match BPM serré + clés Camelot adjacentes + énergie similaire.

---

## Camelot Wheel

A = mineur, B = majeur. Les clés adjacentes (±1) se mixent harmoniquement.

```
         12B                       12A
    11B       1B               11A       1A
  10B           2B           10A           2A
 9B               3B         9A               3A
  8B           4B             8A           4A
    7B       5B                 7A       5A
         6B                          6A
```

---

## Data Schema (localStorage)

- `djvault_tracks` → tableau d'objets track
- `djvault_playlists` → tableau d'objets playlist

```json
{
  "id": "abc123",
  "filename": "track.mp3",
  "title": "After Hours",
  "artist": "Fred Again..",
  "bpm": 128,
  "key": "5A",
  "energy": 72,
  "brightness": 58,
  "duration": 245.3,
  "genre": "Tech House",
  "tags": ["peak time", "dark"],
  "label": "Atlantic",
  "year": 2023,
  "notes": "Mix at 1:30",
  "added": 1718000000000
}
```

> ⚠️ Les fichiers audio ne sont PAS stockés (limites localStorage). Seules les métadonnées sont persistées. Le bouton "Ré-analyser" ne fonctionne que dans la session courante du navigateur — recharger les fichiers via "+ IMPORTER" pour les ré-analyser après fermeture.

---

## Roadmap

- [ ] Détection de clé réelle Krumhansl-Schmuckler
- [ ] Preview waveform Web Audio au hover
- [ ] Drag-to-reorder dans les playlists
- [ ] Export Rekordbox XML
- [ ] Backup/restore via fichier JSON
- [ ] IndexedDB pour bibliothèques >5 MB
- [ ] Layout mobile

---

## License

MIT — forke, modifie, ship.

---

*v2.0 — DJVAULT (organisation) ⊕ DJ FLOW (analyse). Un seul fichier, zéro friction.*
