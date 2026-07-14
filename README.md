# eggd_atlas_main_workflow  (DNAnexus Platform Workflow)
DNAnexus workflow for solid cancer pipeline (Atlas) for the Twist CGP assay.
This is a DNAnexus workflow that implements the Atlas pipeline for solid cancer samples.
The workflow is designed to process sequencing data generated from the Twist CGP assay, performing alignment, variant calling, and annotation for small variants and gather QC metrics.

---

## What apps are used in this workflow?

| App                            | Version |
| ------------------------------ | ------- |
| sentieon-tnbam                 | 5.1.0   |
| eggd_verifybamid               | 2.3.0   |
| eggd_picard_QC                 | 1.4.0   |
| eggd_samtools_flagstat         | 1.1.0   |
| eggd_mosdepth                  | 1.2.0   |
| eggd_athena                    | 1.6.2   |
| eggd_sex_check                 | 1.2.1   |
| eggd_sompy                     | 1.0.5   |
| eggd_vcf_normaliser            | 1.0.0   |
| eggd_vep                       | 1.3.0   |

## Workflow Diagram

```mermaid
graph LR
  S_UMI["Seperate Sentieon UMI app launched by conductor"]
  S_TNBAM["stage-sentieon_tnbam"]
  VERIFY["stage-verifybamid"]
  PICARD["stage-picard"]
  FLAG["stage-flagstat"]
  MOS["stage-mosdepth"]
  ATH["stage-athena"]
  SEX["stage-sex_check"]
  SOMPY["stage-sompy"]
  VCF_NORM["stage-vcf_normaliser"]
  VEP["stage-eggd_vep"]

  %% Primary data flow edges (from JSON links)
  S_UMI --> S_TNBAM
  S_UMI --> VERIFY
  S_UMI --> PICARD
  S_TNBAM --> PICARD
  S_UMI --> FLAG
  S_UMI --> MOS
  MOS --> ATH
  S_UMI --> SEX
  S_TNBAM --> SOMPY

  %% VCF normalization / annotation flow
  S_TNBAM --> VCF_NORM
  VCF_NORM --> VEP


```
