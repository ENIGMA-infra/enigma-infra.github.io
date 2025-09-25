# ENIGMA-PD Visual Quality Control Instructions

Welcome to the **ENIGMA FreeSurfer Visual Quality Control Manual**  
*Last updated: August 2025*

This page outlines the recommended approach to visually inspect and rate FreeSurfer segmentations. The current instructions focus on cortical and subcortical structures only. Subsegmentation quality control will be added once guidelines are available.

## Step-by-step instructions for visual inspection

### 1. Download the template spreadsheets
- Download the [Cortical QC template](./ENIGMA-PD_Cortical_QC_Template.xlsx/)
- Download the [Subcortical QC template](./ENIGMA-PD_Subcortical_QC_Template.xlsx)

These templates should be filled in manually. The only changes to be made to these templates are 1) adding all subjects from your dataset in the first column (you can copy over file names from your nipoppy manifest file), and 2) changing quality control scores from `"pass"` to `"fail"` if needed. The cortical template contains two tabs: the first tab is used to record the visual inspection scores, while the second tab provides additional context on common quality control issues observed in ENIGMA projects.  You do **not** need to review or complete the second tab — it is included to stay consistent with the standard ENIGMA consortium template. The subcortical template consists of one tab, with the selected subcortical areas for visual inspection.

### 2. Learn about the scoring

  In the template spreadsheet, all regions are by default marked as a `"pass"`. Raters are asked to provide a score for each region: **either `"pass"` or `"fail"`**, no other values should be used.
  
  **Be conservative when failing**: Use the `"fail"` label only when there are **obvious, serious issues** that would make the regional estimates **unreliable or unusable**.  Small flaws or mild asymmetries are typically not enough to justify a fail. 

#### Types of scores
For the **cortical** quality control, you will complete several types of scores: an overall score for internal and external QC, and specific scores for each region, assessed separately for the left and right hemispheres. You may optionally add comments or a QC_code (as defined in the second tab of the spreadsheet), but these are not required for the ENIGMA-PD quality assessment.

For the **subcortical** quality control, you will provide specific scores for each region, again assessed separately for the left and right hemispheres. In addition, there is an overarching column `Overall_subcortical_QC`. This can be set to `fail` when all individual regions fail (please still put `fail` for each column). If `Overall_subcortical_QC` is set to fail, the subject will be excluded from the subcortical analysis.

### 3. Familiarize yourself with ENIGMA quality control standards

Before starting, review the ENIGMA quality control instructions, including common segmentation errors and regions that are more prone to segmentation failure. You can find the ENIGMA manuals, adapted for the PD group below:
- [ENIGMA-PD Cortical Quality Control Manual](./Cortical_QC_ENIGMA-PD_July25.pdf)
  - [Version of the cortical quiz slides including the answers to the quiz](./Cortical_QC_ENIGMA-PD_July25_quiz_answers.pdf)
- [ENIGMA-PD Subcortical Quality Control Manual](./Subcortical_QC_ENIGMA-PD_July25.pdf)

We recommend **not focusing too heavily on the manuals**. Though they do provide some useful examples, hands-on practice is key to learning quality control, and the quality of someone’s visual assessment improves primarily through experience.

### 4. Check out the fsqc-generated html page with screenshots

Open the `fsqc-results.html` in your browser. This page shows several different images for each subject of your dataset:
- Screenshots: For internal quality control of the cortical areas and to inspect the subcortical areas
- Skullstrip: To confirm whether skullstripping was successful. If skullstripping failed, regions that are not covered by the extracted brain mask (but should have been) and possibly those near the edges of the brain will fail quality control (for example, the cerebellum). 
- Surfaces: For external quality control of the cortical areas
- Hypothalamus: For quality control of the hypothalamus subsegmentations
- Hippocampus: For quality control of the hippocampus and amygdala subsegmentations

For a walkthrough of how these look in practice, check out [the FSQC tour](./fsqc_tour_ENIGMA-PD.pdf).

### 5. Cortical Quality Control

#### Warm-up: dynamically check a few subjects across all regions
Start by quickly scrolling through around 5–10 subjects to get a sense of the typical variability in cortical segmentations. This dynamic review helps you become familiar with how correctly segmented regions usually appear and what kinds of errors to expect.

#### For the full dataset, go per region  
Most raters prefer evaluating one cortical region at a time across all subjects. This helps with consistency and speeds up spotting systematic issues. As you go, **flag any doubtful cases**, either to revisit later or to share with the ENIGMA core team for second opinion.

### 6. Subcortical Quality Control
After the cortical regions, the subcortical quality control is a piece of cake. Subcortical regions are located close together and can usually be assessed in a single glance (in contrast to the region-by-region approach of the cortical segmentations). Segmentation errors are uncommon, and most regions should pass. Approve unless there are clear, severe distortions, which often affect multiple or all neighboring subcortical regions rather than just one.

---

### Things to Keep in Mind

- **Be lenient**: Only exclude segmentations with **clear, severe errors** that would lead to **invalid regional estimates**.  
- As you review more subjects, it becomes easier to recognize these major failures quickly.  
- **When in doubt, keep the data**, borderline cases are usually acceptable for analysis.