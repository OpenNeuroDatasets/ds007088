## Description of Dataset

Penn LEAD (Penn Longitudinal Executive functioning in Adolescent Development) is a data resource designed to investigate transdiagnostic executive function during development.

This repository contains data derivatives from running fMRIPrep.
We provide derivatives including volumetric data such as motion and distortion-corrected BOLD timeseries, confound regressors, masks, and transforms, as well as quality control metrics and figures. Additional fMRI derivatives after processing with XCP-D can be found [here](https://openneuro.org/datasets/ds006779).

The link to the accompanying raw dataset is here: https://openneuro.org/datasets/ds006688 [accession number ds006688].

The Docker Hub link for fMRIPrep is [here.](https://hub.docker.com/layers/nipreps/fmriprep/25.0.0/images/sha256-04555ac849ec9e08fa8c70ebf0e0eb6fc99fd54b589c4108b5309de7dca0194b)

The quality control recommendations for fMRI can be found on [Github](https://github.com/PennLINC/transdiagnostic_executive_function/tree/main/QC/qc_csvs/final_QC_csvs). Both QC recommendations for fMRI and T1 structural data should be taken into account for fMRI data.

*Note that we manually deleted extra n-back task runs from the data upon consulting the scanner notes after running XCP-D. Hence, the html files for the following subjects that originally had an extra n-back run will incorrectly specify that there were two runs:
- sub-20149, ses-1
- sub-20212, ses-1
- sub-20238, ses-2
- sub-20352, ses-2
- sub-20934, ses-1
- sub-20964, ses-1
- sub-20968, ses-2
- sub-21035, ses-1
- sub-21197, ses-1
- sub-21516, ses-2
- sub-21553, ses-1
- sub-21738, ses-1
- sub-23676, ses-1

## Instructions to generate complete fMRIPrep derivatives
In order to generate complete fMRIPrep derivatives for this dataset, you must clone the fMRIPrep (ds006741), fMRIPrep-anat (ds006732), and raw (ds006688) datasets.
These fMRIPrep derivatives were produced with BABS, which processes each session independently. In order to reproduce that processing structure with vanilla fMRIPrep, you will need to use fMRIPrep version 25.2.0 or later.
Here is an example fMRIPrep call to generate outputs for sub-138950 ses-1:


` ` `
apptainer run \
    --cleanenv \
    -B /path/to/ds006688:/data \
    -B /path/to/ds006732:/fmriprep_anat \
    -B /path/to/ds006741:/fmriprep_minimal \
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
` ` `