# PlatePrep

**From field photographs to annotation-ready settlement-plate imagery — in the browser.**

*[Versão em português → README.pt.md](README.pt.md)*

PlatePrep prepares **settlement-plate** campaign photographs for annotation in
[CoralNet](https://coralnet.ucsd.edu) and other platforms, in a single guided browser workflow:
campaign metadata → per-plate triage → selection → perspective-corrected cropping at fixed
physical scale → named images + ready-to-import CSVs + a crop-geometry manifest.

- **Single file, no installation** — `PlatePrep.html` runs in any Chromium-based desktop browser.
- **Bilingual** interface (English / Portuguese), switchable at any time.
- **Photos never leave your computer** — all image processing is local. The only optional network
  call is the weather lookup (Open-Meteo).
- **Reproducible by design** — every crop's four source corners are recorded in a manifest, so any
  crop can be re-derived and operator error can be screened without re-inspecting images.

**Try it now:** <https://www.projetobioe.com/plateprep.html> (hosted instance, HTTPS)

## The 5-step workflow

| Step | What happens |
|---|---|
| **1 · Set up** | Experiment, site, photographer, depth, number of plates, plate size and the plate → treatment map. **Date, camera and GPS are read from the photos' EXIF**; without GPS (e.g. GoPro HERO12), type coordinates or pick a **saved site**. One click fetches the day's **weather from Open-Meteo** (air temperature and conditions from its historical service, based on the ERA5 reanalysis; water temperature from its marine service). Everything is editable. |
| **2 · Triage** *(optional)* | The campaign photos in sequence with EXIF time; click the **first photo of each plate** and the program groups and numbers the rest. Can be skipped. |
| **3 · Selection** | Thumbnail gallery; each photo shows its triage plate; check the ones to crop (already-cropped photos start unchecked). |
| **4 · Crop** | Click the plate's 4 corners → homography rectification at **fixed scale** (image side = plate side). The plate is pre-filled from triage; treatment, file name and sequence are automatic. `Enter` saves and advances. |
| **5 · Finish** | The output folder receives the crops (`Plate##_T?_EXP_YYYY_MM_DD_##.JPG`), **`metadata_coralnet.csv`** (import-ready), **`triage_mapping.csv`** and **`crop_manifest.csv/.json`** (the 4 corners of every crop). |

Full instructions, with screenshots, in the manuals: [`docs/Manual_PlatePrep_EN.pdf`](docs/Manual_PlatePrep_EN.pdf) · [`docs/Manual_PlatePrep_PT.pdf`](docs/Manual_PlatePrep_PT.pdf).

## Getting started

**Use the hosted instance** (recommended): open <https://www.projetobioe.com/plateprep.html> in
Google Chrome or Microsoft Edge, click *Choose folders and start*, and pick the input folder
(campaign photos, JPG/PNG) and an output folder.

**Or run it yourself:** download [`PlatePrep.html`](PlatePrep.html) and open it in Chrome. Direct
folder access uses the File System Access API, which needs a secure context (HTTPS or
`localhost`); opened from `file://` the browser may restrict folder access, in which case
PlatePrep falls back to packaging its results into a downloadable `.zip`. To self-host, serve the
single file statically over HTTPS — no build step, no backend.

### Requirements

- Chromium-based **desktop** browser (Chrome, Edge). Other browsers fall back to `.zip` output.
- Photos available on the local disk (in Google Drive, mark the folder *Available offline*).
- Internet only for the optional weather lookup (and for the `.zip` fallback, which loads JSZip
  from a CDN); all fields can be filled in manually.

## Outputs

| File | Contents |
|---|---|
| `Plate##_T<letter>_<EXP>_<YYYY>_<MM>_<DD>_<seq>.JPG` | Square, perspective-corrected, fixed-scale crops (`Placa`/`Plate` prefix configurable). Self-describing, machine-parseable names. |
| `metadata_coralnet.csv` | One record per image with the columns CoralNet imports: `Name, Date, Experiment, Site, Treatment, Exposure_Days, Plate, Height (cm), Latitude, Longitude, Depth, Camera, Photographer, Water quality, Strobes, Framing gear used, White balance card, Comments`. `Exposure_Days` is computed from the deployment date; `Comments` carries the day's weather. |
| `triage_mapping.csv` | Original photo → plate / treatment / sequence — a permanent record of the campaign photo log. |
| `crop_manifest.csv` / `.json` | For every crop: the four source-image corner coordinates, source dimensions and output size. Re-derive any crop; quantify between-operator variability from the manifests alone. |

The plate → treatment map of the Bioē coating experiment (30 plates, treatments A–F) is built in
as a template and fully editable — including the treatment legend — for any other experiment.

## Known limitations (v1.0.4)

- Assumes flat, approximately square plates with all four corners visible; corner marking is
  manual (automatic corner detection is on the roadmap).
- Photographs wider than 3600 px are decoded at 3600 px width for memory economy
  (`MAX_DECODE_W`); the manifest records the working-canvas dimensions. Full-resolution decoding
  is planned for v1.1.
- Resampling goes through a 26 × 26 piecewise-affine mesh, which leaves a faint periodic
  intensity structure at the mesh pitch — harmless for point count, but visible to texture-based
  analyses. Per-pixel exact-homography rendering is planned for v1.1.
- Weather data lag real time by a few days (ERA5); recent campaigns are filled in manually.

## Citing

If you use PlatePrep, please cite it — see [`CITATION.cff`](CITATION.cff) (GitHub's *Cite this
repository* button uses it). A methods paper describing and validating the pipeline is in
preparation; this section will be updated with the reference and the archived-version DOI.

## Contributing

Issues and pull requests are welcome. PlatePrep is a single self-contained HTML file; the
rectification (`solveH`, `warpTo`), EXIF, CSV and manifest logic are plain JavaScript with no
dependencies. Please describe your browser and a minimal photo set when reporting a bug.

## License and credits

**CC BY 4.0** — see [`LICENSE`](LICENSE).

**Bioē Project** — Institute of Biology, University of Campinas (Unicamp), Department of
Biochemistry and Molecular Biology, with the Centre for Marine Biology (CEBIMar/USP).
Funding: FAPESP — *Technological Innovation Programs / PROASA – Program for the South Atlantic
Ocean and Antarctic Sciences*, Research Project – Regular, Call for Proposals (2025), 1st Cycle,
grant **#2025/0787809**.

Weather data: [Open-Meteo](https://open-meteo.com) (historical service based on the ERA5
reanalysis; marine service).
