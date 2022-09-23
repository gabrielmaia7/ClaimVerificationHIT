# WTR's Crowdsourced Annotation and ProVe's Evaluation

This repository is part of the work found in the paper ProVe: A Pipeline for Automated Provenance Verification of Knowledge Graphs against Textual Sources

This repository serves **three** main purposes:
1. It contains all the code and data used to **gather crowd annotations for WTR, the evaluation dataset used to measure ProVe's performance**.
2. It merges KG triples and references sampled from Wikidata with the gathered crowd annotations to **compile WTR**.
3. It contains all the code and data that uses the annotated WTR to **evaluate ProVe's performance**.

Here is the description of the repository's structure and contents:

- *single*: This directory holds the task mockup and the HTML template for the task design **T1**, which asks workers to annotate the **individual** stance of a single piece of evidence in relation to a given verbalised triple (the claim).
- *multiple*: This directory holds the task mockup and the HTML template for the task design **T2**, which asks workers to annotate the **collective** stance of a set of 5 passages (the evidence set) in relation to a given verbalised triple (the claim).
- *task_deploy_and_analysis*: This directory holds all the code and data used to (1.) carry the crowdsourcing task, creating tasks based on T1 and T2's designs and launching them on Mechanical Turk. It also contains (2.) the merging of these annotations with the data sampled from Wikidata to create WTR, and (3.) the analysis of ProVe's results based on such annotations.
  - A detailed README describing its contents can be found within the directory.

