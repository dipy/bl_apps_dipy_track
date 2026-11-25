<h1 align="center">bl apps dipy track</h1>

<p align="center">
  Tracks white matter streamlines with DIPY 1.1.1 dipy_track_local using the closest-peaks algorithm on DIPY peaks-and-metrics, a stopping image (FA) and a seed mask, and outputs a .trk tractogram.
</p>

<p align="center">
  <a href="https://github.com/brainlife/abcd-spec"><img src="https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg" alt="ABCD Spec v1.1"></a>
  <a href="https://doi.org/10.25663/brainlife.app.331"><img src="https://img.shields.io/badge/Brainlife-brainlife.app.331-blue.svg" alt="Run on brainlife.io"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-see_LICENSE-lightgrey.svg" alt="License"></a>
</p>

## At a glance

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Tracks white matter streamlines with DIPY 1.1.1 dipy_track_local using the closest-peaks algorithm on DIPY peaks-and-metrics, a stopping image (FA) and a seed mask, and outputs a .trk tractogram.**

Wraps the DIPY 1.1.1 command-line workflow dipy_track_local with --tracking_method closestpeaks. Use it after fitting a DIPY reconstruction model that produced a peaks-and-metrics (PAM) file. It needs the PAM file, a tensor dataset whose FA image is used as the stopping criterion (thresholded by stopping_thr), and a binary seed mask; the result is written as output/track.trk. Sibling Apps in the same repository run the other dipy_track_local algorithms.

| | |
|---|---|
| **Inputs** | `neuro/peaks_and_metrics` · `neuro/tensor` · `neuro/mask` |
| **Outputs** | `neuro/track/trk` |
| **Container** | `brainlife/dipy:1.1.1` |
| **Resources** | 8 CPUs |
| **Run it** | [doi.org/10.25663/brainlife.app.331](https://doi.org/10.25663/brainlife.app.331) |

#### Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `pam_files` | `neuro/peaks_and_metrics` | yes | DIPY peaks-and-metrics (PAM) file with the fiber orientation peaks used for tracking. |
| `stopping_files` | `neuro/tensor` | yes | Tensor dataset; only its FA image is passed to dipy_track_local as the stopping image (md, rd and ad are mapped but not used). Must be in the same space and grid as the PAM file and the seed mask. |
| `seeding_files` | `neuro/mask` | yes | Binary mask; streamlines are seeded inside it with seed_density seeds per voxel dimension. |

#### Outputs

| Output | Datatype | Description |
|---|---|---|
| `output` | `neuro/track/trk` | Whole-brain tractogram in TrackVis .trk format (track.trk). |

<details>
<summary><b>Parameters</b> (7)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `tracking_method` | string | `closestpeaks` | Tracking algorithm passed to dipy_track_local --tracking_method; fixed to 'closestpeaks' for this App. |
| `use_binary_mask` | boolean | `false` | If True, uses a binary stopping criterion. If the provided `stopping_files` are not binary, `stopping_thr` will be used to binarize the images. |
| `stopping_thr` | number | `0.25` | Threshold applied to stopping volume's data to identify where tracking has to stop (default 0.25). |
| `seed_density` | number | `1` | Number of seeds per dimension inside voxel (default 1). For example, seed_density of 2 means 8 regularly distributed points in the voxel. And seed density of 1 means 1 point at the center of the voxel. |
| `step_size` | number | `0.5` | Step size used for tracking (default 0.5mm) |
| `pmf_threshold` | number | `0.1` | Threshold for ODF functions |
| `max_angle` | number | `30` | Maximum angle between streamline segments (range [0, 90] |

</details>

<!-- brainlife-app:end -->

## Versions on brainlife.io

This repository hosts several Apps, each built from its own branch.

| App | Branch | Run it | Status |
|---|---|---|---|
| Tracking using Closest Peaks algorithm | [`1.1.1`](https://github.com/dipy/bl_apps_dipy_track/tree/1.1.1) | [doi:10.25663/brainlife.app.331](https://doi.org/10.25663/brainlife.app.331) | current |
| Tracking using EuDX algorithm | [`1.1.1`](https://github.com/dipy/bl_apps_dipy_track/tree/1.1.1) | [doi:10.25663/brainlife.app.330](https://doi.org/10.25663/brainlife.app.330) | available |
| Tracking using Probabilistic algorithm | [`1.1.1`](https://github.com/dipy/bl_apps_dipy_track/tree/1.1.1) | [doi:10.25663/brainlife.app.448](https://doi.org/10.25663/brainlife.app.448) | available |
| Tracking using Deterministic algorithm | [`1.1.1`](https://github.com/dipy/bl_apps_dipy_track/tree/1.1.1) | [doi:10.25663/bl.app.66](https://doi.org/10.25663/bl.app.66) | available |

## Running the App

### On brainlife.io

Open the App at [doi.org/10.25663/brainlife.app.331](https://doi.org/10.25663/brainlife.app.331), choose `neuro/peaks_and_metrics`, `neuro/tensor`, `neuro/mask` from your project, set the parameters and press **Execute**. The results are saved to your project as `neuro/track/trk`.

### Locally

You need [Singularity/Apptainer](https://apptainer.org/) (the App runs in `brainlife/dipy:1.1.1`), [`jq`](https://jqlang.github.io/jq/).

1. Clone this repository and check out the App's branch (`git checkout 1.1.1`).
2. Create `config.json` with the paths to your inputs and the parameters:

   ```json
   {
       "peaks": "/path/to/pam_files/peaks",
       "stopping_files": "/path/to/stopping_files/stopping_files",
       "md": "/path/to/stopping_files/md",
       "rd": "/path/to/stopping_files/rd",
       "ad": "/path/to/stopping_files/ad",
       "seeding_files": "/path/to/seeding_files/seeding_files",
       "tracking_method": "closestpeaks",
       "use_binary_mask": false,
       "stopping_thr": 0.25,
       "seed_density": 1,
       "step_size": 0.5,
       "pmf_threshold": 0.1,
       "max_angle": 30
   }
   ```

3. Run `./main`.

## DIPY

- This is a Brainlife wrapper App stemming from the [`dipy_track_local`](https://dipy.org/documentation/latest/reference_cmd/dipy_track_local/) workflow.
- This single wrapper is exposed through an apps registered on [Brainlife.io](https://brainlife.io).
- More information about DIPY : [https://dipy.org/](https://dipy.org/).
- More information about the command line `dipy_track_local`: [Command line Reference](https://dipy.org/documentation/latest/reference_cmd/dipy_track_local/).

## Citing

If you use this App, please cite it, the software it builds on and brainlife.io:

- Tracking using Closest Peaks algorithm. brainlife.io App. [doi:10.25663/brainlife.app.331](https://doi.org/10.25663/brainlife.app.331)
- Garyfallidis E, et al. Dipy, a library for the analysis of diffusion MRI data. Front Neuroinform 8:8 (2014). [doi:10.3389/fninf.2014.00008](https://doi.org/10.3389/fninf.2014.00008)
- Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. *Scientific Data* 6:69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

## Authors

- Serge Koudoro
- Javier Ricardo Guaje Guerra

**Contributors**

- Soichi Hayashi

## Funding

[![Wellcome-226486/Z/22/Z](https://img.shields.io/badge/Wellcome-226486%2FZ%2F22%2FZ-purple.svg)](https://wellcome.org/)
[![NIH-NINDS-U24NS140384](https://img.shields.io/badge/NIH_NINDS-U24NS140384-green.svg)](https://reporter.nih.gov/project-details/11261037)
[![NIH-NINDS-UM1NS132207](https://img.shields.io/badge/NIH_NINDS-UM1NS132207-green.svg)](https://reporter.nih.gov/project-details/11411328)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://reporter.nih.gov/project-details/10262925)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://reporter.nih.gov/project-details/10428625)
[![NIH-NIMH-R01MH126699](https://img.shields.io/badge/NIH_NIMH-R01MH126699-green.svg)](https://reporter.nih.gov/project-details/10460628)
[![NIH-NIDCD-R01DC020088](https://img.shields.io/badge/NIH_NIDCD-R01DC020088-green.svg)](https://reporter.nih.gov/project-details/11140284)
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-2203524](https://img.shields.io/badge/NSF_BCS-2203524-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2203524)
[![NSF-IIS-1636893](https://img.shields.io/badge/NSF_IIS-1636893-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-OAC-2148729](https://img.shields.io/badge/NSF_OAC-2148729-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148729)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NSF-IIS-2148700](https://img.shields.io/badge/NSF_IIS-2148700-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148700)
[![NSF-OAC-1916518](https://img.shields.io/badge/NSF_OAC-1916518-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1916518)

<sub>brainlife.io, which runs this App, is publicly funded by Wellcome, the U.S. National Institutes of Health and the U.S. National Science Foundation; acknowledging that support helps keep it running.</sub>

## License

See [LICENSE](LICENSE).
