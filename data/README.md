# Input data

The analysis uses one tab-separated file downloaded from the [UCSC Xena browser](https://xenabrowser.net) for the
TCGA breast invasive carcinoma cohort (TCGA-BRCA). The file is **not** included in this repository.

| Property | Value |
|---|---|
| File name | `denseDataOnlyDownload-3.tsv` |
| SHA-256 | `491c44d60cbfa3ae2dd89b5d151679cd6dee425fad7724aee6f6b2ec7a8c7b2e` |
| Rows | 1,247 samples (1,101 primary tumour, 139 solid normal tissue, 7 metastatic) |
| Columns | 68: `sample`, `samples`, `pathologic_stage`, and expression values for 65 genes |

The 65 genes are:

> TP53, BRCA1, BRCA2, ERBB2, PIK3CA, MYC, EGFR, PTEN, AKT1, KRAS, BRAF, CDK2, CDK4, CDK6, RB1, CCND1, MAPK1, MAPK3,
> MTOR, VEGFA, FGFR1, STAT3, JAK2, SMAD4, NOTCH1, GATA3, ESR1, FOXA1, AR, MDM2, CDH1, CTNNB1, ATM, CHEK2, PALB2, NFKB2,
> NFKB1, REL, FOS, JUN, HIF1A, TGFB1, TGFBR2, SMAD2, SMAD3, PDGFRA, KIT, FLT3, SRC, ABL1, EP300, CREBBP, ARID1A, ARID1B,
> NCOR1, NCOR2, RUNX1, RUNX2, SOX2, SOX9, POU5F1, NANOG, CD44, ALDH1A1, PROM1

To reproduce exactly, place the file in this folder. The notebook verifies the checksum; a different download (for example
a different Xena dataset version or gene list) will produce a warning and may give different results.

## Source datasets

The file was exported from the UCSC Xena browser as one table combining two datasets on the **TCGA hub**
(`https://tcga.xenahubs.net`):

| Content | Xena dataset | Description | Version |
|---|---|---|---|
| Gene expression (65 genes) | `TCGA.BRCA.sampleMap/HiSeqV2_PANCAN` | IlluminaHiSeq pan-cancer normalised RNA-seq: RSEM normalised counts, log2(x + 1), mean-centred per gene across all TCGA cohorts | 2017-10-13 |
| AJCC pathologic stage (`pathologic_stage`) | `TCGA.BRCA.sampleMap/BRCA_clinicalMatrix` | TCGA-BRCA phenotypes | 2019-12-06 |

1,218 of the 1,247 rows have expression values, which matches the sample count of the expression dataset.
