# Changelog

All notable changes to PlatePrep are documented here. Versions follow the build tag shown in the
application header.

## [1.1.2] — 2026-08-16
- Re-derive mode now **skips and flags** a crop when the source photograph's aspect ratio no longer
  matches the frame its manifest was recorded in (an EXIF-orientation drift a single scale factor
  cannot undo) instead of writing a distorted crop. Flagged rows appear in `rederive_log.csv` as
  `orientation-mismatch-skipped`.

## [1.1.1] — 2026-08-16
- **Re-derive crops from a manifest** (link on the welcome screen). Given a folder of source
  photographs, one or more manifests (`crop_manifest.csv/.json`, or the older
  `manifesto_recortes.*`) and an output folder, PlatePrep re-produces every crop from its recorded
  four corners with the current renderer — rescaling the corners exactly if the photograph is now
  read at a different resolution — and writes a new manifest in native coordinates,
  `rederive_log.csv` (source found / missing per crop) and `render_audit.csv` (mesh-line gradient
  ratio per crop). This is the executable form of the manifest's reproducibility promise, and it
  is how the dataset of the methods paper was regenerated with the release renderer.

## [1.1.0] — 2026-08-16
- **Native-resolution decoding.** Up to v1.0.5 photographs wider than 3600 px were decoded at
  3600 px (`MAX_DECODE_W`, a memory economy inherited from the prototype), so 27-MP GoPro frames
  (5568 × 4872) were rectified from a 0.65× working canvas — the "source-resolution discrepancy"
  reported in the methods paper (Section 3.3). Photographs are now decoded at full size (guard
  raised to 12 000 px); the manifest records the source file's own pixel dimensions (`file_w`,
  `file_h`) next to the working-canvas dimensions (`natW`, `natH`) so any downsampling is visible.
- **Per-pixel exact-homography rendering.** The 26 × 26 piecewise-affine mesh (canvas
  clip + drawImage) left faint periodic seams at the mesh pitch (paper, Section 3.4). Every output
  pixel is now mapped through the exact homography and sampled bilinearly (2 × 2 supersampled when
  the source is denser than the output grid), reading only the bounding box of the clicked
  quadrilateral. Regression fixture: v1.1.0 crops agree with the independent reimplementation to
  MAE ≤ 1.2/255, r ≥ 0.998, mesh-line gradient ratio 0.9–1.0× (no seam).
- Source-pixel cache so the live 300-px preview stays fluid while panning/zooming.

## [1.0.5] — 2026-08-16
- **Physical scale embedded in every crop.** The output density (px/cm = output size ÷ plate
  size, 200 px/cm by default) is written into the JPEG's JFIF header (APP0 density, dots/cm or
  dpi, whichever rounds exactly), so tools that honour it (Photoshop, GIMP, QGIS, Python/PIL,
  Bio-Formats…) open the crop already calibrated. Nothing is drawn on the image.
- **ImageJ/Fiji companion macro** `PlatePrep_ImageJ_scale.ijm` written to the output folder
  (ImageJ's built-in JPEG reader ignores JFIF density): one run sets the global scale
  (`Set Scale… distance=<px> known=<cm> unit=cm global`).
- `crop_manifest.csv/.json` gain `plate_cm` and `px_per_cm` per crop.
- Finish screen lists the density and the macro.

## [1.0.4] — 2026-08-15
- Weather source explicitly attributed to Open-Meteo (historical service, ERA5-based; marine
  service) in the interface, manuals and READMEs.
- Manuals (EN/PT) regenerated with current screenshots.

## [1.0.3] — 2026-08-14
- Clear notice when photographs carry no GPS in their EXIF (e.g. GoPro HERO12 Black).
- **Saved sites**: named coordinate presets (two project sites built in) stored in the browser;
  "Save this site" button; last position used is remembered and pre-filled when photos lack GPS.

## [1.0.2] — 2026-08-14
- Camera field became an explicit selector: *Automatic (each photo's EXIF)* / *Other — type the
  model*, with the detected model shown as a hint.
- Depth now has a unit selector (cm / m); the CSV records the value with its unit.

## [1.0.1] — 2026-08-14
- Experiment is validated before leaving step 1; the step bar is navigable backwards; Experiment
  is editable in the crop bar; `Enter` saves even with focus on the plate selector; EXIF reading
  progress shown for large campaigns.

## [1.0.0] — 2026-08-14
- First unified release: 5-step workflow (Set up → Triage → Selection → Crop → Finish), bilingual
  EN/PT interface, EXIF/GPS pre-fill, Open-Meteo weather lookup, editable plate → treatment map,
  optional triage feeding the crop step, fixed-scale homography cropping, `metadata_coralnet.csv`,
  `triage_mapping.csv` and `crop_manifest.csv/.json` outputs, `.zip` fallback for browsers without
  the File System Access API. Supersedes the two standalone components (Segmentador de Placas,
  Clicador de Placas) it was built from.

## Planned — 1.1
- Decode photographs at native resolution (remove the 3600 px working-canvas cap).
- Per-pixel exact-homography rendering (remove the 26 × 26 mesh seams).
- Numeric `Depth`; drop or document the four always-empty CSV columns.
- Automatic corner detection (later).
