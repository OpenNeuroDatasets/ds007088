## Description of Dataset

Penn LEAD (Penn Longitudinal Executive functioning in Adolescent Development) is a data resource designed to investigate transdiagnostic executive function during development.

This repository contains data derivatives from running fMRIPrep.
We provide derivatives including volumetric data such as motion and distortion-corrected BOLD timeseries, confound regressors, masks, and transforms, as well as quality control metrics and figures. Additional fMRI derivatives after processing with XCP-D can be found [here](https://openneuro.org/datasets/ds006779).

The link to the accompanying raw dataset is here: https://openneuro.org/datasets/ds007116 [accession number ds007116].

The Docker Hub link for fMRIPrep is [here.](https://hub.docker.com/layers/nipreps/fmriprep/25.0.0/images/sha256-04555ac849ec9e08fa8c70ebf0e0eb6fc99fd54b589c4108b5309de7dca0194b)

The quality control recommendations for fMRI can be found on [Github](https://github.com/PennLINC/transdiagnostic_executive_function/tree/main/QC/qc_csvs/final_QC_csvs). Both QC recommendations for fMRI and T1 structural data should be taken into account for fMRI data.


## Instructions to generate complete fMRIPrep derivatives

In order to generate complete fMRIPrep derivatives for this dataset, you must clone the fMRIPrep (ds007088), fMRIPrep-anat (ds007089), and raw (ds007116) datasets.
These fMRIPrep derivatives were produced with BABS, which processes each session independently. In order to reproduce that processing structure with vanilla fMRIPrep, you will need to use fMRIPrep version 25.2.0 or later.
Here is an example fMRIPrep call to generate outputs for sub-138950 ses-1:

```
apptainer run \
    --cleanenv \
    -B /path/to/ds007116:/data \
    -B /path/to/ds007089:/fmriprep_anat \
    -B /path/to/ds007088:/fmriprep_minimal \
    -B /path/to/output:/out \
    -B /path/to/working-directory:/work \
    -B /path/to/license.txt/license.txt:/license.txt \
    fmriprep_v25.2.0.sif \
    /data \
    /out \
    participant \
    -w /work \
    --stop-on-first-crash \
    --fs-license-file /license.txt \
    --output-spaces func T1w MNI152NLin6Asym:res-2 \
    --force bbr \
    --skip-bids-validation \
    --cifti-output 91k \
    --fs-subjects-dir /fmriprep_anat/sourcedata/freesurfer \
    --fs-no-resume \
    --participant-label sub-138950 \
    --derivatives minimal=/fmriprep_minimal \
    --session-label 1
```
