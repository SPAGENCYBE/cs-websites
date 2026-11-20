# Export vanuit de CS Group Web Hub

Deze repo krijgt websites uit de hub-artifact `776da453-db46-4a7a-9088-08a8f87e0dd6` (CS Group · Web Hub).

## Werkwijze bij "zet de hub op GitHub"

1. `db_query` op collectie `designs` met `github.status == "aangevraagd"`.
2. Een website bestaat uit meerdere ontwerp-documenten met hetzelfde `setId` (zonder `setId`: het eigen `id`). Per combinatie `lang` + `page` telt alleen de nieuwste (`createdAt`). Per document:
   - HTML: `read_asset` met `htmlAssetId` (of veld `html`).
   - Pad in de site: veld `path` (bv. `index.html`, `diensten.html`, `fr/index.html`). Ontbreekt het: `index.html`.
   - Klant: `db_get clients/<clientId>` voor `logoId` en `photoIds`.
   - Afbeeldingen (één keer per klant): `read_asset` per id; bewaar als `<folder>/assets/logo.<ext>` en `<folder>/assets/foto-<n>.jpg`.
   - Vervang in de HTML `LOGO_URL` door `assets/logo.<ext>` en `PHOTO_<n>_URL` door `assets/foto-<n>.jpg` (hoogste n eerst). Voor pagina's in een taalmap (`fr/...`, `en/...`) wordt dat `../assets/...`.
   - Schrijf `<folder>/<path>`. Bewaar de vorige homepage bij een nieuwe versie als `<folder>/versies/<YYYY-MM-DD>-<style>.html`.
3. Werk de klantenlijst in `README.md` bij (tussen de KLANTEN-markeringen).
4. Commit naar `main` en push.
5. `db_write` merge op het ontwerp: `github.status = "gepubliceerd"`, `publishedAt` (ms), `repoUrl` = `https://github.com/SPAGENCYBE/cs-websites/tree/main/<folder>`, en `pagesUrl` alleen als de repo openbaar is en Pages aan staat. Bij een fout: `status = "fout"` en `error`.

Commits met als auteur carlos.van.nieuwenborgh@gmail.com.
