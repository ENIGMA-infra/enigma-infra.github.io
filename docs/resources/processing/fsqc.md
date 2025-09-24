# Welcome to the ENIGMA-infra FreeSurfer Quality Control (fsqc) guidelines!

## Prerequisites
This guide assumes that you have:
- [installed Nipoppy and organized your data in BIDS](../../resources/data_org/setting_up_nipoppy.md)
- [Apptainer available as container platform](../../resources/how_to_guides/container_platforms.md)
- [performed FreeSurfer 7 processing](../../resources/processing/freesurfer7.md)
- [performed FreeSurfer subsegmentation](../../resources/processing/freesurfer_subseg.md) (not a hard requirement)

## About the fsqc toolbox
Congratulations, you made it to the quality assessment! For this purpose, we will use the FreeSurfer Quality Control (fsqc) toolbox. The [fsqc toolbox](https://github.com/Deep-MI/fsqc) takes existing FreeSurfer (or FastSurfer) output and computes a set of quality control metrics. These will be reported in a summary table and/or .html page with screenshots to allow for visual inspection of the segmentations.

## Pull the container
You need to download the container image that will run the FreeSurfer Quality Control pipeline. Use the following command to pull the image from Docker Hub:
```bash
apptainer build fsqc_2.1.4.sif docker://deepmi/fsqcdocker:2.1.4
```
Make sure the resulting image file is stored in the container directory [referenced in your global config file](../../resources/how_to_guides/container_platforms.md#storing-container-images).

## Set up configuration
To get the Nipoppy specification files for the fsqc container, run:
```bash
nipoppy pipeline install --dataset <dataset_root> 17100133
```
Read more about this step [here](../../resources/how_to_guides/getting_ENIGMA-PD_pipeline_config_files.md).

### Change global config file
Open the global config file and add the correct freesurfer version (7.3.2) under the fsqc pipeline.

### Final check
Before trying to run the pipeline, confirm that the pipeline is recognized by running `nipoppy pipeline list`. This will print a list of the available pipelines.

## Run the pipeline
To run the fsqc pipeline, use the following command:
```bash
nipoppy process --pipeline fsqc --pipeline-version 2.1.4 --pipeline-step process --dataset <dataset_root>
```

## Expected output
After running the command, you will find all results inside the derivatives folder. The output will include:
- **Several folders**, most of them corresponding to the flags used in the command, such as: `screenshots`, `surfaces`, `skullstrip`, `hypothalamus`, `hippocampus`, and also, `status`, `metrics`
- **A CSV file** (`fsqc-results.csv`) summarizing quantitative quality control metrics for all subjects.
- **A main HTML summary file** (`fsqc-results.html`) that aggregates all subject screenshots for easy visual inspection.

You can verify that images were created for all subjects by running:
```bash
nipoppy track-processing --pipeline fsqc --pipeline-step process-tracking --dataset <dataset_root>
```
and checking `processing_status.tsv` under the `derivatives` folder.

When all required files are there, you are ready to move on to the [visual inspection](../../resources/visual_qa/qa_md)!
