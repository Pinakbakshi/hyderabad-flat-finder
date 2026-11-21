# Hyderabad Flat Finder

A static, responsive Hyderabad property discovery and comparison application published through GitHub Pages.

## Features

- Buy and Rent modes
- Search by society, builder, locality and property type
- Locality, property type, BHK, budget, built-up area, bedroom-area, possession and amenity filters
- Square-feet / gaj conversion (1 gaj = 9 sq ft)
- Price and area sorting
- Leaflet map with listing pins, day/night basemaps and fit-to-results
- Browser-persisted saved listings
- Saved-list view and research log
- Shareable search URL (mode, locality, property type, BHK and budget)
- CSV export of current filtered results
- Bulk paste society names into research log
- Responsive layout and dark theme

## Run locally

Run `python3 -m http.server 8000` in the repository root and open http://localhost:8000. No build step is required.

## GitHub Pages deployment

1. Open repository Settings → Pages.
2. Set the build source to GitHub Actions.
3. The workflow at .github/workflows/pages.yml publishes the repository root on every push to main.
4. Follow the deployment in the Actions tab.

Expected site URL: https://pinakbakshi.github.io/hyderabad-flat-finder/

## Architecture and logic

The first release is static-first. index.html contains the UI, sample dataset, filtering engine, map integration and browser-side persistence. Leaflet and map tiles are loaded from public CDNs; map features require an internet connection.

### Property record

Each record includes: stable ID, name, locality, type, BHK, built-up square feet, price, possession, amenities, approximate latitude/longitude, bedroomSqft, transaction mode, builder label and verification flag. Buy prices are represented in lakh INR. Rent prices are represented in thousand INR per month.

### Filter pipeline

Filters are conjunctive (AND) across dimensions. Search text matches name, locality, builder or type. Locality and property type use exact matching. BHK selection is OR within the BHK dimension; the 6+ option means six or more bedrooms. Price and built-up area use inclusive minimum and maximum bounds. When gaj is selected, area bounds are multiplied by nine before comparing against square-foot records. Bedroom area only passes when a known measurement meets the threshold; unknown values are excluded. Every selected amenity must exist on the property. Possession is exact-match when selected. Saved view intersects the filtered records with locally saved IDs. Sorting is applied after filtering.

### State and persistence

Saved IDs are stored in browser localStorage under hff-saved. Research events are stored under hff-log. These are browser-local and do not sync across devices. Search sharing encodes a subset of current filters in the URL query string. CSV export serializes the currently filtered rows and quotes values to preserve commas and quotes.

### Map

Leaflet markers are created for the currently filtered records. Coordinates are approximate locality-level positions, not surveyed building coordinates. Marker popups display the listing and locality. Fit results frames the current result set. Day and night map layers use OpenStreetMap and CARTO public tile services.

## Data quality warning

The bundled property records are illustrative sample data for demonstrating the interface and filter behavior. They are NOT verified live listings, actual builder inventory, current market quotes, confirmed possession information or available properties. Do not rely on the names, prices, amenities or possession states for a property decision.

Bedroom-area measurements are intentionally null in the sample dataset. The minimum bedroom-area filter therefore excludes sample records until a verified bedroom measurement is populated. This avoids fabricating floor-plan dimensions.

Before making a purchase or rental decision, independently verify listing existence, seller authority, price, carpet and built-up area, title and encumbrances, approvals, RERA registration where applicable, possession, maintenance and current availability.

## Roadmap

- Ingest source-attributed verified property listings with source URL and last-checked timestamp.
- Add carpet area, floor, facing, parking, maintenance and RERA fields.
- Add proper locality polygons and distance filters.
- Add verified bedroom dimensions from floor plans.
- Add optional backend accounts and cross-device saved lists.
- Add automated browser tests and accessibility checks.

## Technology

HTML, CSS, vanilla JavaScript, Leaflet 1.9.4, OpenStreetMap/CARTO tiles, GitHub Actions and GitHub Pages.

## License

MIT. See LICENSE.
