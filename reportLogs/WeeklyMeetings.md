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
