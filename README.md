# Variant-Caling-in-RNA-Seq-
![image](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-1-0716-2293-3_13/MediaObjects/504146_1_En_13_Fig1_HTML.png)
# Variant-Calling-from-RNA-seq-Data-Using-the-GATK
<img align="right" alt="coding" width ="800" src= "https://camo.githubusercontent.com/8dd320d95014aec88352fc2dd35a837cf5614910bd2f7d5f3a44a73207d6de55/68747470733a2f2f75732e762d63646e2e6e65742f353031393739362f75706c6f6164732f46696c6555706c6f61642f66612f6536306563663839626431623236343564396663653638636366333931392e706e67">

>In brief, the key modifications made to the DNAseq Best Practices focus on handling splice junctions correctly, which involves specific mapping and pre-processing procedures, as well as some new functionality in the HaplotypeCaller. Here is a detailed overview:
<img align= "right" alt="coding" width="800" src="https://camo.githubusercontent.com/032092918e223dd9f9e6e787e103e1a435bdbf0edf6cc67b1bd20ebd2aa94234/68747470733a2f2f75732e762d63646e2e6e65742f353031393739362f75706c6f6164732f46696c6555706c6f61642f63392f6163343637383462653339663331666139373662356163393434646531372e706e67">

# Workflow

<img align="right" alt="coding" width="800" src="https://gatk.broadinstitute.org/hc/theming_assets/01HZPKW49MR2MNGABV15G08EYE">
RNA-Seq variant calling using GATK (Genome Analysis Toolkit) involves several key steps. Below is a step-by-step guide to perform variant calling on RNA-Seq data:

### 1. **Quality Control (QC)**
   - **Tools**: FastQC, MultiQC
   - **Steps**:
     1. Run `FastQC` on the raw FASTQ files to assess the quality of the sequencing reads.
     2. Use `MultiQC` to aggregate and visualize the results from multiple `FastQC` reports.

### 2. **Read Trimming (Optional)**
   - **Tools**: Trimmomatic, Cutadapt
   - **Steps**:
     - Trim adapters and low-quality bases from the reads if necessary.

### 3. **Alignment to Reference Genome**
   - **Tools**: STAR, HISAT2
   - **Steps**:
     1. Align the RNA-Seq reads to the reference genome using `STAR` or `HISAT2`.
     2. Generate a BAM file sorted by coordinates.

### 4. **Mark Duplicates**
   - **Tools**: GATK (MarkDuplicates), Picard
   - **Steps**:
     1. Use `GATK MarkDuplicates` or `Picard MarkDuplicates` to identify and mark duplicate reads in the BAM file.

### 5. **Split'N'Trim and Reassign Mapping Qualities**
   - **Tools**: GATK
   - **Steps**:
     1. Use `GATK SplitNCigarReads` to split reads into exonic segments and hard-clip any overhanging portions.
     2. Reassign mapping qualities from STAR (MAPQ=255) to a lower value (e.g., 60) using `SplitNCigarReads`.

### 6. **Base Quality Score Recalibration (BQSR)**
   - **Tools**: GATK
   - **Steps**:
     1. Perform Base Quality Score Recalibration (BQSR) using `GATK BaseRecalibrator`.
     2. Generate recalibrated BAM files.

### 7. **Variant Calling**
   - **Tools**: GATK HaplotypeCaller
   - **Steps**:
     1. Run `GATK HaplotypeCaller` with the RNA-Seq-specific settings (`-ERC GVCF` mode recommended) to call variants.

### 8. **Post-Processing**
   - **Steps**:
     1. Combine GVCFs (if working with multiple samples) using `GATK CombineGVCFs`.
     2. Genotype GVCFs using `GATK GenotypeGVCFs`.

### 9. **Variant Filtering**
   - **Tools**: GATK VariantFiltration
   - **Steps**:
     1. Filter variants using `GATK VariantFiltration` with appropriate RNA-Seq-specific filters.
     2. For SNPs and indels, you may apply the following filters:
        - SNPs: `QD < 2.0 || FS > 30.0 || MQ < 40.0 || MQRankSum < -12.5 || ReadPosRankSum < -8.0`
        - Indels: `QD < 2.0 || FS > 200.0 || ReadPosRankSum < -20.0`

### 10. **Annotation**
   - **Tools**: ANNOVAR, SnpEff, VEP
   - **Steps**:
     1. Annotate the variants to determine their potential impact using `SnpEff`, `VEP`, or `ANNOVAR`.

### 11. **Visualization and Interpretation**
   - **Tools**: IGV (Integrative Genomics Viewer)
   - **Steps**:
     1. Visualize the aligned reads and called variants in `IGV` to ensure accuracy.
     2. Interpret the variants in the context of the biological question being studied.

This workflow should cover the essentials of RNA-Seq variant calling using GATK. Each tool has specific parameters that may need to be adjusted depending on the dataset and research question.
