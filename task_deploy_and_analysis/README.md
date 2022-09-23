This directory holds all the code and data used to:

1. Carry out the crowdsourcing task, creating tasks based on T1 and T2's designs and launching them on Mechanical Turk.
2. Perform the merging of these annotations with the data sampled from Wikidata to create WTR.
3. Perform the analysis of ProVe's results based on such annotations.

WTR is available [here](https://figshare.com/s/df0ec1c233ebd50817f4) as a complete dataset which integrated **data on triples**, **data on references**, and **crowd annotations**. 
However, during crowdsourcing and evaluation, WTR was used as a collection of csv files rather than a unified dataset, each holding a portion of the features described in the Figshare version. These were later joined into the version available on Figshare through code seen in this repository.

Here is the description of this directory's structure and contents:

- *config*: This directory holds all **configuration files** used for **crowdsourcing** the WTR annotations.
  - amazon_credentials_template.json: This file is a template to hold your amazon credentials, in order to use Amazon's Mechanical Turk. Replace the access and secret keys with your own and change the name of the file to 'amazon_credentials.json'. Make sure your amazon AWS account has access to MTurk and has a MTurk requester account linked to it.
  - mongodb_credentials.json: This file is a template to hold your mongodb credentials, in order to access a mongodb instance to continuously save crowdsourcing results as they come. Replace the connection string with your own and change the name of the file to 'mongodb_credentials.json'. You can use other databases for this but that would require changing quite a bit of the code in this repo.
  - banlist.json: A list of Worker IDs from Mechanical Turk who have previously been identified as scammers.
  - *pilot*: This directory holds config files that are specific to the crowdsourcing pilot experiment.
    - task_config_single.json: This file holds the configurations for the T1 tasks.
    - task_config_multiple.json: This file holds the configurations for the T2 tasks.
  - *campaign*: This directory holds config files that are specific to the main crowdsourcing campaign.
    - task_config_single.json: This file holds the configurations for the T1 tasks.
    - task_config_multiple.json: This file holds the configurations for the T2 tasks.
- *data*: This directory holds all the data used in the crowdsourcing of WTR's annotations, as well as in the evaluation of ProVe. It also contains a script that compiles WTR into a stand-alone dataset by joining the different datafiles that compose it.
  -  *pilot*: This directory holds all the data used in the crowdsourcing pilot experiments.
     -  single.json: This is the triple-evidence pairs that will be given to the crowd to be annotated as part of T1 tasks.
     -  single_gd.json: Same as single.json, but these have been pre-annotated by the authors and will be used as gold standard for quality assurance.
     -  single_TaskSets.json: The pairs in single.json and single_gd.json are organised into task sets such that each task set has 6 pairs for annotation, 2 being gold standards, and each task set is put inside a T1 template to form a task that will be annotated by 5 distinct crowd workers.
     -  multiple.json: This is the triple-evidence pairs that will be given to the crowd to be annotated as part of T1 tasks.
     -  multiple_gd.json: Same as single.json, but these have been pre-annotated by the authors and will be used as gold standard for quality assurance.
     -  multiple_TaskSets.json: The pairs in single.json and single_gd.json are organised into task sets such that each task set has 6 pairs for annotation, 2 being gold standards, and each task set is put inside a T1 template to form a task that will be annotated by 5 distinct crowd workers.
  -  *campaign*: This directory holds all the data used in the main crowdsourcing campaign. It is structured identically to its sibling directory *pilot*.
  -  language_tests.json: This contains the language questions used as attention tests for the tasks in both T1 and T2.
  -  reference_html_as_sentences_df.csv: This file holds all the data on references sampled from Wikidata and which form WTR, alongside each reference's sentences and passage sets as extracted by ProVe.
  -  verbalised_claims_df_final.csv: This file holds all the data on claims sampled from Wikidata, associated to the references in 'reference_html_as_sentences_df.csv'. Alongside 'reference_html_as_sentences_df.csv', this forms the non-annotated triple-reference pairs of WTR.
  -  textual_entailment_df.json: This file holds the results from executing ProVe on the triple-reference pairs of WTR.
  -  textual_entailment_df_filtered_for_crowdsourcing.json: This is the same as textual_entailment_df.json, but only the results obtained by running ProVe on the union of all passage sets is kept.
  -  unique_references_for_manual_annotation.csv: This file holds the author annotations of each triple-reference pair of WTR.
  -  Compile_WTR.ipynb: This notebook unites the triple, reference, and annotation data into the version of WTR found in Figshare.
  -  *WTR*: This directory holds the data used to build WTR.
     -  *htmls*: This directory holds the HTML contents of each reference in WTR.
     -  WTR_non_filtered_non_annotated.csv: This file unites the triple and reference data also seen in 'reference_html_as_sentences_df' and 'verbalised_claims_df_final' into a single dataframe that details the sampled data without including by-products of ProVe's execution.
     -  WTR.json: The version of WTR found in Figshare.
- *notebooks*: This directory holds the python notebooks and scripts used to run crowdsourcing tasks, inspect the evaluation data, and perform ProVe's evaluation.
  - Distributions_Claims+Verbs.ipynb: This notebook takes the file 'verbalised_claims_df_final.csv', which holds the data on the 409 uniquely verbalised triples that make up WTR, and analyses it, showing and plotting its value distributions.
  - Distributions_References.ipynb: This notebook takes the file 'reference_html_as_sentences_df.csv', which holds the data on the 416 unique references sampled for WTR (409 of which constitute WTR, with 7 dropped due to having repeated URLs and verbalisations) and analyses it, showing and plotting its value distributions.
  - Investigating_Final_Pipeline_Output.ipynb: This notebook takes the file 'textual_entailment_df.json', which holds the data on the execution of ProVe's pipeline over the 409 uniquely verbalised triples that make up WTR, and analyses it, showing and plotting its value distributions.
  - GenerateTaskSets.ipynb: This notebook takes all the 409 verbalised triple with their evidence sets, where evidence was selected from the union of all differently-sized windows, available in the file 'textual_entailment_df_filtered_for_crowdsourcing.json', and divides triple-evidence bundles into task sets. Each task set contains 4 triple-evidence pairs for annotation and 2 that are pre-annotated and serve as gold standard.
  - ExperimentRunner.ipynb: This notebook takes the data prepared by GenerateTaskSets.ipynb and inserts each task set into the T1 and T2 templates, effectively launching the tasks into MTurk.
  - Pilot_Results.ipynb: This notebook analyses the results from pilot crowdsourcing runs. Analysis is just enough to twitch parameters like compensation and instructions.
  - Campaign_Results.ipynb: This notebook takes the results from the main crowdsourcing campaign, explores and aggregates them, and merge them with the rest of the WTR data as processed by ProVe (found mainly at 'textual_entailment_df_filtered_for_crowdsourcing', 'reference_html_as_sentences_df', and 'verbalised_claims_df_final') in order to evaluate ProVe's performance.
  - fleiss.py: This script implements Fleiss' agreement metrics.
  - krippendorff_alpha.py: This script implements Krippendorff's Alpha, an agreement metric.
  - mturk.py: This module has lots of functions and abstractions to allow for the interface with Mechanical Turk.
  - update_db.py: This script goes in a loop and will update the mongodb with results coming from the crowd every few seconds.
- *imgs*: This directory holds all images extracted from ProVe's analysis to be placed on the paper and presentations.

