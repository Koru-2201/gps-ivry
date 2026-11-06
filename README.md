# Applications de Navigation GPS Ivry-sur-Seine (Waze-like)

Trois applications web autonomes et prêtes pour **GitHub Pages** (statiques, sans build, HTML/CSS/JS pur), optimisées pour **Chrome et Brave sur Samsung Galaxy A15**.

---

## 📁 Structure du projet

```
gps-apps/
├── feux.geojson               # 129 feux tricolores extraits d'OpenStreetMap (Ivry-sur-Seine)
├── leaflet/
│   ├── index.html             # Leaflet + plugin leaflet-rotate + tuiles CARTO (dark_all / light_all)
│   └── feux.geojson
├── maplibre-maptiler/
│   ├── index.html             # MapLibre GL JS + style MapTiler (pitch ~55°, constante MAPTILER_KEY)
│   └── feux.geojson
└── maplibre-openfreemap/
    ├── index.html             # MapLibre GL JS + style OpenFreeMap personnalisé style Waze (sans clé)
    └── feux.geojson
```

---

## 🔑 1. Renseigner la clé MapTiler (pour `/maplibre-maptiler`)

1. Crée un compte gratuit sur [maptiler.com](https://cloud.maptiler.com/) (offre gratuite généreuse).
2. Ouvre le fichier [`maplibre-maptiler/index.html`](file:///C:/Users/steph/.gemini/antigravity/scratch/gps-apps/maplibre-maptiler/index.html).
3. À la ligne 219, remplace :
   ```js
   const MAPTILER_KEY = "YOUR_MAPTILER_API_KEY";
   ```
   par ta clé personnelle, par exemple :
   ```js
   const MAPTILER_KEY = "abc123xyz456";
   ```
*(Note : Si tu laisses la clé par défaut, l'application bascule automatiquement sur un style vectoriel de secours OpenFreeMap).*

---

## 🚀 2. Déploiement sur GitHub Pages

1. Crée un nouveau dépôt GitHub (public ou privé).
2. Pousse le contenu de `gps-apps/` sur la branche `main` :
   ```bash
   git init
   git add .
   git commit -m "feat: 3 webapps gps ivry-sur-seine"
   git branch -M main
   git remote add origin https://github.com/<ton-pseudo>/<ton-depot>.git
   git push -u origin main
   ```
3. Sur GitHub, rends-toi dans **Settings** → **Pages**.
4. Sous **Build and deployment** :
   - Source : **Deploy from a branch**
   - Branch : **main** / folder: `/(root)`
   - Clique sur **Save**.
5. Tes trois applications seront disponibles aux adresses suivantes (après 1 à 2 minutes) :
   - `https://<ton-pseudo>.github.io/<ton-depot>/leaflet/`
   - `https://<ton-pseudo>.github.io/<ton-depot>/maplibre-maptiler/`
   - `https://<ton-pseudo>.github.io/<ton-depot>/maplibre-openfreemap/`

---

## 📱 3. Test sur Android (Chrome / Brave sur Samsung A15)

1. Ouvre l'URL sur **Chrome** ou **Brave**.
2. **Autorisation GPS** : Accepte la demande d'accès à la position haute précision.
3. **Spécificité Brave Browser** :
   - Si la position n'apparaît pas ou reste bloquée, clique sur l'**icône de lion Brave** dans la barre d'adresse.
   - Désactive les **Brave Shields** pour le site ou passe la protection d'empreinte numérique en standard/autorisé.
4. **Wake Lock** : Dès que tu touches l'écran une fois, l'écran reste allumé en permanence pendant la conduite (se réactive si tu réduis puis rouvres le navigateur).

---

## 🚦 4. Feux Rouges & Méthode Overpass Turbo de secours

Les 129 feux tricolores d'Ivry-sur-Seine ont déjà été téléchargés dans `feux.geojson`. 

Si tu souhaites les rafraîchir ou en cas de problème, voici la requête Overpass :
1. Va sur [overpass-turbo.eu](https://overpass-turbo.eu/).
2. Colle cette requête dans l'éditeur :
   ```overpass
   [out:json][timeout:30];
   area["name"="Ivry-sur-Seine"]["admin_level"="8"]->.a;
   (
     node["highway"="traffic_signals"](area.a);
   );
   out body;
   >;
   out skel qt;
   ```
3. Clique sur **Exécuter**, puis sur **Exporter** → **Données** → **Télécharger en GeoJSON**.
4. Renomme le fichier obtenu en `feux.geojson` et remplace celui des répertoires.
