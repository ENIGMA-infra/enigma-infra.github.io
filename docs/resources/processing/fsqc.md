# Welcome to the ENIGMA-infra FreeSurfer Quality Control (fsqc) guidelines!

## Quality Assessment part 1: Running the FS-QC toolbox
Congratulations, you made it to the quality assessment! For this purpose, we will use FreeSurfer Quality Control (FS-QC) toolbox. The [FS-QC toolbox](https://github.com/Deep-MI/fsqc) takes existing FreeSurfer (or FastSurfer) output and computes a set of quality control metrics. These will be reported in a summary table and/or .html page with screenshots to allow for visual inspection of the segmentations.

### Pulling the Docker image

You need to download the container image that will run the freesurfer quality control pipeline. Use the following command to pull the image from Docker Hub:

```bash
apptainer build fsqc_2.1.4.sif docker://deepmi/fsqcdocker:2.1.4
```

Make sure the resulting image file is placed in the container directory referenced in your global Nipoppy configuration.

### Getting the Nipoppy pipeline specification files for this pipeline
To get the Nipoppy specification files for the FS-QC container, run:
```bash
nipoppy pipeline install --dataset <dataset_root> 17100133
```

Read more about this step [here](../../resources/how_to_guides/getting_ENIGMA-PD_pipeline_config_files.md).

### Change your global config file
Open the global config file and add the correct freesurfer version (7.3.2) under the fsqc pipeline.

#### Final check

Before trying to run the pipeline, confirm that the pipeline is recognized by running `nipoppy pipeline list`. This will print a list of the available pipelines.

### Running the pipeline

To run the subsegmentation pipeline, use the following command:

```bash
nipoppy process --pipeline fsqc --pipeline-version 2.1.4 --pipeline-step process --dataset <dataset_root>
```

### Expected FS-QC output

After running the command, you will find all results inside the derivatives folder. The output will include:

- **Several folders**, most of them corresponding to the flags used in the command, such as: `screenshots`, `surfaces`, `skullstrip`, `hypothalamus`, `hippocampus`, and also, `status`, `metrics`

- **A CSV file** (`fsqc-results.csv`) summarizing quantitative quality control metrics for all subjects.

- **A main HTML summary file** (`fsqc-results.html`) that aggregates all subject screenshots for easy visual inspection.

You can verify that images were created for all subjects by running

```bash
nipoppy track-processing --pipeline fsqc --pipeline-step process-tracking --dataset <dataset_root>
```
and checking `processing_status.tsv` under the `derivatives` folder.

#### Important notes for viewing and copying files:

##### Locally
- We **strongly recommend downloading the entire output folder locally** before opening the HTML file. Opening the HTML on a server or network drive is often slow and may cause images not to load properly.

- When copying files, **make sure to include all generated folders** (such as `screenshots`, `surfaces`, etc.) along with the `fsqc-results.html` file. These folders contain the images referenced in the HTML and are essential for proper display.

##### On the server
- If you have not experienced such issues before and prefer to work directly on your server, you can instead open the HTML file in your available browser (for example: `firefox fsqc-results.html`).

##### Final check
- When opening the `fsqc-results.html` file:  
  - Scroll through the subjects to confirm all images load and no data is missing.  
- Click on any image to zoom in, or right-click and choose “Open in new tab” and inspect details more closely.