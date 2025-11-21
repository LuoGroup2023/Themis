# Themis

**Themis** : a robust and accurate species-level metagenomic profiler.

<!-- [![BioConda Install](https://img.shields.io/conda/dn/bioconda/themis.svg?style=flag&label=BioConda%20install)](https://anaconda.org/bioconda/themis)
[![Anaconda-Server Badge](https://anaconda.org/bioconda/themis/badges/version.svg)](https://anaconda.org/bioconda/themis)
[![License](https://img.shields.io/github/license/xujialupaoli/Themis)](https://www.gnu.org/licenses/gpl-3.0.en.html) -->


## Table of Contents

- [Overview](#overview)
- [Installation](#installation)
- [Features](#Features)
- [Quick start](#quick-start)
  - [1. Build custom reference databases](#1-build-custom-reference-database)
  - [2. Profile reads against custom databases](#2-profile)
<!-- - [Input files](#input-files)
- [Output files](#output-files)
- [Command reference](#command-reference)
- [Tips and known issues](#tips-and-known-issues)
- [Citation](#citation)
- [License](#license)
- [Contact](#contact) -->

---
## Overview
Themis is a fast and robust metagenomic profiler that achieves high accuracy across ultra-low to high sequencing depths. Themis combines a rapid, high-recall pre-screening step with graph-based refinement using colored de Bruijn graphs, reducing classification ambiguity and improving scalability to large reference databases.

## Installation 


```
conda install -c bioconda -c conda-forge themis
## Run themis.
themis -h
```
## Features

- **Commands**
  - `themis build-custom` Build custom themis databases.
  - `themis profile` Profile reads against custom databases.


## Quick start
* **1-build-custom-reference-database** 
```
themis build-custom  --input-file input_genomes.txt --taxonomy-files nodes.dmp names.dmp --db-prefix themisDB --level strain -t $threads -k 19
```
input_genomes.txt is a headerless, tab-separated manifest where each line contains (1) the absolute path to a genome FASTA file, (2) its strain\_name, and (3) the corresponding strain-level NCBI taxid.

Due to the large size of the reference pangenome we used for testing, we provide the `genomes_info.txt` used here. You need to download these genomes from NCBI RefSeq and update the actual paths in `genomes_info.txt`. Please note that NCBI RefSeq periodically updates their database, so we cannot guarantee that all the listed genomes will be available. Building the reference pangenome takes approximately one week with this `genomes_info.txt`. 

* **2-profile**

```
# short read(pair-end)
themis -r read1.fq -r $read2.fq --db-prefix themisDB --ref-info genomes_info.txt --out themis_query --threads 64 -k 31
# long read
themis -r $reads.fq --single --db-prefix themisDB --ref-info genomes_info.txt --out themis_query --threads 64 -k 31
```
genomes_info.txt is a tab-separated metadata table with a header line. The columns are, in order: strain_name, strain_taxid, species_taxid, species_name, and genome_path, where strain_name and strain_taxid must be unique and genome_path gives the absolute path to the corresponding genome FASTA file.





