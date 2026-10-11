# EvolvDx fork of nf-core/rnafusion

`master` mirrors nf-core/rnafusion. EvolvDx changes land on **`evolvdx-main`**, which is the
revision the automated runs use. Never open pull requests against nf-core upstream.

## Changes from upstream (4.1.3)

- `--trim_front1` / `--trim_front2`: clip a fixed number of bases from the 5' end of read 1 / read 2
  with fastp (needs `fastp` in `--tools`). Our Takara SMARTer Stranded Total RNA-Seq Pico v3
  libraries start read 2 with an 8-base UMI and a 6-base linker, so runs use `--trim_front2 14`.
  rnafusion has no UMI deduplication; Arriba flags PCR duplicates itself and STAR-Fusion counts
  reads as aligned.

## How runs are launched

Runs go through the EvolvDx automation (EVolvDx/evolvdx_data_infrastructure): a `run.json`
with `"pipeline": "EVolvDx/rnafusion"`, `"revision"` pinned to an `evolvdx-main` commit and
`"profile": "rnafusion_grch38"`. That profile (rendered into the Batch site config) points
`genomes_base` at the pinned references in `s3://evolvdx-references-<acct>/rnafusion/references/`
and sets the run defaults (`tools`, `trim_front2`). STAR-Fusion and Arriba results are loaded
into PlacidDB (`rnaseq_starfusion`, `rnaseq_arriba`) by `placid-load-rnafusion`
(EVolvDx/evolvdx_db_utils).
