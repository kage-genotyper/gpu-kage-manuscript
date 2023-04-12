---
title: Ultra-fast genotyping of SNPs and short indels using GPU acceleration
keywords:
- markdown
- publishing
- manubot
lang: en-US
date-meta: '2023-04-12'
author-meta:
- Jørgen Wictor Henriksen
- Knut Dagestad Rand
- Geir Kjetil Sandve
- Ivar Grytten
header-includes: |
  <!--
  Manubot generated metadata rendered from header-includes-template.html.
  Suggest improvements at https://github.com/manubot/manubot/blob/main/manubot/process/header-includes-template.html
  -->
  <meta name="dc.format" content="text/html" />
  <meta property="og:type" content="article" />
  <meta name="dc.title" content="Ultra-fast genotyping of SNPs and short indels using GPU acceleration" />
  <meta name="citation_title" content="Ultra-fast genotyping of SNPs and short indels using GPU acceleration" />
  <meta property="og:title" content="Ultra-fast genotyping of SNPs and short indels using GPU acceleration" />
  <meta property="twitter:title" content="Ultra-fast genotyping of SNPs and short indels using GPU acceleration" />
  <meta name="dc.date" content="2023-04-12" />
  <meta name="citation_publication_date" content="2023-04-12" />
  <meta property="article:published_time" content="2023-04-12" />
  <meta name="dc.modified" content="2023-04-12T15:25:03+00:00" />
  <meta property="article:modified_time" content="2023-04-12T15:25:03+00:00" />
  <meta name="dc.language" content="en-US" />
  <meta name="citation_language" content="en-US" />
  <meta name="dc.relation.ispartof" content="Manubot" />
  <meta name="dc.publisher" content="Manubot" />
  <meta name="citation_journal_title" content="Manubot" />
  <meta name="citation_technical_report_institution" content="Manubot" />
  <meta name="citation_author" content="Jørgen Wictor Henriksen" />
  <meta name="citation_author_institution" content="Biomedical Informatics research group, Department of Informatics, University of Oslo, Oslo, Norway" />
  <meta name="citation_author" content="Knut Dagestad Rand" />
  <meta name="citation_author_institution" content="Biomedical Informatics research group, Department of Informatics, University of Oslo, Oslo, Norway" />
  <meta name="citation_author_institution" content="Centre for Bioinformatics, University of Oslo, Oslo, Norway" />
  <meta name="citation_author" content="Geir Kjetil Sandve" />
  <meta name="citation_author_institution" content="Biomedical Informatics research group, Department of Informatics, University of Oslo, Oslo, Norway" />
  <meta name="citation_author_institution" content="Centre for Bioinformatics, University of Oslo, Oslo, Norway" />
  <meta name="citation_author_institution" content="UiORealArt Convergence Environment, University of Oslo, Oslo, Norway" />
  <meta name="citation_author" content="Ivar Grytten" />
  <meta name="citation_author_institution" content="Biomedical Informatics research group, Department of Informatics, University of Oslo, Oslo, Norway" />
  <link rel="canonical" href="https://kage-genotyper.github.io/gpu-kage-manuscript/" />
  <meta property="og:url" content="https://kage-genotyper.github.io/gpu-kage-manuscript/" />
  <meta property="twitter:url" content="https://kage-genotyper.github.io/gpu-kage-manuscript/" />
  <meta name="citation_fulltext_html_url" content="https://kage-genotyper.github.io/gpu-kage-manuscript/" />
  <meta name="citation_pdf_url" content="https://kage-genotyper.github.io/gpu-kage-manuscript/manuscript.pdf" />
  <link rel="alternate" type="application/pdf" href="https://kage-genotyper.github.io/gpu-kage-manuscript/manuscript.pdf" />
  <link rel="alternate" type="text/html" href="https://kage-genotyper.github.io/gpu-kage-manuscript/v/c4f361f2acde2ac627abf3bb221993647fb0d5d4/" />
  <meta name="manubot_html_url_versioned" content="https://kage-genotyper.github.io/gpu-kage-manuscript/v/c4f361f2acde2ac627abf3bb221993647fb0d5d4/" />
  <meta name="manubot_pdf_url_versioned" content="https://kage-genotyper.github.io/gpu-kage-manuscript/v/c4f361f2acde2ac627abf3bb221993647fb0d5d4/manuscript.pdf" />
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

+ **Knut Dagestad Rand**
  <br>
  <small>
     Biomedical Informatics research group, Department of Informatics, University of Oslo, Oslo, Norway; Centre for Bioinformatics, University of Oslo, Oslo, Norway
  </small>

+ **Geir Kjetil Sandve**
  <br>
  <small>
     Biomedical Informatics research group, Department of Informatics, University of Oslo, Oslo, Norway; Centre for Bioinformatics, University of Oslo, Oslo, Norway; UiORealArt Convergence Environment, University of Oslo, Oslo, Norway
  </small>

+ **Ivar Grytten**
  ^[✉](#correspondence)^<br>
  <small>
     Biomedical Informatics research group, Department of Informatics, University of Oslo, Oslo, Norway
  </small>


::: {#correspondence}
✉ — Correspondence to 
Ivar Grytten \<ivargry@ifi.uio.no\>.

:::




## Abstract
As sequencing costs have steadily decreased in recent years, having computationally efficient methods for variant discovery and genotyping has become increasingly important. Recent work have shown that genotyping can be done efficiently and accurately using *alignment-free* methods that are based on analyzing kmers from sequenced reads. In particular, we have recently presented the KAGE genotyper, which uses an efficient pangenome representation of known individuals in a population to further increase accuracy and efficiency. While existing genotypers like KAGE use the *Central Processing Unit* (CPU) to count and analyze kmers, the *Graphical Processing Unit* (GPU)  has shown to be promising for reducing runtime for similar problems.

We here present *GKAGE*, a new and improved version of KAGE that utilizes the GPU to further increase the computational efficiency. This is done by counting and analyzing large amounts of kmers in the many parallel cores of a GPU. We show that GKAGE is, on comparable cost hardware, able to genotype an individual up to a magnitude faster than KAGE while producing the same output, which makes it by far the fastest genotyper available today. GKAGE can run on consumer-grade GPUs, and enables genotyping of a human sample in only a matter of minutes without the need for expensive high-performance computers. GKAGE is open source and available at <https://github.com/kage-genotyper/kage>.

## Introduction
The cost of sequencing a full human genome has fallen drastically in recent years, and consumers can now get their whole genome sequenced for a few hundred dollars [@crow], a fraction of what was the price only a few years ago. As millions of genomes are likely to be sequenced in the coming years, there is an ever-increasing need for efficient methods for analyzing this genomic data. At the core of such analysis is *variant detection*, determining which genetic variants are present in a sample based on the sequenced reads.

Recent methods [@kage; @malva; @pangenie; @graphtyper] have shown that detecting variants in a human sample can be performed efficiently and with high accuracy by *genotyping* the sample against an existing database of known human variation. Such methods use prior knowledge from e.g. the 1000 Genomes Project [@thousand_genomes] about where in the genome individuals frequently have variation, and for each such genetic variant uses the genomic reads from the sample to infer the most likely genotype. While such methods traditionally have been based on aligning reads to a reference genome, which is slow, recent *alignment-free methods* have shown that drastic speedup can be achieved by instead analyzing kmers from the sequenced reads. In a recent publication [@kage], we proposed a highly efficient alignment-free method, KAGE, that uses prior knowledge from a population to achieve high genotyping accuracy while being more efficient than other alignment-free genotypers. Alignment-free genotypers, like KAGE, generally rely on analyzing kmers from reads against kmers associated with genomic variants. Prior to genotyping, kmers that represent alleles of variants of interest are collected and stored in an index (e.g. a hashmap) that enables lookup of kmers from reads to variant alleles. This is typically done once for a set of variants, and this index can then be reused for genotyping any individual against those variants. When genotyping an individual, kmers from reads are collected and looked up in the kmer index, to obtain kmer counts for each variant allele. Generally, genotype probabilities are then found by analyzing the counts of how many reads support the various alleles of the variants of interest. In KAGE, these counts are compared against expected counts sampled from a population to obtain better estimates.


In contrast to other genotypers, especially alignment-based genotypers like GATK [@gatk], the time-consuming steps of KAGE (like processing and counting kmers) can benefit from the massive parallelization that the Graphical Processing Unit (GPU) offers [@gerbil, @kmc2]. Here, we show that by applying GPU acceleration to the compute-heavy parts of KAGE, we end up with a very efficient genotyper, GKAGE. GKAGE uses the GPU to process genomic reads, extract kmers and count how many kmers support alleles of genetic variants. We show that GKAGE gives significant speedup (up to 10X) over the original KAGE genotyper (which was already faster than any other genotyper), while producing the exact same output. GKAGE has been implemented so that it is able to run even on standard consumer GPUs, and is able to genotype a whole human sample in a matter of minutes.

## Results
GKAGE is a GPU accelerated version of the recently published KAGE genotyper. GKAGE uses CUDA-powered GPUs to efficiently parse and encode kmers from reads, and genotype a set of known SNPs and indels based on the kmer counts. GKAGE produces output that is identical to that of KAGE, with reduced runtime on systems that support the CUDA-interface for GPU acceleration. The software is open source and available at <https://github.com/kage-genotyper/kage>. As part of GKAGE, we have also implemented a static GPU hashmap for counting kmers through a Python interface, available at <https://github.com/kage-genotyper/cucounter>.

We have recently shown that KAGE is an order of magnitude faster than existing genotypers while giving better or comparable accuracy [@kage]. We thus only benchmark GKAGE against KAGE to show the effect of GPU acceleration. We do this by running GKAGE and KAGE on a human whole genome sample (15x coverage) on two different systems:

1. A high-performance server with an AMD EPYC 7742 64-Core CPU and two NVIDIA Tesla V100 GPUs. KAGE was run using 16 threads and GKAGE was run using one GPU.

2. A regular desktop computer with an 11th Gen Intel(R) Core(TM) i5-11400F @ 2.60GHz CPU with 6 cores and a NVIDIA GTX 1660 super GPU. KAGE was run using 6 threads.

Table 1 shows the runtimes on these two systems. GKAGE is approximately 5x faster on the high-end system and more than 10x faster on the desktop computer. Since GKAGE only needs the kmer counts of a predefined set of kmers (those associated with variants), and no existing GPU-based kmer counter is able to count only a given set of kmers, we have implemented our own kmer counter as part of GKAGE. An alternative solution would be to count all kmers using an existing tool and filter out those kmers that are relevant. Table 1 also shows the time spent by the GPU kmer counter Gerbil [@gerbil] to only count kmers.


|                           | KAGE     | GKAGE   | Gerbil (only kmer counting) |
|---------------------------|----------|---------|-----------------------------|
| Desktop computer          | 1993 sec | 178 sec | 464 sec                     |
| High-performance computer | 510 sec  | 94 sec  | 438 sec                     |
Table: Running times of KAGE, GKAGE and Gerbil (only kmer counting)
{#tbl:table1}



## Methods

### Implementation
Here we describe more in detail how GKAGE has been implemented. While GKAGE is implemented as part of KAGE, and in large parts uses the same code, compute-heavy parts have been reimplemented so that the GPU is utilized. GKAGE implements GPU support for two bottleneck components of KAGE that were suitable for GPU acceleration:

**Reading and encoding kmers** from a FASTA/FASTQ file is achieved in KAGE by using BioNumPy [@bionumpy], a Python library built on top of NumPy [@numpy]. BioNumPy uses NumPy to efficiently read chunks of DNA reads from fasta files, encode the bases to a 2-bit representation, and then encode the valid kmers as 64-bit integer representations in an array. GPU support for this step was achieved by utilizing CuPy [@cupy], a GPU accelerated computing library with an interface that closely follows that of NumPy. This was implemented by replacing the NumPy module in BioNumPy with CuPy, effectively replacing all NumPy function calls with calls to CuPy's functions providing the same functionality, although GPU accelerated. This strategy worked out of the box for most parts of the BioNumPy solution, with only a few modifications having to be made due to certain functions in NumPy's interface not being supported by CuPy. 


**Counting kmers** As part of GKAGE, we have implemented a static hashtable for counting a predefined set of kmers on the GPU. The implementation supports parallel and high-throughput hashing and counting of large chunks of kmers simultaneously on the GPU.
This static hashtable is implemented as a C++ class in CUDA [@cuda], with two arrays of 64-bit and 32-bit unsigned integers to represent kmers (keys) and counts (values) respectively. CUDA kernels are implemented that handle insertion (only once during initialization of the hashtable), counting and querying of kmers. The hashtable uses open addressing and a simple linear probing scheme with a murmur hash [@murmur] for the keys.

To find a kmer $k$'s position in the hashtable, the initial probe position $p_0$ is found by computing

$$
  p_0=hash(k) \bmod c
$$


where $hash$ is a murmur hash function and $c$ is the capacity of the hashtable.
If $p_0$ is occupied by a different kmer than $k$, the next probing position $p_i$ can be computed given the previous probing position $p_{i-1}$ with

$$
  p_i=(p_{i-1} + 1) \bmod c
$$

The probing will continue until either $k$ or an empty slot in the hashtable is observed (See Figure @fig:figure1 for an illustration of this).

The hashtable supports three main operations: insertion, counting and querying.
In each of the cases, the input is an array of 2-bit encoded kmers, and for querying the return value is an array of counts associated with the input kmers. For insertion, counting or querying of $n$ kmers, $n$ CUDA threads are launched. Each thread is then responsible for fulfilling the relevant operation associated with the kmer, i.e. incrementing or fetching the count associated with the kmer in the hashtable, all achieved using the probing scheme previously described. Furthermore, for insertions and count updates, CUDA atomic operations are used to avoid race conditions. To use the hashtable class in Python, C++ bindings are implemented using pybind11 [@pybind11]. 

Since KAGE only needs to count kmers that are preselected to represent alleles of known variants, which typically is only a small subset of the kmers present in genomic reads, the hashmap needed for this requires only a few gigabytes of memory. Thus, when genotyping 28 million variants of a human sample, a GPU with 4GB of memory is sufficient (see details below).

![As an array of 64-bit integer encoded kmers are counted by the hash table, each CUDA thread will compute the first probe position $p_0$ for each individual kmer, and then continue probing by linearly moving up to the next consecutive slot until either an empty slot or the original kmer handled by the thread is observed. If an empty slot is observed, the thread terminates. If the original kmer is observed, the value at the current slot is increased.](content/images/hashtable_counting.png){#fig:figure1 width=6in}

#### Memory Usage
The memory usage for GKAGE is influenced by two factors: The size of the kmer index, and the number of sequences to handle at the time (chunk size). Since the chunk-size can be set to a suitable number for a given system (we used 10 MB in our experiments), the main factor will be the size of the kmer-index.

The size of the kmer-index itself is again influenced by three factors: The number of variants to be genotyped ($N_v$), the average number of kmers per variant ($k$), and the loading factor of the hash table ($L$). Given these, the memory will be in the order of $O(N_v *k / L)$. Thus, for a fixed loading factor (default 0.57), the memory consumption of GKAGE is primarily driven by the number of variants to be genotyped. For systems with little memory, the loading factor can be increased to remove the memory consumption, but that can severely damage performance when the loading factor approaches 1. For genotyping the 28 million variants in the experiment in table 1, with $3.4$ kmers per variant, and a loading factor of $0.57$, the memory consumption of the index is $(28*10^6*3.4/0.57)*(8 + 4) B = 2GB$ (using 8 bytes for keys and 4 bytes for counts).

### Benchmarks
Benchmarking was performed on two different computer systems, as described in the Results section, using simulated reads for a whole genome sample with 15x coverage. A Snakemake [@snakemake] pipeline for reproducing the benchmarking results can be found at <https://github.com/kage-genotyper/GKAGE-benchmarking>.

## Discussion
We have presented GKAGE, a GPU accelerated genotyper. Our results show that alignment-free genotyping is an ideal problem for GPU acceleration. While the existing KAGE genotyper is already fast by today’s standards, GKAGE is considerably faster, and enables rapid genotyping on even consumer computers. We believe these results are relevant as the experimental side of whole-genome sequencing is continuously becoming cheaper and more common.

Since the original KAGE genotyper was implemented mainly using the array programming libraries NumPy and BioNumPy in Python, adding GPU-support to the existing code base was possible to do in a clean way by using the CuPy library as well as implementing some custom CUDA kernels with Python wrappers. We thus believe that this work shows that adding GPU-support to existing tools is feasible and can be beneficial in cases where many independent operations are performed on an array of data, which is common for computational biology problems. As GPUs are steadily becoming cheaper and more available, we thus believe there might be a huge potential in improving the performance of existing tools and methods, which in many cases can be done quite easily through the Python ecosystem with packages such as CuPy [@cupy], Numba [@numba] and BioNumPy [@bionumpy].



[@malva]: doi:10.1016/j.isci.2019.07.011
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
[@pangenie]: doi:10.1038/s41588-022-01043-w
[@crow]: doi:10.1016/j.cell.2019.02.041
[@gatk]: doi:10.1101/201178
[@snakemake]: doi:10.12688/f1000research.29032.2



## References

<!-- Explicitly insert bibliography here -->
<div id="refs"></div>

