# Monitor Destacadas y Empresariales — Valora Analitik

Recoge cada 30 minutos los titulares de
[Noticias destacadas](https://www.valoraanalitik.com/noticias-economicas-importantes/) y
[Empresariales](https://www.valoraanalitik.com/noticias-empresariales/)
de valoraanalitik.com, y los publica como una portada estática.

```
.github/workflows/actualizar-noticias.yml   cron + commit automático
scraper/scrape.py                           extracción y fusión con el histórico
data/noticias.json                          archivo que consume la página
index.html                                  portada (GitHub Pages)
requirements.txt
```

## Montaje

1. Repositorio nuevo llamado `monitor_noticias_valoraanalitik`, rama `main`.
   Cree las carpetas con **Add file → Create new file** escribiendo la ruta
   completa (`.github/workflows/actualizar-noticias.yml`, `scraper/scrape.py`,
   `data/noticias.json`), no arrastrando archivos.
2. **Settings → Actions → General → Workflow permissions**: *Read and write permissions*.
3. **Settings → Pages**: *Deploy from a branch*, rama `main`, carpeta `/ (root)`.
4. **Actions → Actualizar Valora Analitik → Run workflow**.

Si le pone otro nombre al repositorio, cambie la URL de `FUENTE_JSON` en
`index.html`.

## Detalles

- El sitio corre WordPress con Elementor. Cada nota es un
  `article.elementor-post` cuyas clases incluyen `category-<slug>`; el filtro
  usa esa clase, así que los bloques laterales de otras secciones no se cuelan.
- La etiqueta que aparece bajo cada titular sale de `.elementor-post__badge`
  cuando existe; si no, de la primera categoría del artículo distinta a la de
  la sección. `MAPA_ETIQUETAS` en `scrape.py` traduce los slugs frecuentes
  (`noticias-dian` → DIAN, `dolar-hoy` → Dólar).
- Las notas que están en las dos secciones aparecen en ambas columnas, que es
  el comportamiento del propio sitio.
- Conserva 60 titulares por sección y descarta lo anterior a 30 días.

## Prueba local

```bash
pip install -r requirements.txt
python scraper/scrape.py
python -m http.server 8000
```
