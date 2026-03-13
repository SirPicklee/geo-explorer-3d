# 🌍 3D World Atlas — Plants, Volcanoes & Tsunamis

> An interactive single-page web application that visualizes the most important plants per country, the world's 150 most significant volcanoes, and major tsunami events — all rendered on a real-time 3D globe.

![Version](https://img.shields.io/badge/version-1.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-active-brightgreen)

---
## 🖼 Screenshots

<img width="1919" height="941" alt="Image" src="https://github.com/user-attachments/assets/c737bc6e-ea89-4371-95e4-ec814e1e184f" />

<img width="1906" height="949" alt="Image" src="https://github.com/user-attachments/assets/5eee02a6-a0a9-46e6-b846-2d7dcb344c43" />

<img width="1883" height="926" alt="Image" src="https://github.com/user-attachments/assets/86ad58b5-a1a3-40d5-ae0a-bc2cc04466c1" />

<img width="1896" height="851" alt="Image" src="https://github.com/user-attachments/assets/8ca1bccb-ab6f-400a-9ebf-b8eea3c3dcc8" />
## 📋 Table of Contents

- [Features](#-features)
- [Technologies](#-technologies)
- [Installation](#-installation)
- [Usage](#-usage)
- [How to Add Images](#-how-to-add-images)
- [Data Sources](#-data-sources)
- [Layers Overview](#-layers-overview)
- [Project Structure](#-project-structure)
- [Contributing](#-contributing)
- [License](#-license)

---

## ✨ Features

| Feature | Description |
|---|---|
| 🌐 **3D Globe** | Rotatable and zoomable Earth rendered with Three.js |
| 🌿 **Country Plants** | Top 5 most important plants per country |
| 🌋 **World Volcanoes** | 150 of the world's most significant active & historic volcanoes |
| 🌊 **Tsunami Events** | Major historical tsunami locations with impact data |
| 🖱️ **Interactive Layers** | Each data layer can be toggled on/off independently |
| 📍 **Detail Panel** | Click any marker to open a rich info card |
| 📱 **Responsive** | Works on desktop, tablet, and mobile |

---

## 🛠 Technologies

```
HTML5 / CSS3 / Vanilla JavaScript
Three.js        →  3D globe rendering & camera controls
GeoJSON         →  Country boundaries and location data
D3.js           →  Color scales and data interpolation (optional)
```

> **No build step required.** All libraries are loaded via CDN.

---

## 🚀 Installation

### Option 1 — Open Directly

```bash
# 1. Clone the repository
git clone https://github.com/your-username/3d-world-atlas.git

# 2. Navigate to the folder
cd 3d-world-atlas

# 3. Open in your browser
open index.html
```

### Option 2 — Local Server (Recommended)

Some browsers block local file requests due to CORS policy. Use a simple local server:

```bash
# Python 3
python -m http.server 8080

# Node.js (npx)
npx serve .

# VS Code
# Install "Live Server" extension → right-click index.html → "Open with Live Server"
```

Then visit `http://localhost:8080` in your browser.

---

## 🎮 Usage

### Controls

| Action | Desktop | Mobile |
|---|---|---|
| Rotate globe | Left click + drag | Single finger drag |
| Zoom in / out | Mouse wheel | Two-finger pinch |
| Click a marker | Left click | Tap |
| Close panel | `ESC` or ✕ button | ✕ button |
| Reset camera | Double click | Double tap |

### Layer Controls

Use the panel in the top-right corner to toggle layers:

- 🌿 **Plants** — Shows 5 plants per country (green markers)
- 🌋 **Volcanoes** — 150 major volcanoes (orange / red markers)
- 🌊 **Tsunamis** — Major tsunami source points (blue markers)

---

## 🖼 How to Add Images

Images can be added in **three different ways** depending on where you want them to appear.

---

### 📌 Method 1 — Earth Texture (Globe Surface)

Replace the default globe surface with a high-resolution Earth image.

**Step 1 — Prepare your image**

- Format: `.jpg` or `.png`
- Recommended size: `4096 × 2048 px` (equirectangular projection)
- Free textures: [NASA Visible Earth](https://visibleearth.nasa.gov) or [Solar System Scope](https://www.solarsystemscope.com/textures/)

**Step 2 — Place the file**

```
assets/
└── textures/
    └── earth_texture.jpg   ← put it here
```

**Step 3 — Reference it in `index.html`**

```javascript
// Find this section in index.html and update the path:
const textureLoader = new THREE.TextureLoader();
const earthTexture = textureLoader.load('assets/textures/earth_texture.jpg');

const earthMaterial = new THREE.MeshPhongMaterial({
  map: earthTexture
});
```

---

### 📌 Method 2 — Marker Icons (Volcanoes, Plants, Tsunamis)

Replace the default colored dots with custom icon images on the globe.

**Step 1 — Prepare your icons**

- Format: `.png` with transparent background
- Recommended size: `64 × 64 px` or `128 × 128 px`
- Naming convention:

```
assets/
└── icons/
    ├── icon_volcano.png    ← volcano marker
    ├── icon_plant.png      ← plant marker
    └── icon_tsunami.png    ← tsunami marker
```

**Step 2 — Load icons as sprites in `index.html`**

```javascript
// Create a sprite material from your icon image
const volcanoTexture = new THREE.TextureLoader().load('assets/icons/icon_volcano.png');
const spriteMaterial = new THREE.SpriteMaterial({ map: volcanoTexture });

// Create the sprite and position it on the globe surface
const sprite = new THREE.Sprite(spriteMaterial);
sprite.scale.set(0.1, 0.1, 0.1);  // Adjust size as needed
sprite.position.set(x, y, z);       // 3D coordinates on the globe
scene.add(sprite);
```

**Step 3 — Convert lat/lon to 3D coordinates**

```javascript
// Helper function — paste this into index.html
function latLonToVector3(lat, lon, radius) {
  const phi   = (90 - lat) * (Math.PI / 180);
  const theta = (lon + 180) * (Math.PI / 180);
  return new THREE.Vector3(
    -(radius * Math.sin(phi) * Math.cos(theta)),
      radius * Math.cos(phi),
      radius * Math.sin(phi) * Math.sin(theta)
  );
}

// Usage example
const pos = latLonToVector3(37.8, 28.4, 1.02); // lat, lon, slight offset from surface
sprite.position.copy(pos);
```

---

### 📌 Method 3 — Info Card Images (Detail Panel)

Show a photo inside the popup card when a user clicks a marker.

**Step 1 — Prepare your photos**

- Format: `.jpg` or `.webp`
- Recommended size: `400 × 250 px` (landscape)
- Naming convention:

```
assets/
└── photos/
    ├── etna.jpg
    ├── krakatau.jpg
    ├── tsunami_2004.jpg
    └── cherry_blossom.jpg
```

**Step 2 — Add the image path to your data object**

```javascript
// Inside your volcano / plant / tsunami data array in index.html:
const volcanoes = [
  {
    name: "Mount Etna",
    lat: 37.75,
    lon: 15.00,
    country: "Italy",
    lastEruption: 2023,
    image: "assets/photos/etna.jpg",   // ← add this field
    description: "The tallest active volcano in Europe."
  },
  // ...
];
```

**Step 3 — Render the image in the detail panel**

```javascript
// Inside your click handler / panel rendering function:
function showDetailPanel(data) {
  const panel = document.getElementById('detail-panel');

  panel.innerHTML = `
    <button onclick="closePanel()">✕</button>
    ${data.image
      ? `<img src="${data.image}" alt="${data.name}"
             style="width:100%; border-radius:8px; margin-bottom:12px;">`
      : ''
    }
    <h2>${data.name}</h2>
    <p>${data.description}</p>
  `;

  panel.style.display = 'block';
}
```

---

### 📌 Quick Image Path Reference

```
3d-world-atlas/
├── index.html
└── assets/
    ├── textures/
    │   └── earth_texture.jpg       → Globe surface
    ├── icons/
    │   ├── icon_volcano.png        → Volcano markers
    │   ├── icon_plant.png          → Plant markers
    │   └── icon_tsunami.png        → Tsunami markers
    └── photos/
        ├── etna.jpg                → Volcano detail cards
        ├── cherry_blossom.jpg      → Plant detail cards
        └── tsunami_2004.jpg        → Tsunami detail cards
```

> 💡 **Always use relative paths** (`assets/photos/etna.jpg`), never absolute paths (`C:/Users/...`). Absolute paths break when you deploy the project online.

> 💡 **Compress images before adding them.** Use [Squoosh](https://squoosh.app) or [TinyPNG](https://tinypng.com) to reduce file size without losing visible quality. Large textures over 5 MB will noticeably slow down the globe load time.

---

## 📊 Data Sources

### 🌿 Plant Data
- **Source:** [GBIF — Global Biodiversity Information Facility](https://www.gbif.org)
- **Coverage:** 195 countries × 5 plants ≈ 975 records
- **Criteria:** Endemicity, economic importance, cultural value, ecosystem role

### 🌋 Volcano Data
- **Source:** [Smithsonian Institution — Global Volcanism Program](https://volcano.si.edu)
- **Coverage:** 150 most significant volcanoes worldwide
- **Fields:** Name, coordinates, last eruption year, VEI index, country

### 🌊 Tsunami Data
- **Source:** [NOAA — National Centers for Environmental Information](https://www.ngdc.noaa.gov/hazard/tsu.shtml)
- **Coverage:** Events with M ≥ 7.0 and high damage rating
- **Fields:** Date, source magnitude, wave height, affected countries, casualty count

---

## 🗂 Layers Overview

### 🌿 Country Plants — Sample Data

| Country | Plants |
|---|---|
| 🇹🇷 Turkey | Tulip, Thyme, Hazelnut, Olive, Cedar |
| 🇧🇷 Brazil | Brazil Nut, Açaí, Rubber Tree, Yerba Maté, Silk Floss |
| 🇯🇵 Japan | Cherry Blossom, Bamboo, Japanese Maple, Ginkgo, Wisteria |
| 🇦🇺 Australia | Eucalyptus, Wattle (Acacia), Banksia, Grass Tree, Waratahs |
| 🇨🇳 China | Bamboo, Ginkgo, Lotus, Peony, Chrysanthemum |

### 🌋 Featured Volcanoes

| Volcano | Country | Last Eruption |
|---|---|---|
| Krakatau | Indonesia | 2020 |
| Etna | Italy | 2023 |
| Fuji | Japan | 1707 |
| Vesuvius | Italy | 1944 |
| Mauna Loa | USA (Hawaii) | 2022 |
| Popocatépetl | Mexico | 2023 |

### 🌊 Featured Tsunamis

| Event | Year | Max Wave | Casualties |
|---|---|---|---|
| Indian Ocean Earthquake | 2004 | ~30 m | ~227,000 |
| Tōhoku (Japan) | 2011 | ~40 m | ~20,000 |
| Alaska | 1964 | ~67 m | 139 |
| Lisbon | 1755 | ~20 m | ~60,000 |
| Papua New Guinea | 1998 | ~15 m | 2,200 |

---

## 📁 Project Structure

```
3d-world-atlas/
│
├── index.html              # Main application — all logic lives here
│
├── assets/                 # Static assets
│   ├── textures/           # Globe surface textures
│   ├── icons/              # Custom marker icons
│   └── photos/             # Detail card images
│
├── data/                   # (Optional) External JSON data files
│   ├── plants.json
│   ├── volcanoes.json
│   └── tsunamis.json
│
└── README.md               # This file
```

> The project is designed to run as a **single file** (`index.html`). All data and styles are embedded inline. The `assets/` and `data/` folders are optional extensions.

---

## 🤝 Contributing

1. Fork this repository
2. Create a new branch (`git checkout -b feature/new-feature`)
3. Commit your changes (`git commit -m 'Add new feature'`)
4. Push the branch (`git push origin feature/new-feature`)
5. Open a Pull Request

### Areas to Contribute

- 🌿 Completing missing country plant data
- 🌋 Adding new volcano or tsunami records
- 🎨 UI / UX improvements
- 🌐 Multi-language support (i18n)
- 📱 Mobile experience enhancements
- 🖼 Adding high-quality photos for detail cards

---

## 📜 License

This project is distributed under the [MIT License](LICENSE).  
Data sources are subject to their own licensing terms — GBIF (CC BY 4.0), Smithsonian GVP (CC BY 4.0), NOAA (Public Domain).

---

<div align="center">
  <sub>🌍 Explore the world — through its plants, its fire, and its waves.</sub>
</div>
