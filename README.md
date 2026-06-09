# Non-coding RNA Discovery in Glioblastoma Using Long-Read Sequencing

## Overview
An end-to-end bioinformatics pipeline for discovering novel lncRNAs and circRNAs 
in glioblastoma (GBM) using Oxford Nanopore long-read RNA sequencing. Applied to 
26 glioma patients (17 GBM, 9 LGG; dataset PRJNA1189527), the pipeline identifies 
novel non-coding RNA candidates absent from public databases and prioritises them 
using a composite multi-evidence scoring framework.

## Key Results
- 931 non-coding transcripts → 336 unique lncRNA loci
- 29 novel lncRNA loci (absent from LNCipedia v5.2 and GENCODE v44)
- 3 priority lncRNA candidates (novel + HIGH confidence + brain-silent)
- Top candidate: LNC_L00295/POLR2J2 (composite score = 1.000, PPAR signalling)
- 393 unique circRNA junctions, 392 novel
- 4 high-confidence circRNA sponge candidates (miRDB v6.0, score ≥ 80)
- Striking circRNA enrichment in mitochondrial ETC genes (MT-ND6, MT-CO1, MT-ND1)
  suggesting a link to GBM Warburg metabolic reprogramming

## Pipeline
Phase 1: QC           NanoStat + NanoFilt (Q≥7, length≥200bp)
Phase 2: Alignment    minimap2 → SAMtools → FLAIR (lncRNA) / CIRI-long (circRNA)
Phase 3: Classification  FEELnc + CPC2 → BEDTools → LNCipedia/GENCODE cross-ref
Phase 4: Structure    RNAfold (MFE/nt) + miRDB v6.0 sponge prediction
Phase 5: Functional   DESeq2 + Spearman co-expression + clusterProfiler + composite 5-component prioritisation score
## Reproduction
This pipeline was developed in Google Colab with data mounted from Google Drive.

1. Download raw reads from NCBI SRA (BioProject PRJNA1189527)
2. Mount to Google Drive under `MyDrive/bioinfo_project/`
3. Open `bioinfo_project.ipynb` in Google Colab
4. Run cells sequentially — see `environment.yml` for software versions

## Software Versions
See `environment.yml` for full dependency list. Key tools:
NanoFilt 2.8.0 | minimap2 2.26 | SAMtools 1.17 | FLAIR 2.0 | CPC2 1.0.1
CIRI-long 1.0 | ViennaRNA 2.6 | BEDTools 2.31 | DESeq2 1.50.2
clusterProfiler 4.18.4 | R 4.5.3 | Bioconductor 3.22

## Results
All output files are in `results/` organised by phase:
- `results/phase3/` — annotated lncRNA and circRNA tables
- `results/phase4/` — RNAfold outputs, structural disorder scores
- `results/phase5/` — DESeq2 results, prioritisation scores, 17 figures

## Dataset
NCBI BioProject: [PRJNA1189527](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA1189527)
