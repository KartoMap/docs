# Technical Documentation: Fundamentals of Cartography (Belgium & Europe)

This document serves as a reference for the development of Geographic Information System (GIS) software. It covers standards, data formats and sources specific to the Belgian-European context.

---

## 1. Coordinate Reference Systems (CRS / EPSG)

Choosing the coordinate system is the first critical step in ensuring that layers are superimposed correctly.

### Essential standards
| Scale | EPSG Code | Name | Main use |
| :--- | :--- | :--- | :--- |
| **Global (Web)** | `EPSG:3857` | WGS 84 / Pseudo-Mercator | Standard for base maps (Google, OSM, Mapbox). |
| **GPS / Satellite** | `EPSG:4326` | WGS 84 (Lat/Lon) | Raw storage of GPS coordinates. |
| **European** | `EPSG:4258` | ETRS89 | INSPIRE directive standard for interoperability in Europe. |
| **Belgium** | `EPSG:31370` | Belgian Lambert 2008 | **Mandatory** for administrative and cadastral accuracy in Belgium. |

> **Technical note:** For smooth software operation, data is often stored in `31370` for precision calculations (area, distance) and reprojected to `3857` for web display.

---

## 2. Data Models: Vector vs Raster

Developing a mapping engine requires managing two distinct types of data.



[Image of Vector vs Raster data comparison]


### A. Vector Data (Points, Lines, Polygons)
* **Usage:** Streets, cadastral parcels, points of interest.
* **Advantage:** Infinite precision when zooming, allows attributes to be attached (e.g. street name).

### B. Raster Data (Pixel Grids)
* **Use:** Aerial photographs (orthophotomaps), digital terrain models (DTMs), heat maps.
* **Advantage:** Ideal for representing continuous phenomena.

---

## 3. File Formats and Storage

### Local file formats
* **GeoJSON (`.geojson`):** The simplest for web development (JSON text format). Very verbose for large files.
* **GeoPackage (`.gpkg`):** The recommended modern standard. It is an all-in-one SQLite database. Replaces the old Shapefile format.
* **Vector Tiles (`.pbf` / `.mvt`):** Compressed format for displaying millions of points smoothly on the web.

### Database storage
* **PostgreSQL + PostGIS:** The absolute industry standard. Allows you to perform spatial SQL queries such as:
```sql
  SELECT name FROM restaurants WHERE ST_DWithin(geom, user_location, 500);
