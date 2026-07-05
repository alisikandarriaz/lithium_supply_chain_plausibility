# Lithium Supply Chain Plausibility Assessment

A geospatial research pipeline for assessing the transparency and reliability of the global lithium supply chain using real open data, spatial analysis, knowledge graphs, and remote sensing.

Department of Geoinformatics — Z_GIS, Paris Lodron University of Salzburg

## What this project does

This case study traces 5 verified lithium supply chain paths from mine to battery manufacturer and runs four plausibility checks on each path using real, publicly available data.

## Research question

When a company declares a lithium supply chain path, can we verify it is geologically consistent, geographically feasible, structurally complete, and physically active on the ground, using geospatial data, remote sensing, and knowledge graphs?

## The four plausibility rules

Each path is scored 0 to 1 on each rule. Overall score is the average of all four. PASS at 0.8 or above, WARN at 0.5 or above, FAIL below 0.5.

| Rule | What it checks | How |
|---|---|---|
| 1. Geological consistency | Does the geology at the declared mine location actually support lithium extraction? | Cross-checked against USGS MRDS records and the Geoscience Australia Critical Minerals dataset |
| 2. Geographic feasibility | Is the declared trade route between nodes physically and logistically realistic? | Haversine distance between consecutive supply chain nodes, compared against maximum feasible distances per route type |
| 3. Network structure completeness | Is every tier present, mining, processing, and manufacturing? | Counts tiers present in the master supply chain table for each path |
| 4. Remote sensing confirmation | Does satellite imagery confirm the mine is actually active on the ground? | NDVI (Normalized Difference Vegetation Index) calculated from Sentinel-2 imagery over a 5km buffer at each mine |

## Supply chain paths studied

| Path | Mine | Country | Processing | Country | Manufacturer | Country |
|---|---|---|---|---|---|---|
| 1 | Greenbushes | Australia | Kwinana Plant | Australia | CATL | China |
| 2 | Salar de Atacama | Chile | La Negra Refinery | Chile | LG Chem | South Korea |
| 3 | Wodgina | Australia | Kemerton Plant | Australia | Panasonic | Japan |
| 4 | Qinghai Lake Brine | China | Ganfeng Refinery | China | BYD | China |
| 5 | Pilgangoora | Australia | Chinese Refineries | China | Samsung SDI | South Korea |

All paths and node relationships are manually curated from each company's public annual report or ESG report, cited in the source_document field of the master table.

## Data sources

All data is open access and free to download.

| # | Dataset | Source | Format |
|---|---|---|---|
| 1 | USGS MRDS geological records | USGS | CSV |
| 2 | Mineral Commodity Summaries 2026 | USGS | PDF |
| 3 | Australian Critical Minerals Map 2025 | Geoscience Australia | XLSX / PDF |
| 4 | IGO Annual Report 2024 | IGO Limited | PDF |
| 5 | SQM Annual Report 2024 | SQM | PDF |
| 6 | Albemarle Annual Report 2025 | Albemarle | PDF |
| 7 | Pilbara Minerals Annual Report 2024 | Pilbara Minerals | PDF |
| 8 | CATL ESG Report 2025 | CATL | PDF |
| 9 | Ganfeng Annual Report 2024 | Ganfeng Lithium | PDF |
| 10 | LG Chem Sustainability Report 2024 | LG Chem | PDF |
| 11 | Samsung SDI Sustainability Report 2025 | Samsung SDI | PDF |
| 12 | UN Comtrade lithium trade flows (HS Code 282520) | United Nations | CSV |
| 13 | Sentinel-2 satellite imagery | Copernicus, via Google Earth Engine | API |
| 14 | OpenStreetMap Nominatim geocoding | OpenStreetMap | JSON API |
| 15 | Open Supply Hub facility data | Open Supply Hub | CSV |

### Note on Open Supply Hub

A search of Open Supply Hub across mining and material production sectors returned no results for Tianqi Lithium, CATL, Ganfeng, or any other lithium supply chain company. This absence is itself a finding of this study, upstream lithium supply chains remain missing from global facility transparency platforms that cover other sectors, like apparel, extensively.

## Technology stack

| Tool | Purpose |
|---|---|
| Python 3 | Primary programming language |
| Google Colab | Development and execution environment |
| pandas | Data loading and cleaning |
| geopandas | Spatial data handling |
| folium | Interactive mapping |
| plotly | Interactive charts, maps, and knowledge graph visualization |
| networkx | Knowledge graph construction |
| matplotlib | Trade flow charts |
| Google Earth Engine API (earthengine-api) | Sentinel-2 satellite imagery access and NDVI calculation |
| gdown | Automated data download from Google Drive |
| requests | OpenStreetMap Nominatim geocoding |

## Notebook structure

The entire pipeline runs in a single Google Colab notebook, divided into 9 sections.

| Section | What it does |
|---|---|
| 1 | Setup and library installation |
| 2 | Data download and Google Drive connection |
| 3 | Data loading (MRDS, Geoscience Australia, UN Comtrade), coordinate extraction, geocoding, and master supply chain table construction |
| 4 | Trade flow charts, export volume and price trends for Australia, Chile, and China |
| 5 | Interactive supply chain map of all 15 nodes |
| 6 | Knowledge graph construction, facility, company, and country nodes with material flow, ownership, location, and trade edges |
| 7 | Plausibility assessment, Rules 1 to 3 (geology, distance, structure) |
| 8 | Remote sensing, Sentinel-2 NDVI analysis via Google Earth Engine, Rule 4 |
| 9 | Final summary combining all four rules into an overall plausibility report |

## How to run this project

**Step 1** — Create a free Google Earth Engine account at earthengine.google.com and note your Cloud Project ID, needed for Section 8.

**Step 2** — Open the notebook in Google Colab using the link below.

**Step 3** — Run Section 1 to install dependencies.

**Step 4** — Run Section 2. This downloads the required geological and trade flow data files automatically and mounts your own Google Drive to save outputs.

**Step 5** — Run Sections 3 through 9 in order. Section 8 will prompt you to sign in to Google Earth Engine, use the same Google account as Section 2.

All charts, maps, and the knowledge graph are saved to a folder in your own Google Drive called `lithium-plausibility-outputs`.

## Key findings

- All 5 declared supply chain paths pass the plausibility assessment.
- China dominates midstream processing, exporting 120.7 million kg of lithium hydroxide in 2024, 53 times more than Australia.
- Lithium prices collapsed 76 percent between 2022 and 2024, from $38.47/kg to $9.31/kg (Chile), explaining financial stress across several paths.
- Greenbushes NDVI (0.51) is uncertain due to surrounding dense forest, a smaller buffer would give a more precise result. The mine is confirmed active from independent annual report data.
- Open Supply Hub has no records for any company in this lithium supply chain, a clear transparency gap.

## Limitations

- Full details on scope limitations, coordinate uncertainties, and where automation still needs human judgment are documented in the accompanying presentation and notebook comments.
- This study demonstrates the methodology on 5 major paths from key producing countries. Full global coverage would require assessing hundreds of paths.

## References

1. IEA (2025). *World Energy Outlook 2025*. International Energy Agency.
2. UNCTAD (2026). *Critical Minerals, Critical Decisions*. United Nations.
3. USGS (2026). *Mineral Commodity Summaries 2026*. U.S. Geological Survey.
4. Geoscience Australia (2026). *Australian Critical Minerals Map 2025*.
5. Cerai, A.P. (2024). Geography of Control: A Deep Dive Assessment on Criticality and Lithium Supply Chain. *Mineral Economics*.
6. IGO Limited (2024). *Annual Report 2024*.
7. SQM (2024). *Annual Report 2024*.
8. Albemarle Corporation (2025). *Annual Report 2025*.
9. Pilbara Minerals (2024). *Annual Report 2024*.
10. CATL (2025). *ESG Report 2025*.
11. Ganfeng Lithium (2024). *Annual Report 2024*.
12. LG Chem (2024). *Sustainability Report 2024*.
13. Samsung SDI (2025). *Sustainability Report 2025*.
14. UN Comtrade (2024). HS Code 282520, Lithium oxide and hydroxide.
15. Open Supply Hub (2024). Global Supply Chain Data Platform.
16. OpenStreetMap contributors (2024). Nominatim Geocoding API.
17. Copernicus / ESA (2024). Sentinel-2 Level-2A via Google Earth Engine.
