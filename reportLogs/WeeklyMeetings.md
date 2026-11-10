### Date: [10/11 2026]

#### Who did you help this week?

-

#### What helped you this week?

* The Purrlab writing guide for the project report.
* Reusing the PadChest training and prediction pipeline from the original implementation made it much easier to set up the Harvard-GF classification experiments.

#### What did you achieve?

* Continued writing the paper draft, including the methodology, preprocessing, diversity metrics and parts of the results section based on notes made throughout
* Adapted the original training and prediction scripts to support Harvard-GF.
* Set up and tested a ResNet-50 training pipeline for Harvard-GF using 5-fold cross-validation and early stopping. (epoch 100, patience 5)
* Trained models for all six Harvard-GF scenarios: All, Female, Male, Asian, Black and White.
* Generated downstream classification performance results (AUC) for all scenarios.
* Obtained the following mean AUCs: All: 0.800, Female: 0.779, Male: 0.800, Asian: 0.746,White: 0.773, Black: 0.774



#### What did you struggle with?

* Debugging HPC jobs and ensuring code changes were available on the correct branch and environment.
* Adapting code that was originally written for PadChest to work correctly with Harvard-GF.
* Debugging the prediction script and verifying that all scenario results were written correctly to the final AUC file.
* Deciding how much time to spend investigating alternative preprocessing choices compared to moving forward with the performance experiments.

#### What would you like to work on next week?

* Merge the diversity metric results with the AUC results and analyse their relationship using the same methodology as the original paper.
* Compare Harvard-GF scenario rankings induced by diversity metrics and classification performance
* Generate figures and tables for the results section
* Finish the "Metric ranking versus classification performance" subsection
* Continue improving the discussion and interpretation of the results

#### Where do you need help from Veronika?

* How should I compare the diversity metric results and the AUC results? Is ranking-based analysis (e.g. Spearman correlation, as used in the original paper) sufficient, or would you recommend additional analyses?
* How much emphasis should I place on the normalisation experiments in the final report, given that the main project goal is reproducing the findings on a new dataset?
* We discussed using race instead of scanner. How should I discuss this difference when interpreting the results and comparing them to the original study?


#### Any other topics

-



### Date: [3/11 2026]

#### Who did you help this week?

-

#### What helped you this week?

Running separate normalization experiments in a notebook made it easier to isolate the effect of preprocessing choices before integrating them into the full metric pipeline. And using the HPC guide to actually run it successfully.

#### What did you achieve?

* Emailed the Harvard-GF authors and got a reply: -2 is the optic disc and -1 the optic rim (region labels, not thickness). They did not answer about 0, the units or normalisation
* Integrated the Harvard-GF dataset into the diversity evaluation pipeline.
* Created preprocessing and dataset-loading code for Harvard-GF
* Implemented different normalization strategies: raw, fixed, clip350, disc_zero, minmax
* Looked at the different normalisation strategies across the som of the metrics and demographics using notebook
* Successfully ran the Harvard-GF diversity pipeline on the HPC and generated diversity metrics for: All, Female, Male, Asian, Black, White using the fixed normalisation


#### What did you struggle with?

* Deciding which normalization best matches the intended use of Theos framework.
* Understanding the original Harvard-GF preprocessing ->
* My first Harvard-GF job failed because the HPC checkout was on the wrong branch. A full MorphoMNIST scenario takes about 5 hours, so I could only run one.
* Determining whether using a different normalization could affect comparison with Theos published results.


#### What would you like to work on next week?

* Compare the remaining metrics ( FID and Inception-based metrics) across normalization strategies.
* Decide on a final preprocessing pipeline for Harvard-GF.
* Begin comparing Harvard-GF results directly with the patterns observed in Theo's study.

#### Where do you need help from Veronika?

* Is fixed normalization + robustness checks a reasonable strategy?
* Do i need to validate FID/Inception too, or is the Vendi analysis enough?
* Should i keep the current encoding used in Harvard-GF, switch to one-hot encoding, or compare both approaches?
* Is it the right time to move on to classifier training/AUC experiments, or should I finish investigating the preprocessing and normalization choices first?
* At what point would you recommend I start writing? Should I begin drafting sections such as the dataset description, preprocessing decisions, and methodology now, or wait until more of the experiments are complete?



### Date: [20/10 2026]

#### Who did you help this week?
-


#### What helped you this week?

Meeting with Veronika helped clarify that unusual values should not automatically  be discarded simply because they lie out of the normality.

#### What did you achieve?

* Investigated the distribution of RNFLT values across 1000 randomly sampled images
* Histrogram analysis on number of pixels at specific RNFLT values in the sample
* Identified that -1 and -2 are some kind of structured data and not random noise
* Identified that all positive RNFLT values lie on a quantization grid with spacing of about 1.95503
* Investigated different images at different pixel value ranges.
* Investigated some of the highest valued RNFLT images and saw that they retained the charachteristics.
* Looked further into the code of the original Harward-GF project (found they scale using a fixed 350, and clip RNFLT values to the range (-2,350) before training)

#### What did you struggle with?

* Determining the exact meaning of the values -2,-1 and 0 because they are not fully documented in the paper
* Understanding wether the RNFLT images had undergone preprocessing or not
* Deciding on an appropriate normalization strategy
* Using the hpc... but i am getting there

#### What would you like to work on next week?

* Finalise the RNFLT preprocessing and normalisation strategy

#### Where do you need help from Veronika?

* How to treat -1 and -2 (and potentially 0)
* Feedback on normalisation methods


#### Any other topics

-



### Date: [06/10 2026]

#### Who did you help this week?

-

#### What helped you this week?

Theos code on how to preprocess data. Potentially can lean on Padchest preprocessing for Harward-GF data.

#### What did you achieve?

* Looking at comparison metrics and deciding on which can be used and which cannot
  * image metrics IS/FID/Vendi×3 and metadata diversity can be used
  * RougeL and semantic diversity cannot be used since there's no text modality
* Took a further look into the data of the Harward-GF and how it should be preprocessed
* Made rough draft of HarvardGFDataset, modeled closely on PadchestDataset, matching the __getitem__ contract the metric functions expect (image shape, metadata vector format)
* Also made a rough draft of process_harvardgf() preprocessing step mirroring process_padchest(), so metadata gets extracted from the raw .npz files once and cached to csv, rather than reloaded every run


#### What did you struggle with?

* Havent ran the code on HPC (mostly because of time limitations)

#### What would you like to work on next week?

* Finish drafts of HarvardGFDataset and process_harvardgf()
* Test HarvardGFDataset
* Get GPU confirmed and benchmarked on the HPC

#### Where do you need help from Veronika?

* Race encoding for the metadata vector: ordinal (0/1/2) vs. true one-hot
* Given the original paper found most of the metrics (IS, Vendi pixel/HOG, metadata diversity) didn't correlate well with AUC even in their own data, what is the goal in running them on Harvard-GF anyway? Confirming the same null result, or hoping something different shows up here?

#### Any other topics

This space is yours to add to as needed.


### Acknolodgement
The material in Dr. Mystery's Lab Guide is partially derived from "Whitaker Lab Project Management" by Dr. Kirstie Whitaker and the Whitaker Lab team, used under CC BY 4.0. Dr. Mystery's Lab Guide is licensed under CC BY 4.0 by Julia Klompmaker.



### Date: [29/09 2026]

#### Who did you help this week?

-

#### What helped you this week?

Theos readme on how to run his experiments as well as documentation from pytorch as to how to run it on mac.

#### What did you achieve?

* Cloning theos repo + downloading Morph-data
* Making research project repo
* Downloading Harward-GF data
* Doing som initial dataset runs of GF data

#### What did you struggle with?

* Getting Theos model up and running on my computer, without actually downloading the heavy padchest data

#### What would you like to work on next week?

* Further look into Harward-GF data
* Maybe fit data to theos model

#### Where do you need help from Veronika?

* Using the built in splits from the Harward-GF dataset, or not? There is a train, validate, test split as of now.
* 2d and 3d images(we discussed this last time, but i forgot what conclusion we came to)
* How should i add Theos repo to mine. Fork it?

#### Any other topics

This space is yours to add to as needed.


### Acknolodgement
The material in Dr. Mystery's Lab Guide is partially derived from "Whitaker Lab Project Management" by Dr. Kirstie Whitaker and the Whitaker Lab team, used under CC BY 4.0. Dr. Mystery's Lab Guide is licensed under CC BY 4.0 by Julia Klompmaker.


### Date: [Template]

#### Who did you help this week?

Replace this text with a one/two sentence description of who you helped this week and how.


#### What helped you this week?

Replace this text with a one/two sentence description of what helped you this week and how.

#### What did you achieve?

* Replace this text with a bullet point list of what you achieved this week.
* It's ok if your list is only one bullet point long!

#### What did you struggle with?

* Replace this text with a bullet point list of where you struggled this week.
* It's ok if your list is only one bullet point long!

#### What would you like to work on next week?

* Replace this text with a bullet point list of what you would like to work on next week.
* It's ok if your list is only one bullet point long!
* Try to estimate how long each task will take.

#### Where do you need help from Veronika?

* Replace this text with a bullet point list of what you need help from Veronica on.
* It's ok if your list is only one bullet point long!
* Try to estimate how long each task will take.

#### Any other topics

This space is yours to add to as needed.


### Acknolodgement
The material in Dr. Mystery's Lab Guide is partially derived from "Whitaker Lab Project Management" by Dr. Kirstie Whitaker and the Whitaker Lab team, used under CC BY 4.0. Dr. Mystery's Lab Guide is licensed under CC BY 4.0 by Julia Klompmaker.
