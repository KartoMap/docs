# Documentation Technique : Fondamentaux de la Cartographie (Belgique & Europe)

Ce document sert de base de référence pour le développement d'un logiciel d'Information Géographique (SIG). Il couvre les standards, les formats de données et les sources spécifiques au contexte belgo-européen.

---

## 1. Systèmes de Coordonnées de Référence (CRS / EPSG)

Le choix du système de coordonnées est la première étape critique pour assurer la superposition correcte des couches.

### Les standards incontournables
| Échelle | Code EPSG | Nom | Usage principal |
| :--- | :--- | :--- | :--- |
| **Mondial (Web)** | `EPSG:3857` | WGS 84 / Pseudo-Mercator | Standard pour les fonds de carte (Google, OSM, Mapbox). |
| **GPS / Satellite** | `EPSG:4326` | WGS 84 (Lat/Lon) | Stockage brut des coordonnées GPS. |
| **Européen** | `EPSG:4258` | ETRS89 | Standard de la directive INSPIRE pour l'interopérabilité en Europe. |
| **Belgique** | `EPSG:31370` | Belge Lambert 2008 | **Obligatoire** pour la précision administrative et cadastrale en Belgique. |

> **Note technique :** Pour un logiciel fluide, on stocke souvent en `31370` pour les calculs de précision (surface, distance) et on reprojette en `3857` pour l'affichage web.

---

## 2. Modèles de Données : Vecteur vs Raster

Le développement d'un moteur cartographique nécessite la gestion de deux types de données distincts.



[Image of Vector vs Raster data comparison]


### A. Données Vectorielles (Points, Lignes, Polygones)
* **Usage :** Rues, parcelles cadastrales, points d'intérêt.
* **Avantage :** Précision infinie au zoom, permet d'attacher des attributs (ex: nom de la rue).

### B. Données Raster (Grilles de pixels)
* **Usage :** Photos aériennes (orthophotoplans), modèles numériques de terrain (MNT), cartes de chaleur.
* **Avantage :** Idéal pour représenter des phénomènes continus.

---

## 3. Formats de Fichiers et Stockage

### Formats de fichiers locaux
* **GeoJSON (`.geojson`) :** Le plus simple pour le développement Web (format texte JSON). Très verbeux pour les gros fichiers.
* **GeoPackage (`.gpkg`) :** Le standard moderne recommandé. C'est une base de données SQLite tout-en-un. Remplace avantageusement le vieux format Shapefile.
* **Vector Tiles (`.pbf` / `.mvt`) :** Format compressé pour l'affichage de millions de points de manière fluide sur le web.

### Stockage en base de données
* **PostgreSQL + PostGIS :** Le standard absolu de l'industrie. Permet d'effectuer des requêtes spatiales SQL du type :
  ```sql
  SELECT name FROM restaurants WHERE ST_DWithin(geom, user_location, 500);
