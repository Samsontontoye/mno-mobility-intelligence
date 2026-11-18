# Geographic Data Source Record

## Kenya administrative boundaries (geoBoundaries)

### Dataset details

| Field | Value |
|---|---|
| Dataset | Kenya administrative boundaries |
| Country | Kenya (KEN) |
| Administrative level | ADM1 — first-level administrative units (counties) |
| Provider | geoBoundaries |
| Product | gbOpen |
| Format | GeoJSON |
| Download date | Recorded automatically in the companion metadata JSON |
| Boundary vintage | Read from `boundaryYearRepresented` |
| Boundary ID | Read from `boundaryID` |
| Original source | Read from `boundarySource` and `licenseSource` |
| Licence | Read from `boundaryLicense`; do not assume one licence applies to every gbOpen boundary |

### Source and licensing

Source portal: https://www.geoboundaries.org/

API endpoint:
`https://www.geoboundaries.org/api/current/gbOpen/KEN/ADM1/`

The gbOpen product contains openly licensed boundaries, but the original licence may vary by boundary. Retain and follow the specific boundary's `boundaryLicense`, `licenseDetail`, `licenseSource`, `boundarySource`, and `boundarySourceURL` metadata. Follow any attribution, share-alike, modification, or other conditions stated there.

### Attribution statement

Use the exact metadata-generated attribution printed by the download script. A project-level acknowledgement can read:

> Administrative boundary data: geoBoundaries, Kenya ADM1, gbOpen. Original source, licence, boundary vintage, and download metadata are recorded in `data/external/geoBoundaries_KEN_ADM1_metadata.json`. See https://www.geoboundaries.org/.

This does not replace any additional attribution required by the original licence or upstream source.

### Reproducibility

- Keep the GeoJSON and metadata JSON together.
- Record the boundary ID, year represented, build date, source update date, exact download URL, and UTC download timestamp.
- The `/current/` endpoint can change. Preserve the downloaded file and use that archived copy for repeat analyses rather than silently downloading a newer version.
- If you update the boundaries, retain the new version separately and document the change.
- Document geometry repair, simplification, reprojection, clipping, or other transformations.

### Use in this project

These boundaries support aggregation of inferred subscriber home-location estimates and network-observed presence by administrative area.

A county-level subscriber count is a count of subscribers observed or inferred in this MNO dataset. It is not, by itself, a census count or estimate of the county's total population. Cell-site coordinates and inferred home locations have spatial uncertainty.

### References

- geoBoundaries API documentation: https://www.geoboundaries.org/api.html
- geoBoundaries: https://www.geoboundaries.org/
- Runfola, D. et al. (2020). “geoBoundaries: A global database of political administrative boundaries.” *PLOS ONE*, 15(4), 1–9. https://doi.org/10.1371/journal.pone.0231866

---

## Python: download boundaries and record provenance

Run this in your notebook after `TARGET_AREAS_FILE` has been defined. It saves the boundary GeoJSON and a companion metadata file. Licence and source fields are read from the API response, not guessed.

```python
from datetime import datetime, timezone
import json
import requests

TARGET_AREAS_FILE.parent.mkdir(parents=True, exist_ok=True)

API_URL = "https://www.geoboundaries.org/api/current/gbOpen/KEN/ADM1/"

response = requests.get(API_URL, timeout=60)
response.raise_for_status()
metadata = response.json()

download_url = metadata.get("gjDownloadURL")
if not download_url:
    raise ValueError("geoBoundaries metadata did not provide gjDownloadURL.")

geo_response = requests.get(download_url, timeout=120)
geo_response.raise_for_status()
geojson = geo_response.json()

if geojson.get("type") != "FeatureCollection":
    raise ValueError("Downloaded file is not a GeoJSON FeatureCollection.")

features = geojson.get("features", [])
if not features:
    raise ValueError("Downloaded GeoJSON contains no features.")

with open(TARGET_AREAS_FILE, "w", encoding="utf-8") as f:
    json.dump(geojson, f)

downloaded_at = datetime.now(timezone.utc).isoformat()

metadata_record = {
    "dataset": "Kenya administrative boundaries",
    "provider": "geoBoundaries",
    "country": metadata.get("boundaryName", "Kenya"),
    "country_iso3": metadata.get("boundaryISO", "KEN"),
    "administrative_level": metadata.get("boundaryType", "ADM1"),
    "product": "gbOpen",
    "boundary_id": metadata.get("boundaryID"),
    "boundary_year_represented": metadata.get("boundaryYearRepresented"),
    "boundary_source": metadata.get("boundarySource"),
    "boundary_license": metadata.get("boundaryLicense"),
    "license_detail": metadata.get("licenseDetail"),
    "license_source": metadata.get("licenseSource"),
    "boundary_source_url": metadata.get("boundarySourceURL"),
    "source_data_update_date": metadata.get("sourceDataUpdateDate"),
    "build_date": metadata.get("buildDate"),
    "downloaded_at_utc": downloaded_at,
    "api_url": API_URL,
    "download_url": download_url,
    "feature_count": len(features),
    "geometry_file": TARGET_AREAS_FILE.name,
    "geometry_format": "GeoJSON",
    "attribution": (
        "Administrative boundary data: geoBoundaries, "
        f"{metadata.get('boundaryName', 'Kenya')} "
        f"{metadata.get('boundaryType', 'ADM1')}, gbOpen. "
        f"Boundary ID: {metadata.get('boundaryID')}. "
        f"Original source: {metadata.get('boundarySource')}. "
        f"Licence: {metadata.get('boundaryLicense')}. "
        "See https://www.geoboundaries.org/."
    )
}

metadata_path = TARGET_AREAS_FILE.with_name(
    "geoBoundaries_KEN_ADM1_metadata.json"
)

with open(metadata_path, "w", encoding="utf-8") as f:
    json.dump(metadata_record, f, indent=4, ensure_ascii=False)

print("Boundary file:", TARGET_AREAS_FILE)
print("Metadata file:", metadata_path)
print("Feature count:", len(features))
print("Boundary ID:", metadata_record["boundary_id"])
print("Boundary vintage:", metadata_record["boundary_year_represented"])
print("Original licence:", metadata_record["boundary_license"])
print("Upstream source:", metadata_record["boundary_source"])
print("Downloaded at UTC:", downloaded_at)
print("\nAttribution:")
print(metadata_record["attribution"])
```

## Suggested project structure

```text
mno_mobility_intelligence_residence_and_population_displacement_inference/
├── data/
│   └── external/
│       ├── target_areas.geojson
│       └── geoBoundaries_KEN_ADM1_metadata.json
└── docs/
    └── data_sources/
        └── geographic_data_sources.md
```

Save this document as `docs/data_sources/geographic_data_sources.md`.
