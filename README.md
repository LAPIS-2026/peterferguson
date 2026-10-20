# XII LAPIS 2026 — Stellar Streams in Photometric Surveys

Lectures, practice, and student project for the XII La Plata International School (LAPIS) 2026,
*Local Volume Cosmology: constraints on dark matter in the era of Gaia, LSST and S4*,
La Plata, 19–30 October 2026.

**Peter Ferguson** · pferguso@uw.edu

*Códigos y datos para el práctico del Prof. Peter Ferguson en la escuela XII LAPIS 2026.*

## Lectures

| | Title | Slides |
|---|---|---|
| 1 | Discovery: finding streams in photometric surveys | [Lecture 1](https://docs.google.com/presentation/d/1v8M8euYTiYZrLkxg_90Nu5eOrT7rlGTflwK4_SHWAAY/edit?usp=drive_link) 
| 2 | Project introduction | [slides](https://docs.google.com/presentation/d/1MLxs_F3bKFGd5GNdTyNYLsvzYCQTEW1aZ0Ug6bJgcmI/edit?usp=drive_link)
| 3 | Modeling streams: data and simulations | slides |
| 4 | Streams as dark matter probes, and the decade ahead | slides|

## Practice (after Lecture 1)

Build an isochrone matched filter and recover the ATLAS–Aliqa Uma stream in DELVE DR3 data.


## Student project

[Project description (PDF)](project/lapis2026_streams_project.pdf)

- `project/download_delve_cutouts.ipynb` — download DELVE DR3 Gold cutouts around the AAU streams with `lsdb`.
- If downloading the data is difficult a Pre-made cutout can be found here: [link, 180 MB](https://drive.google.com/file/d/1wMQ4wCvaNaszgTu7Kd9wKl8sRmCeIo4L/view?usp=drive_link). Reach out if you need the full delve footprint (20Gb)

## Pre-reading

- [Shipp et al. (2018)](https://arxiv.org/abs/1801.03097) — finding streams in photometric data
- [Ferguson et al. (2022)](https://arxiv.org/abs/2104.11755) — characterizing streams
- [Drlica-Wagner et al. (2019), §4.2](https://arxiv.org/abs/1902.01055) — dark matter constraints with LSST: stellar stream gaps
- [Bonaca & Price-Whelan (2025)](https://arxiv.org/abs/2405.19410) — review of stellar streams

## Setup

### 1. Create the environment

```bash
conda env create -f environment.yaml
conda activate lapis_streams
python -m ipykernel install --user --name lapis_streams
```

This gives you Python 3.14 with the matched-filter stack (numpy, scipy, pandas, matplotlib, astropy,
healpy, skyproj), `lsdb` for DELVE cutouts, `gala` for orbits and particle-spray streams, and
`galstreams` for the tracks of known streams. It is everything you need for the practice, the core
project, and Option A.

Quick check:

```bash
python -c "import lsdb, gala, galstreams; print(lsdb.__version__, gala.__version__, galstreams.__version__)"
```

### 2. Option B only: ugali and streamobs

Stream injection uses [streamobs](https://github.com/LSSTDESC/streamobs), which samples stars from
isochrones with [ugali](https://github.com/DarkEnergySurvey/ugali). Neither is on PyPI in a usable
form (the PyPI `ugali` predates the current isochrone format), so install both from GitHub with the
environment active:

```bash
conda activate lapis_streams

# ugali, from GitHub (not PyPI)
pip install git+https://github.com/DarkEnergySurvey/ugali.git

# streamobs: clone, install in editable mode, download its data files (from Zenodo)
git clone https://github.com/LSSTDESC/streamobs.git
pip install -e ./streamobs
python streamobs/bin/download_data.py
```

Quick check:

```bash
python -c "import ugali.isochrone, streamobs; print('ok')"
```

`pip install -e` puts `streamobs` on your path permanently, so you do not need the `PYTHONPATH`
export from the streamobs README. If `download_data.py` fails, see
<https://lsstdesc.github.io/streamobs/data.html>.

### Notes

- `pandas` is pinned below 3 because `galstreams` 1.2.1 crashes when loading its stream library
  under pandas 3. Everything else works with either.
- `s3fs` is only needed if you use the MAST S3 mirror of DELVE instead of the default
  `https://data.lsdb.io` endpoint.

## Cómo clonar este repositorio

```bash
git clone https://github.com/LAPIS-2026/peterfergusson.git
```
