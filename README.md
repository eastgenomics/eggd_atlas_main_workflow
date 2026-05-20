# eggd_atlas_main_workflow  (DNAnexus Platform Workflow)
DNAnexus workflow for solid cancer pipeline based on the Uranus workflow.

---

## What apps are used in this workflow?

| App                            | Version |
| ------------------------------ | ------- |
| eggd_sentieon_umi              | 1.0.0   |
| sentieon-tnbam                 | 5.1.0   |
| eggd_verifybamid               | 2.2.1   |
| eggd_picard_QC                 | 1.3.0   |
| eggd_samtools_flagstat         | 1.1.0   |
| eggd_mosdepth                  | 1.1.0   |
| eggd_athena                    | 1.4.0   |
| eggd_sex_check                 | 1.1.0   |
| eggd_sompy                     | 1.0.5   |
| eggd_apheleia                  | 1.0.1   |
| eggd_vep                       | 1.3.0   |
| eggd_vcf_rescue                | 1.2.0   |
| eggd_generate_variant_workbook | 2.11.1  |

## Workflow Diagram

```mermaid
graph LR
  S_BWA["stage-sentieon_bwa"]
  S_UMI["stage-sentieon_umi"]
  S_TNBAM["stage-sentieon_tnbam"]
  CNV["stage-cnvkit"]
  VERIFY["stage-verifybamid"]
  PICARD["stage-picard"]
  FLAG["stage-flagstat"]
  MOS["stage-mosdepth"]
  ATH["stage-athena"]
  SEX["stage-sex_check"]
  SOMPY["stage-sompy"]
  APH["stage-apheleia"]
  VN_MUT["stage-vcf_normaliser_mutect2"]
  VEP_MUT["stage-eggd_vep_mutect2"]
  RESCUE["stage-eggd_vcf_rescue"]
  GENWB["stage-eggd_generate_variant_workbook"]

  %% Primary data flow edges (from JSON links)
  S_BWA --> S_TNBAM
  S_UMI --> S_BWA
  S_BWA --> CNV
  S_BWA --> VERIFY
  S_BWA --> PICARD
  S_TNBAM --> PICARD
  S_BWA --> FLAG
  S_BWA --> MOS
  MOS --> ATH
  ATH --> APH
  S_BWA --> SEX
  S_TNBAM --> SOMPY

  %% VCF normalization / annotation flow
  S_TNBAM --> VN_MUT
  VN_MUT --> VEP_MUT
  VEP_MUT --> RESCUE
  RESCUE --> GENWB

  %% Independent/standalone nodes
  CNV
```
