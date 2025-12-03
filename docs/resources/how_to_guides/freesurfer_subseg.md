# Welcome to the ENIGMA-infra FreeSurfer subsegmentations guidelines!

## Prerequisites

This guide assumes that you have:

- [installed Nipoppy and organized your data in BIDS](./setting_up_nipoppy.md)
- [Apptainer available as container platform](../open_science_tools/container_platforms.md)
- [performed FreeSurfer 7 processing](./freesurfer7.md)

## About the pipeline
This pipeline uses existing FreeSurfer 7 functionalities to extract subnuclei volumes from subcortical regions like the *thalamus*, *hippocampus*, *brainstem*, *hypothalamus*, *amygdala*, and *hippocampus*. It requires completed FreeSurfer output (`recon-all`) and integrates the subsegmentation outputs directly into the existing `/mri` and `/stats` directories. Additionally, the pipeline will perform [Sequence Adaptive Multimodal SEGmentation (SAMSEG)](https://surfer.nmr.mgh.harvard.edu/fswiki/Samseg) on T1w images in order to calculate a superior total intracranial volume (TIV).

## Pull container
You need to download the container image that will run the subsegmentations. Use the following Apptainer command to pull the image from Docker Hub:
```bash
apptainer build freesurfer_subseg_1.0.sif docker://nichyconsortium/freesurfer_subseg:1.0
```
Make sure the resulting image file is stored in the container directory [referenced in your global config file](../open_science_tools/container_platforms.md#storing-container-images).

## Set up configuration
To get the Nipoppy specification files for the subsegmentation container, run:
```bash
nipoppy pipeline install --dataset <dataset_root> 15877956
```
Read more about this step [here](./getting_ENIGMA-PD_pipeline_config_files.md).

**Note:** If you have multiple T1w images per subject per session, the container will throw an error. In this case, you will need to open the invocation file under `<dataset_root>/pipelines/processing/freesurfer_subseg-1.0/` and specify the name of the desired T1w image for SAMSEG in the following way:
```
"t1_image_path": "[[NIPOPPY_BIDS_PARTICIPANT_ID]]_[[NIPOPPY_BIDS_SESSION_ID]]_<edit-me>_T1w.nii.gz"
```

### Change global config file
Open the global config file and add the path to your freesurfer license file under the freesurfer_subseg pipeline, just like you did for the fMRIPrep pipeline:

```json
"PIPELINE_VARIABLES": {
  "BIDSIFICATION": {},
  "PROCESSING": {
    "fmriprep": {
      "24.1.1": {
        "FREESURFER_LICENSE_FILE": "path/to/license/file/license.txt",
        "TEMPLATEFLOW_HOME": "path/to/templateflow/"
      }
    },
    "freesurfer_subseg": {
      "1.0": {
        "FREESURFER_LICENSE_FILE": "path/to/license/file/license.txt"
      }
    }
  }
}
```

### Final check
Before trying to run the pipeline, confirm that the pipeline is recognized by running `nipoppy pipeline list`. This will print a list of the available pipelines.

## Run pipeline
To run the subsegmentation pipeline, use the following command:
```bash
nipoppy process --pipeline freesurfer_subseg --pipeline-version 1.0 --dataset <dataset_root>
```
**Note:** In case you want to submit a batch job to process all participants/sessions, you can find more info [here](./freesurfer7.md#run-pipeline).

### Track pipeline output
Use the `nipoppy track-processing` command to check which participants/sessions have complete output:
```bash
nipoppy track-processing --pipeline freesurfer_subseg --dataset <dataset_root>
```
This helps you confirm whether the pipeline ran successfully across your dataset (again, check `processing_status.tsv` under the `derivatives` folder).

## Extract pipeline output
For automatic extraction of the subsegmentation metrics and SAMSEG TIV into one .tsv file, you can use the `fs_subseg_stats` pipeline. The Zenodo ID for this pipeline is 17800572, so you can install it with the following command:
```
nipoppy pipeline install --dataset <dataset_root> 17800572
```
When the pipeline is installed, you may need to change the permission of the downloaded python script. Go to `<dataset_root>/pipelines/extraction/fs_subseg_stats-0.2/` and run `chmod 777 fs_subseg_stats.py`. Then, you can simply run
```bash
nipoppy extract --pipeline fs_subseg_stats --dataset <dataset_root>
```
to get things going. You can find the extracted data under `<dataset_root>/derivatives/freesurfer_subseg/1.0/idp/fs_subseg_stats-0.2/`.

Now, it's time to continue with [quality control](./fsqc.md) or get ready for data sharing!
