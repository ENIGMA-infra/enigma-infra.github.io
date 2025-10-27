# Welcome to the ENIGMA-infra guidelines for setting up Nipoppy!

## What is Nipoppy?
Nipoppy is a lightweight framework for standardized data organization and processing of neuroimaging-clinical datasets. Its goal is to help users adopt the [FAIR principles](https://www.go-fair.org/fair-principles/) and improve the reproducibility of studies. 

## Why?
We strongly recommend adoption of Nipoppy tools to simplify coordination and ensure reproducibility of this end-to-end process across all sites. 

The ongoing collaboration between the ENIGMA working groups and Nipoppy team has streamlined data curation, processing, and analysis workflows, which significantly simplifies tracking of data availability, addition of new pipelines and upgrading of existing pipelines. The ENIGMA and Nipoppy teams are available to support and guide users through the process of implementing the framework, ensuring a smooth transition. To join the Nipoppy support community, we recommend joining the [Discord channel](https://discord.gg/dQGYADCCMB). Here you can ask questions and find answers while working with Nipoppy. 

**In the context of ENIGMA working groups, we will primairly use Nipoppy to help you with BIDSification and to facilitate easy processing with pipelines (such as FreeSurfer processing).**

For more information, see the [Nipoppy documentation](https://nipoppy.readthedocs.io/en/stable/index.html).

## Nipoppy installation
The recommended steps and commands for the installation of Nipoppy are outlined below. For more information, we refer to the [Nipoppy documentation installation page](https://nipoppy.readthedocs.io/en/stable/overview/installation.html). 

### Step 1) Setting up a Python environment

Create a new environment with Python version of at least 3.9. Here we call it nipoppy_env, but it can be named anything. In a Terminal window, run:

```bash
conda create --name nipoppy_env python=3
```

Activate the environment, e.g. by running:

```bash
conda activate nipoppy_env
```

### Step 2) Installing the nipoppy package

Install Nipoppy by running:

```bash
pip install nipoppy
```

### Step 3) Check whether the installation was successful

If Nipoppy was installed successfully, The following command should print a usage message:

```bash
nipoppy -h
```

## Creating a Nipoppy dataset: Choose your starting point
Once Nipoppy is successfully installed, you will need to create a Nipoppy dataset and populate it with your data. There are a few different starting points depending on the current state of your dataset:
- If you have your data already in BIDS format, click [here](#starting-with-bidsified-data). 
- If you have raw MRI data (DICOM of NIFTI files) that are not yet in BIDS, continue directly below. 
- If you're not sure what BIDS is or if you're wondering why you should convert your data into BIDS at all, you can find more info [here](../open_science_tools/BIDS_info.md).

## Creating a Nipoppy dataset: Starting from source data (either DICOMs or NIfTIs that are *not yet* in BIDS)
This is the scenario assumed by the Nipoppy [Quickstart page](https://nipoppy.readthedocs.io/en/stable/overview/quickstart.html). The steps and commands are outlined below as well.

Follow this guide to:
1. Create an empty Nipoppy directory tree
2. Customize the manifest file
3. Copy data into the Nipoppy directory tree

### Step 1) Create an empty Nipoppy directory tree

Run *(change <dataset_root> to your dataset name)*:

```bash
nipoppy init --dataset <dataset_root>
```

The newly created structure of empty folders (directory tree) follows the Nipoppy specification. Other Nipoppy commands expect all these directories to exist – they will throw an error if that is not the case.

Check out the directory tree:

```bash
tree <dataset_root>
```

### Step 2) Customize the manifest file

The Nipoppy manifest file is a .tsv file that contains expected availability information about the participants, visits, sessions, and imaging datatypes available in a dataset. This file is **mandatory**.

The manifest file is automatically created at `<dataset_root>/manifest.tsv` by nipoppy init and contains some example participants. You have to modify the manifest file to describe your dataset. This is one of the few manual steps part of Nipoppy. We recommend downloading the manifest file or editing it in a text editor available on your cluster to add all participants and information about the visits, sessions, and data types. 

> Note: if your dataset is cross-sectional (i.e. only has one time point), you still need to create a `session_id` for the manifest. In this case the value would be the same for all participants.

You can read more about the manifest file and check out some examples in the [Nipoppy documentation](https://nipoppy.readthedocs.io/en/latest/explanations/manifest.html)

### Step 3) Copy data into the Nipoppy directory tree

It is time to [copy and reorganize](https://nipoppy.readthedocs.io/en/stable/how_to_guides/user_guide/organizing_imaging.html) your raw imaging data to prepare them for BIDS conversion. Once this is done, you can find how to perform the BIDSification within the Nipoppy framework [here](https://nipoppy.readthedocs.io/en/stable/how_to_guides/user_guide/bids_conversion.html). We recommend applying a containerized BIDS-conversion pipeline that can be run within Nipoppy. 

## Starting with BIDSified data

If your dataset is already in BIDS, then the manifest-generation step can be skipped by initializing the Nipoppy dataset with this command, specifying the path to your new dataset and the path to your existing BIDS data:

```bash
nipoppy init --dataset <dataset_root> --bids-source <path_to_existing_bids_data>
```

This command will create a Nipoppy dataset (i.e. directory tree) from preexisting BIDS dataset and automatically generate a manifest file for you! 

### BIDS datasets without sessions

If the existing BIDS data does not have session-level folders, Nipoppy will create "dummy sessions" (called `unnamed`) in the manifest. This is because the Nipoppy manifest still requires a non-empty `session_id` value when imaging data is available for a participant.

If it is feasible to redo the BIDSification to include session folders, we recommend doing so since this is considered good practice. Otherwise, Nipoppy can still be run, but you will need to make some manual changes. For more information, see [here](https://nipoppy.readthedocs.io/en/stable/how_to_guides/init/bids.html#bids-data-without-sessions)

### Starting from data already processed with FreeSurfer7


We still encourage you to use Nipoppy to organize your source and/or BIDS data with your processed FS7 output to make use of automated trackers and downstream subsegmentation processing. However, you may need to some help depending on your version of FreeSurfer and naming convention of `participant_id`. Reach out to us on our [Discord channel](https://discord.gg/dQGYADCCMB) and we would be happy to help! 
