---
title: Ultra-fast genotyping of SNPs and short indels using GPU-acceleration
keywords:
- markdown
- publishing
- manubot
lang: en-US
date-meta: '2023-01-29'
author-meta:
- Jørgen Wictor Henriksen
- Ivar Grytten
- Knut Dagestad Rand
- Geir Kjetil Sandve
header-includes: |
  <!--
  Manubot generated metadata rendered from header-includes-template.html.
  Suggest improvements at https://github.com/manubot/manubot/blob/main/manubot/process/header-includes-template.html
  -->
  <meta name="dc.format" content="text/html" />
  <meta property="og:type" content="article" />
  <meta name="dc.title" content="Ultra-fast genotyping of SNPs and short indels using GPU-acceleration" />
  <meta name="citation_title" content="Ultra-fast genotyping of SNPs and short indels using GPU-acceleration" />
  <meta property="og:title" content="Ultra-fast genotyping of SNPs and short indels using GPU-acceleration" />
  <meta property="twitter:title" content="Ultra-fast genotyping of SNPs and short indels using GPU-acceleration" />
  <meta name="dc.date" content="2023-01-29" />
  <meta name="citation_publication_date" content="2023-01-29" />
  <meta property="article:published_time" content="2023-01-29" />
  <meta name="dc.modified" content="2023-01-29T21:47:21+00:00" />
  <meta property="article:modified_time" content="2023-01-29T21:47:21+00:00" />
  <meta name="dc.language" content="en-US" />
  <meta name="citation_language" content="en-US" />
  <meta name="dc.relation.ispartof" content="Manubot" />
  <meta name="dc.publisher" content="Manubot" />
  <meta name="citation_journal_title" content="Manubot" />
  <meta name="citation_technical_report_institution" content="Manubot" />
  <meta name="citation_author" content="Jørgen Wictor Henriksen" />
  <meta name="citation_author_institution" content="Biomedical Informatics research group, Department of Informatics, University of Oslo, Oslo, Norway" />
  <meta name="citation_author" content="Ivar Grytten" />
  <meta name="citation_author_institution" content="Biomedical Informatics research group, Department of Informatics, University of Oslo, Oslo, Norway" />
  <meta name="citation_author" content="Knut Dagestad Rand" />
  <meta name="citation_author_institution" content="Biomedical Informatics research group, Department of Informatics, University of Oslo, Oslo, Norway" />
  <meta name="citation_author_institution" content="Centre for Bioinformatics, University of Oslo, Oslo, Norway" />
  <meta name="citation_author" content="Geir Kjetil Sandve" />
  <meta name="citation_author_institution" content="Biomedical Informatics research group, Department of Informatics, University of Oslo, Oslo, Norway" />
  <meta name="citation_author_institution" content="Centre for Bioinformatics, University of Oslo, Oslo, Norway" />
  <meta name="citation_author_institution" content="UiORealArt Convergence Environment, University of Oslo, Oslo, Norway" />
  <link rel="canonical" href="https://kage-genotyper.github.io/gpu-kage-manuscript/" />
  <meta property="og:url" content="https://kage-genotyper.github.io/gpu-kage-manuscript/" />
  <meta property="twitter:url" content="https://kage-genotyper.github.io/gpu-kage-manuscript/" />
  <meta name="citation_fulltext_html_url" content="https://kage-genotyper.github.io/gpu-kage-manuscript/" />
  <meta name="citation_pdf_url" content="https://kage-genotyper.github.io/gpu-kage-manuscript/manuscript.pdf" />
  <link rel="alternate" type="application/pdf" href="https://kage-genotyper.github.io/gpu-kage-manuscript/manuscript.pdf" />
  <link rel="alternate" type="text/html" href="https://kage-genotyper.github.io/gpu-kage-manuscript/v/253eccef06a2d0accace69df4fe20207c9e86a7e/" />
  <meta name="manubot_html_url_versioned" content="https://kage-genotyper.github.io/gpu-kage-manuscript/v/253eccef06a2d0accace69df4fe20207c9e86a7e/" />
  <meta name="manubot_pdf_url_versioned" content="https://kage-genotyper.github.io/gpu-kage-manuscript/v/253eccef06a2d0accace69df4fe20207c9e86a7e/manuscript.pdf" />
  <meta property="og:type" content="article" />
  <meta property="twitter:card" content="summary_large_image" />
  <link rel="icon" type="image/png" sizes="192x192" href="https://manubot.org/favicon-192x192.png" />
  <link rel="mask-icon" href="https://manubot.org/safari-pinned-tab.svg" color="#ad1457" />
  <meta name="theme-color" content="#ad1457" />
  <!-- end Manubot generated metadata -->
bibliography:
- content/manual-references.json
manubot-output-bibliography: output/references.json
manubot-output-citekeys: output/citations.tsv
manubot-requests-cache-path: ci/cache/requests-cache
manubot-clear-requests-cache: false
...








## Authors



+ **Jørgen Wictor Henriksen**
  <br>
  <small>
     Biomedical Informatics research group, Department of Informatics, University of Oslo, Oslo, Norway
  </small>

+ **Ivar Grytten**
  <br>
  <small>
     Biomedical Informatics research group, Department of Informatics, University of Oslo, Oslo, Norway
  </small>

+ **Knut Dagestad Rand**
  <br>
  <small>
     Biomedical Informatics research group, Department of Informatics, University of Oslo, Oslo, Norway; Centre for Bioinformatics, University of Oslo, Oslo, Norway
  </small>

+ **Geir Kjetil Sandve**
  ^[✉](#correspondence)^<br>
  <small>
     Biomedical Informatics research group, Department of Informatics, University of Oslo, Oslo, Norway; Centre for Bioinformatics, University of Oslo, Oslo, Norway; UiORealArt Convergence Environment, University of Oslo, Oslo, Norway
  </small>


::: {#correspondence}
✉ — Correspondence possible via [GitHub Issues](https://github.com/kage-genotyper/gpu-kage-manuscript/issues)
or email to
Geir Kjetil Sandve \<geirksa@ifi.uio.no\>.


:::




## Abstract
As sequencing costs have steadily decreased in recent years, having efficient methods for variant discovery and genotyping has become increasingly important. Recent methods have shown that genotyping can be done efficiently and accurately using *alignment-free* methods that are based on analysing kmers from sequenced reads. In recent work, we have presented the KAGE genotyper, which uses an efficient pangenome representation of known individuals in a population to further increase accuracy and efficiency.

We here present *GKAGE*, a new and improved version of KAGE that utilises the *Graphical Processing Unit* (GPU) to further increase the efficiency by counting and analysing large amounts of kmers in parallel. We show that GKAGE is able to genotype an individual up to a magnitude faster than KAGE while producing the same output, which makes it by far the fastest genotyper available today. GKAGE can run on consumer-grade GPUs, and enables genotyping of a human sample in only a matter of minutes without the need for expensive high-performance computers. GKAGE is open source and available at <https://github.com/ivargr/kage>.

## Introduction
The cost of sequencing a full human genome has fallen drastically in recent years, and consumers can now get their whole genome sequenced for a few hundred dollars, a fraction of what was the price only a few years ago. As millions of genomes are likely to be sequenced in the coming years, there is an ever-increasing need for efficient methods for analysing this genomic data. At the core of such analysis is *variant detection*, determining which genetic variants a sample has based on the sequenced reads.

Recent methods [@kage; @malva; @graphtyper] have shown that detecting variants in a human sample can be performed efficiently and with high accuracy by *genotyping* the sample against an existing database of known human variation. Such methods use prior knowledge from e.g. the 1000 Genomes Project [@thousand_genomes] about where in the genome individuals may have variation, and for each such genetic variant uses the genomic reads from the sample to infer the most likely genotype for the given sample. While such methods traditionally have been based on aligning reads to a reference genome, which is slow, recent *alignment-free methods* have shown that drastic speedup can be achieved by instead analysing kmers from the sequenced reads. In a recent publication [@kage] we proposed a highly efficient alignment-free method, KAGE, that used prior knowledge from a population to achieve high genotyping accuracy while being more efficient than other alignment-free genotypers. 

Alignment-free genotypers, like KAGE, generally work by using an index (e.g. a hashmap) that enables “mapping” of kmers from input reads to alleles of known genetic variants. The genotyping is then performed by counting how many reads support the various alleles of the variants of interest. Since all kmers from the input reads are counted independently, this process can easily be parallelized. While existing alignment-free genotypers uses the central processing unit (CPU) for parallel kmers counting, we hypothesise that this can be done much more efficiently by instead using the *Graphical Processing Unit (GPU)*, which is able to perform massive amounts of computations in parallel. Recent work has also shown that the GPU can greatly improve the efficiency of kmer counting in general [@gerbil, @kmc2].

We here present GKAGE, a GPU-accelerated version of KAGE, which to our knowledge is the first genotyper to utilise the GPU. GKAGE uses the GPU to process genomic reads, extract kmers and count how many kmers support alleles of genetic variants. We show that GKAGE gives significant speedup (up to 8X) over the original KAGE genotyper while producing the exact same output. GKAGE has been implemented so that it is able to run even on standard consumer GPUs, and is able to genotype a whole human sample in a matter of minutes.

## Results
GKAGE is a GPU-accelerated version of the recently published KAGE genotyper. GKAGE uses CUDA-powered GPUs to efficiently parse and encode kmers from reads and genotype a set of known SNPs and indels based on the kmer counts. GKAGE produces identical output as KAGE with reduced runtime on systems that enable GPU-acceleration and support the CUDA-interface. The software is open source and available at <https://github.com/ivargr/kage>. As part of GKAGE, we have also implemented a static GPU hashmap for counting kmers through a Python interface, available at <https://github.com/jorgenwh/cucounter>.

We have recently shown that KAGE is an order of magnitude faster than existing genotypers while giving better or comparable accuracy [@kage]. We thus only benchmark GKAGE against KAGE to show the effect of GPU-acceleration. We do this by running GKAGE and KAGE on a human whole genome sample (15x coverage) on two different systems:

1. A high-end server with an AMD EPYC 7742 64-Core CPU and two NVIDIA Tesla V100 GPUs. KAGE was run using 16 cores and GKAGE was run using one GPU.

2. A regular desktop computer with an 11th Gen Intel(R) Core(TM) i5-11400F @ 2.60GHz CPU and a NVIDIA GTX 1660 super GPU. KAGE was run using 6 cores.

Table 1 shows the runtimes on these two systems. GKAGE is approximately 5x faster on the high-end system and 8x faster on the desktop computer.


|                           | KAGE     | GKAGE   |
|---------------------------|----------|---------|
| Desktop computer          | 1880 sec | 180 sec |
| High-performance computer | 510 sec  | 108 sec |
Table: Running times of KAGE and GKAGE
{#tbl:table1}





## Methods

### Implementation
Here we describe more in detail how GKAGE has been implemented. While GKAGE is implemented as part of KAGE, and in large parts uses the same code, compute-heavy parts have been reimplemented so that the GPU is utilised. GKAGE mainly implements GPU support for two bottleneck components of KAGE that were suitable for GPU-acceleration:

**Reading and encoding kmers** from a FASTA/FASTQ file is achieved in KAGE by using BioNumPy [@bionumpy], a Python library built on top of NumPy [@numpy]. BioNumPy uses NumPy to efficiently read chunks of DNA reads from fasta files, encode the bases to a 2-bit representation, and then encode the valid kmers as 64-bit integer representations in an array. GPU support for this step was achieved by utilising CuPy [@cupy], a GPU accelerated alternative to NumPy, as a drop-in replacement for NumPy.

**Counting kmers**
As part of GKAGE, we have implemented a static hashmap for counting a predefined set of kmers on the GPU. The implementation supports parallel and high-throughput hashing and counting of large chunks of kmers simultaneously on the GPU. 

Genotyping based on kmer counts is performed using the existing KAGE algorithm, which is already very fast and did not benefit from GPU-acceleration.

### Benchmarks
A Snakemake pipeline for reproducing the benchmarking results can be found at https://….


## Discussion
We have presented a GPU-accelerated genotyper by enabling GPU-support for the most compute-intensive operations of the existing KAGE genotyper. Our results show that alignment-free genotyping is an ideal problem for GPU-acceleration. While the existing KAGE genotyper already is fast by today’s standards, GKAGE is considerably faster, and makes it even easier to run whole-genome genotyping on a typical consumer computer. We believe the results are relevant as whole-genome sequencing is continuously becoming cheaper and more common.

Since the original KAGE genotyper was implemented mainly using the array programming libraries NumPy and BioNumPy in Python, adding GPU-support to the existing code base was possible to do in a clean way by using the CuPy library as well as implementing some custom CUDA kernels with Python wrappers. We thus believe that this work shows that adding GPU-support to existing tools is feasible and can be beneficial in cases where many independent operations are performed on an array of data. As GPUs are becoming cheaper and more available, we thus believe there might be a huge potential in improving the performance of existing tools and methods, which in many cases can be done quite easily through the Python ecosystem with packages such as CuPy [@cupy], Numba [@numba] and BioNumPy [@bionumpy].





[@malva]: doi:10.1016/j.isci.2019.07.011
[@kage]: doi:10.1186/s13059-022-02771-2
[@jellyfish]: doi:10.1093/bioinformatics/btr011 
[@numpy]: doi:10.1038/s41586-020-2649-2
[@thousand_genomes]: doi:10.1038/nature15393
[@graphtyper]: doi:10.1038/ng.3964
[@gerbil]: doi:10.1186/s13015-017-0097-9
[@kmc2]: doi:10.1109/ASAP.2018.8445084
[@bionumpy]: doi:10.1101/2022.12.21.521373
[@numba]: doi:10.1145/2833157.2833162



## References

<!-- Explicitly insert bibliography here -->
<div id="refs"></div>

