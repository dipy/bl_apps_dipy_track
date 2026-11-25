[![Abcdspec-compliant](https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg)](https://github.com/brain-life/abcd-spec)
[![Run on Brainlife.io](https://img.shields.io/badge/Brainlife-bl.app.66-blue.svg)](https://doi.org/10.25663/bl.app.66)
[![Run on Brainlife.io](https://img.shields.io/badge/Brainlife-bl.app.330-blue.svg)](https://doi.org/10.25663/brainlife.app.330)
[![Run on Brainlife.io](https://img.shields.io/badge/Brainlife-bl.app.331-blue.svg)](https://doi.org/10.25663/brainlife.app.331)
[![Run on Brainlife.io](https://img.shields.io/badge/Brainlife-bl.app.448-blue.svg)](https://doi.org/10.25663/brainlife.app.448)

# Dipy Tracking

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Tracks white matter streamlines with DIPY 1.1.1 dipy_track_local using the closest-peaks algorithm on DIPY peaks-and-metrics, a stopping image (FA) and a seed mask, and outputs a .trk tractogram.**

Wraps the DIPY 1.1.1 command-line workflow dipy_track_local with --tracking_method closestpeaks. Use it after fitting a DIPY reconstruction model that produced a peaks-and-metrics (PAM) file. It needs the PAM file, a tensor dataset whose FA image is used as the stopping criterion (thresholded by stopping_thr), and a binary seed mask; the result is written as output/track.trk. Sibling Apps in the same repository run the other dipy_track_local algorithms.

| | |
|---|---|
| 📥 **Inputs** | `neuro/peaks_and_metrics` · `neuro/tensor` · `neuro/mask` |
| 📤 **Outputs** | `neuro/track/trk` |
| 🐳 **Container** | `brainlife/dipy:1.1.1` |
| ⚙️ **Resources** | 8 CPUs |
| 🔗 **Run it** | [doi.org/10.25663/brainlife.app.331](https://doi.org/10.25663/brainlife.app.331) |

#### 📥 Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `pam_files` | `neuro/peaks_and_metrics` | yes | DIPY peaks-and-metrics (PAM) file with the fiber orientation peaks used for tracking. |
| `stopping_files` | `neuro/tensor` | yes | Tensor dataset; only its FA image is passed to dipy_track_local as the stopping image (md, rd and ad are mapped but not used). Must be in the same space and grid as the PAM file and the seed mask. |
| `seeding_files` | `neuro/mask` | yes | Binary mask; streamlines are seeded inside it with seed_density seeds per voxel dimension. |

#### 📤 Outputs

| Output | Datatype | Description |
|---|---|---|
| `output` | `neuro/track/trk` | Whole-brain tractogram in TrackVis .trk format (track.trk). |

<details>
<summary><b>⚙️ Parameters</b> (7)</summary>

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

📚 **References used by this App:**
- Garyfallidis E, et al. Dipy, a library for the analysis of diffusion MRI data. Front Neuroinform 8:8 (2014). [doi:10.3389/fninf.2014.00008](https://doi.org/10.3389/fninf.2014.00008)
- Avesani P, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. Sci Data 6:69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

<!-- brainlife-app:end -->

The App offers 4 Fiber Trackings algorithms: Deterministic Tracking, Probabilistic Tracking, Eudx Tracking, Closest Peaks Tracking.

### Authors
- [Serge Koudoro](skoudoro@iu.edu)
- [Javier Ricardo Guaje Guerra](jrguajeg@iu.edu)

### Contributors
- [Soichi Hayashi](soichih@gmail.com)

### Funding Acknowledgement
brainlife.io is publicly funded and for the sustainability of the project it is helpful to Acknowledge the use of the platform. We kindly ask that you acknowledge the funding below in your code and publications. Copy and past the following lines into your repository when using this code.

[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-1636893](https://img.shields.io/badge/NSF_BCS-1636893-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-ACI-1916518](https://img.shields.io/badge/NSF_ACI-1916518-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1916518)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://grantome.com/grant/NIH/R01-EB029272-01)

### Citations
We ask that you the following articles when publishing papers that used data, code or other resources created by the brainlife.io community.

1. Garyfallidis, E., Brett, M., Amirbekian, B., Rokem, A., van der Walt, S., Descoteaux, M., Nimmo-Smith, I., & Dipy Contributors (2014). Dipy, a library for the analysis of diffusion MRI data. Frontiers in neuroinformatics, 8, 8. [https://doi.org/10.3389/fninf.2014.00008](https://doi.org/10.3389/fninf.2014.00008)

2. Avesani, P., McPherson, B., Hayashi, S. et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. Sci Data 6, 69 (2019). [https://doi.org/10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

## Running the App

#### 1. On Brainlife.io

You can see a list of [Dipy Apps currently regsitered on Brainlife](https://brainlife.io/apps#dipy). Find the App that you'd like to run and click "Execute" tab to specify dataset that you'd like to run the App on.

#### 2. On  your machine (Running Locally)

To run this command, you can simply type:

`singularity exec -e docker://brainlife/dipy:1.1.1 dipy_track_local [your_args]`

To see the documentation of all arguments, [go to the following page](https://dipy.org/documentation/1.1.1./reference_cmd/dipy_track_local/)

## Input

To see the documentation of all the arguments, follow this [link](https://dipy.org/documentation/1.1.1./reference_cmd/dipy_track_local/).

## Output

All output files will be generated according to the passed arguments, as explained [here](https://dipy.org/documentation/1.1.1./reference_cmd/dipy_track_local/).

### Dependencies

This app runs on [singularity](https://www.sylabs.io/singularity/).

### DIPY
- This is a Brainlife wrapper App stemming from the [`dipy_track_local`](https://dipy.org/documentation/latest/reference_cmd/dipy_track_local/) workflow.
- This single wrapper is exposed through an apps registered on [Brainlife.io](https://brainlife.io).
- More information about DIPY : [https://dipy.org/](https://dipy.org/).
- More information about the command line `dipy_track_local`: [Command line Reference](https://dipy.org/documentation/latest/reference_cmd/dipy_track_local/).
