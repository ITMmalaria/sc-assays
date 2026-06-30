# WGS pipeline reproducibility and results

The [plasmovar](https://github.com/ITMmalaria/plasmovar) pipeline was run with the following command:

```bash
NXF_SYNTAX_PARSER=v1 nextflow run ITMMalaria/plasmovar -r 0.3.0 \
    -params-file params.yml \
    -profile singularity,vsc_calcua \
    -c custom.config \
    -ansi-log false \
    -work-dir ./work \
    -with-report report.html \
    -with-timeline timeline.html \
    -resume
```

The custom (non-default) parameters can be found in the [params yml file](./params.yml) and [config file](./custom.config) (for the `gatk GenotypeGVCFs` `--include-non-variant-sites` option, which was not part of the pipeline's parameters yet). The input files are listed in [samplesheet.csv](./samplesheet.csv).

> [!WARNING]
> Note that all hard-coded file paths would need to be adapted in order to re-run the pipeline.

The [MultiQC report](./multiqc_report.html) provides an overview of all processes' outputs.

## `bcftools` variant calls

Variant sites and per-sample genotypes reported by `bcftools view` and `query` are stored in the `bcftools-*.txt` files.

```bash
#!/bin/bash

# bcftools 1.23.1

bcftools query \
    -r Pf3D7_02_v3:631190,Pf3D7_02_v3:814288,Pf3D7_11_v3:1058035,Pf3D7_04_v3:1121472,Pf3D7_03_v3:140167,Pf3D7_03_v3:79845,Pf3D7_03_v3:96476,Pf3D7_02_v3:825357,Pf3D7_09_v3:1428875,Pf3D7_01_v3:557240 \
    -f 'CHROM\tPOS\tREF\tALT\tVQSLOD-FILTER\tQD (quality-by-depth)\tQUAL\tDP (depth)\n%CHROM\t%POS\t%REF\t%ALT\t%FILTER\t%QD\t%QUAL\t%DP\n\tsample\tGT\tGQ\tDP\tAD\n[\t%SAMPLE\t%GT\t%GQ\t%DP\t%AD\n]' \
    PlasmoDB-68_Pfalciparum3D7_Genome_merged.vcf.gz

bcftools view \
    -r Pf3D7_02_v3:631190,Pf3D7_02_v3:814288,Pf3D7_11_v3:1058035,Pf3D7_04_v3:1121472,Pf3D7_03_v3:140167,Pf3D7_03_v3:79845,Pf3D7_03_v3:96476,Pf3D7_02_v3:825357,Pf3D7_09_v3:1428875,Pf3D7_01_v3:557240 \
    PlasmoDB-68_Pfalciparum3D7_Genome_merged.vcf.gz
```
