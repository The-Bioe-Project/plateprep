# Changelog

All notable changes to PlatePrep are documented here. Versions follow the build tag shown in the
application header.

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
