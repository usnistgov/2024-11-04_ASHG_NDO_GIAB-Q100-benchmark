# ASHG 2024 GIAB Q100 Variant Benchmark Poster Presentation

## Title

New diploid assembly-based Genome In A Bottle HG002 T2T “Q100” small and structural variant benchmark sets.

## Authors

- N.D. Olson
- N. Dwarshuis
- J.M. McDaniel
- J. Wagner
- J.M. Zook
- T2T Q100 Consortium
- GIAB Consortium

## Abstract

Genome in a Bottle was started over ten years ago to develop well-characterized
human genome reference materials for validating genome sequencing and variant
calling methods. GIAB has produced genomic DNA reference materials and benchmark
sets composed of high-confidence variant calls and regions confidently
identified as homozygous reference or as high-confidence variants. These
benchmark sets are widely used to evaluate and train variant calling methods.
Traditionally, these benchmark sets are developed by integrating variant
callsets generated using different sequencing technologies and variant calling
algorithms. However, this process involves mapping reads to a reference, which
is limited in its ability to characterize more complex genomic regions. With
recent advances in sequencing methods and genome assembly algorithms, creating
high-quality diploid de novo genome assemblies is possible. A subgroup of the
T2T consortium that generated the first complete human genome assembly, CHM13,
is working on a highly polished and curated T2T diploid genome assembly of GIAB
HG002 (son of the Ashkenazi Jewish trio.) While previous GIAB benchmark sets
have used diploid assemblies for particular genomic regions, we use the HG002
Q100 T2T diploid assembly to generate the first whole genome assembly-based
small and structural variant benchmark sets. These benchmark sets were generated
using a new Snakemake-based pipeline and include regions of the genome that were
excluded when using read-mapping-based benchmark generation methods, e.g., the
highly polymorphic MHC, gene conversions, and more complex SVs. The small
variant benchmark set includes ~ 89.8% ( 2.74 Gbp) of the HG002 assembly with
>3.6 million SNVs and ~950,00 indels (>50bp), >700,000more variants than the
previous v4.2.1 benchmark. The structural variant benchmark set covers nearly
90.3% (2.75 Gbp) of the HG002 assembly and nearly 30,000 variants. This
represents a significant improvement over the previous v0.6 HG002 SV benchmark,
which covered only 2.51Gbp and included ~10k variants. Draft versions of the
benchmark sets are publicly available and undergoing external validation before
official release.

## Background

### Variant Calling and Benchmarking

- GA4GH Benchmarking team best practices for small variants includes;
- Comparison methods that account for variant representation differences,
- Matching and performance metric definitions, and
- Stratifying results by variant type and context.
- Summarized in Krusche et al. 2019 (doi.org/10.1038/s41587-019-0054-x) Table S1
- Best practices implementation (github.com/Illumina/hap.py).
- Web application available on precisionFDA (precision.fda.gov)
- precisionFDA accounts required for access available upon request.
- Stratifications (for both small and structural variant benchmarking)
  - Set of bed files defining genomic regions for stratifying benchmarking
    results. Useful for indentifying regions where variants calls are unreliable
    or can be optimized.
  - Stratifications for GRCh37, GRCh38, CHM13, and HG002 maternal and paternal haplotype
  - Preprint describing stratifications and pipeline to automate generation
  - Preprint: https://doi.org/10.1101/2023.10.27.563846
  - Pipeline: https://github.com/ndwarshuis/giab-stratifications
- Small Variant Benchmarking Tools
  - hap.py (https://github.com/Illumina/hap.py)
  - vcfeval (https://github.com/RealTimeGenomics/rtg-tools)
- Structural Variant Benchmarking Tools:
  - TRUVARI (github.com/spiralgenetics/truvari)
  - hap-eval (https://github.com/Sentieon/hap-eval)
- Combined Small and Structural Variant Benchmarking tool:
  - vcfdist (https://github.com/TimD1/vcfdist)

## Benchmark Set Generation

### Q100 HG002 Diploid Assembly

[High accuracy T2T diploid assembly for HG002](https://github.com/marbl/HG002)

### Development Framework for Assembly-Based Benchmarks (DeFrabb)

Snakemake pipeline created to facilitate benchmark
development process in a transparent and well documented
manner.

- Streamlining benchmark set development process.
- Easily generates small and structural variant benchmarks for multiple GIAB RMs against multiple reference genomes (GRCh37, GRCh38, and CHM13).

Code: https://github.com/usnistgov/defrabb

Developed primarily for internal use to generate draft benchmarks that
will undergo internal and external evaluation before formal release.

- Configuration file defines input files and tool parameters
- Analysis table defines assembly-base variant calls, benchmark set and evaluations
- Assembly-based variant calls using dipcall
  - minimap2 used for assembly-assembly alignments
  - variants called using paftools.js
  - diploid regions defined using bedtk
- Benchmark set:
  - Variants: dipcall output vcf annotated using truvari anno
  - Regions: excludes regions with known systematic errors in assembly and assembly-assembly errors.
- Evaluations:
  - Method: hap.py and Truvari bench currently
  - Truth and query callsets
  - Truth regions and target regions
- Automatically calculates summary stats vcf and bed files
- Wrapper script to generate
  - Snakemake report for pipeline run information
  - Snakemake archive for run documentation
  - initial internal sharing of pipeline run output

### Benchmark Set Validation

- Comparison of draft benchmarks to multiple external variant callsets
- Figure depicts benchmarking process and RIDE (Reliable Identification of Errors)
  principal from https://doi.org/10.1038/s41576-023-00590-0

### Q100 HG002 Variant Benchmark Sets

- Input assembly: T2T Q100 HG002, high-quality polished diploid assembly
  - T2T assembly of both haplotypes generated using PacBio HiFi and Ultra-long ONT data for initial assembly along with Hi-C and Strand-Seq for assembly graph resolution. Additional short read Illumina, Element, and Onso data used for polishing and refinement.
  - https://github.com/marbl/hg002

- Excluded Regions (v3.5 of GIAB stratifications)
  - Segmental Duplications not fully assembled in both haplotypes
  - Tandem Repeats greater than 10kb not fully assembled in both haplotypes
  - Satellites not fully assembled in both haplotypes
  - Gaps in reference assembly (Ns in GRCh37 and GRCh38, rDNA arrays CHM13)
  - Assembly flanking regions - 15kb around assembly breaks
  - VDJ - a region that undergoes somatic recombination
  - Structural Variants including overlapping repeat regions (small variant benchmark set only)
  - Discordant variant calls between dipcall and PAV
  - Inversions Identified by PAV
  - Variants in the HG002 mosaic benchmark set
  - Previously identified errors associated with dipcall's assembly based
    variant calls

_Benchmark Set Under Active Development_

Want to help with external evaluations of the small or structural variant benchmark? Let us know!

## References

### Publications

- Variant calling review: https://doi.org/10.1038/s41576-023-00590-0
- GA4GH small variant benchmarking best practices: Krusche et al. 2019 doi.org/10.1038/s41587-019-0054-x
- GIAB Stratifications: https://doi.org/10.1038/s41467-024-53260-y
- Mosaic Benchmark Poster: https://mdic.org/wp-content/uploads/2018/11/MDIC_AGBT-Poster_FINAL.pdf
- TandemRepeat Benchmark: https://doi.org/10.1038/s41587-024-02225-z

### Data

- [GIAB Stratifications v3.5](https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/release/genome-stratifications/v3.5/)
- [HG002 Draft Whole Genome Small and SV Benchmark](https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/data/AshkenazimTrio/analysis/NIST_HG002_DraftBenchmark_defrabbV0.018-20240716/), this is a work in progress check parent directory for new drafts.

## Other GIAB Projects

- Cancer Genome In A Bottle- new tumor/normal GIAB sample for future somatic variant benchmark set.
  - ASHG Poster 8009F
  - https://www.nist.gov/programs-projects/cancer-genome-bottle
- StratoMod
  - Interpretable machine learning model for predicting sequencing and variant calling errors
  - https://rdcu.be/dZmFP
- Genome Stratifications
  - Updated stratifications for use in interpreting benchmarking results, now have stratifications for GRCh37, GRCh38, CHM13, and HG002 Q100 maternal and paternal haplotype
  - https://rdcu.be/dZmF7
  - Stratifications: https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/release/genome-stratifications/v3.5/
- Mosaic Benchmark - characterization of low frequency SNVs in HG002
  - Developed in collaboration with Camille Daniels and Ade Abdulkadir bioinformatics team at MDIC (medical devices innovation consortium) as part of the [Somatic Reference Samples Initiative](https://mdic.org/project/cancer-genomic-somatic-reference-samples/)
  - Poster describing benchmark https://mdic.org/wp-content/uploads/2018/11/MDIC_AGBT-Poster_FINAL.pdf
- GIAB Transcriptome Pilot Study
  - Qualitative characterization of transcripts and isoforms
  - Large batch of RNA for HG002, HG003, HG004
  - Sequenced using multiple technologies: Illumina (cDNA and mRNA), PacBio HiFi (IsoSeq and Kinnex), ONT (cDNA and direct RNA.)
  