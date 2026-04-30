---
title: About fanta.bio
type: about
---

[<span style="font-size: 2em">fanta.bio</span>](https://fanta.bio) (Functional genome ANnotations with Transcriptional Activities) is a database that collects functional annotations of genomes for studying gene regulation, with a primary focus on cis-regulatory elements (CREs) such as promoters and enhancers.

Identification of CREs in **fanta.bio** is based on transcription signatures. Both promoters and enhancers produce specific sets of RNAs, including mRNA, lncRNA, uaRNA (upstream antisense RNA), and eRNA (enhancer RNA). These transcription signatures are effectively used in the course of the identification. The pioneering work of transcriptome-based CRE identification was carried out in [the FANTOM5 project](https://fantom.gsc.riken.jp/5/), and we here applied an advanced approach to an expanded dataset. We additionally collect relevant resources, such as genome binding sites of transcription factors (“trans” factors) and genome variations across individuals.


## Interfaces

We provide the following interfaces to access the data:

- In-house CRE annotation viewer: The most convenient way to explore the database is through our web interface (https://fanta.bio/)
- Genome Browser: For researchers who prefer visualizing the data via a genomic view, we provide access to the data through [the UCSC Genome Browser database using track hub](https://genome-asia.ucsc.edu/cgi-bin/hgTracks?hubUrl=[https://data.fanta.bio/trackhub/hub.txt](https://data.fanta.bio/hub/v1.1.0-2409/trackhub/hub.txt)), enabling comparison with other genomic datasets.
- Download Archive: For advanced users, we offer download archives (https://data.fanta.bio/) for working with the data locally.


## How the CREs are identified?

CRE peaks are identified based on the experimental measures of transcription starting sites in a broad range of samples.
CAGE (Cap Analysis of Gene Expression) is a method to sequence capped RNA 5'-ends, and publicly available data produced by CAGE are utilized in our pipeline for identification, including the ones produced by [FANTOM5](https://fantom.gsc.riken.jp/5/), [FANTOM6](https://fantom.gsc.riken.jp/6/), and others deposited in [SRA](https://www.ncbi.nlm.nih.gov/sra)/[ENA](https://www.ebi.ac.uk/ena)/[DRA](https://www.ddbj.nig.ac.jp/dra). Our pipeline utilizes a newly developed method based on transcription divergence (Kawaji et al. in prep.).

The CREs peaks are categorized into two groups: promoter level activity (PLA) and enhancer level activity (ELA), based on the levels of transcription (that is, CRE activity). Genomic coordinates of the CREs are provided in [BED9+ format](https://genome.ucsc.edu/FAQ/FAQformat.html#format1), where thickStart/thickEnd represent the core region of the CRE peak bounded by the highest signals in forward and reverse strands.

<p>
In the genome browser view, the color of CRE peaks with PLA ranges from red to blue, indicating the direction of transcription. Red is used for the forward direction (+1), blue for the reverse direction (-1), and intermediate colors for directionalities in-between, where directionality is defined as <!-- $ \frac{ (ForwardCounts) - (ReverseCounts) }{ (ForwardCounts) + (ReverseCounts) }$ -->
<math display="inline" class="text-md italic">
  <mfrac>
    <mrow>
      <mi>(ForwardCounts)</mi>
      <mo>−</mo>
      <mi>(ReverseCounts)</mi>
    </mrow>
    <mrow>
      <mi>(ForwardCounts)</mi>
      <mo>+</mo>
      <mi>(ReverseCounts)</mi>
    </mrow>
  </mfrac>
</math>. The CRE peaks with ELA are indicated by yellow.
</p>

## How the CRE activities are measured?

Cell-dependent gene regulation requires activation of a specific set of CREs, and we quantify the CRE activities by measuring their transcription outputs per cell or tissue type. RNA 5'-ends obtained by a dedicated protocol (e.g. CAGE) within each of the the CRE regions are counted, normalized as CPM (counts per million) to adjust sequence depth, and scaled by the RLE method ([Anders et al. 2013](https://doi.org/10.1038/nprot.2013.099)) to make sample-wise comparison sensible.


## Which ChIP-seq data is used?

Of the dataset provided by [ChIP-Atlas](https://chip-atlas.org/), ChIP-seq peaks in the "TF and Others" categories is obtained. A parf of the data derived from the cell lines matching to the transcriptome data are included. For their entire data set (incl. ATAC-seq and Bisulfite-seq) and the data-mining tools, please visit their web site (https://chip-atlas.org/).

Additional ChIP-seq peaks can be examined via the UCSC Genome Browser, for example ReMap (https://remap.univ-amu.fr/).


## Which genome variation data in human is used?

[TogoVar](https://togovar.org/) is used as the data source of human genome variation. They focus on Japanese genetic variations but also included non-Japanese ones via dbSNP (https://www.ncbi.nlm.nih.gov/snp/) and others.

Additional variations can be examined via the UCSC Genome Browser, for example ClinVar (https://www.ncbi.nlm.nih.gov/clinvar/),
gnomAD (https://gnomad.broadinstitute.org/), TCGA Pan-cancer mutations through the [Genomic Data Commons Portal](https://portal.gdc.cancer.gov/), and dbSNP (https://www.ncbi.nlm.nih.gov/snp/).


## Which genome variation data in mouse is used?

[MoG+](https://molossinus.brc.riken.jp/) is used as the data source of mouse genome variation. They collected variations across mouse subspecies or strains.

Additional variations can be examined via the UCSC Genome Browser, for example genomic variations of the common laboratory mouse strains from Mouse Genomes Project (https://www.sanger.ac.uk/data/mouse-genomes-project/), and dbSNP (https://www.ncbi.nlm.nih.gov/snp/).


## Partnership

**fanta.bio** is affiliated with [INTRARED](https://www.intrared.org/), serving as a member database within the network.


## Cite us

All data produced by **fanta.bio** is distributed under the [CC-BY 4.0 license](http://creativecommons.org/licenses/by/4.0/). When you use the data and/or the website, please cite the following:

<blockquote style="font-style: normal;">
Nobusada T <em>et al.</em>, Update of the FANTOM web resource: enhancement for studying noncoding genomes. <em>Nucleic Acids Res.</em> <strong>53</strong> (D1), D419–D424 (2025). doi: <a href="https://doi.org/10.1093/nar/gkae1047">10.1093/nar/gkae1047</a>.
</blockquote>


## Contact us

If you have any questions or need assistance, please feel free to contact us at *help [at] fanta.bio*.


## The teams

The construction and maintenance of **fanta.bio** is a collaborative effort of the following three labs:

- [Laboratory for Large-Scale Biomedical Data Technology](https://www.ims.riken.jp/labo/64/) at [RIKEN IMS](https://www.ims.riken.jp/english/) (led by Dr. Kasukawa)
- [Integrated Bioresource Information Division](https://www.riken.jp/en/research/labs/brc/integr_bioresour_inf_div/) at [RIKEN BRC](https://web.brc.riken.jp/en/) (led by Dr. Masuya)
- [Research Center for Genome & Medical Sciences](https://www.igakuken.or.jp/genome-center/) at [TMiMS](https://www.igakuken.or.jp/english/) (led by Dr. Kawaji)


## Acknowledgements

We sincerely appreciate the following resources for sharing their data:

- [ChIP-Atlas](https://chip-atlas.org/) a data-mining suite for exploring epigenomic landscapes
- [MoG+](https://molossinus.brc.riken.jp/) a database of genomic variations across mouse subspecies for biomedical research
- [TogoVar](https://togovar.org/) a comprehensive Japanese genetic variation database

We are also grateful to [the UCSC Genome browser](https://genome.ucsc.edu/) and [its Asian mirror](https://genome-asia.ucsc.edu/) for enabling genomic interface.

**fanta.bio** is supported by [JST](https://www.jst.go.jp/) [NBDC](https://biosciencedbc.jp/) Grant Number JPMJND2202 in [Database Integration Coordination Program (DICP)](https://biosciencedbc.jp/en/funding/program/dicp/)


## References
- Mitsuhashi N, Toyo-Oka L, Katayama T, Kawashima M, Kawashima S, Miyazaki K, Takagi T. TogoVar: A comprehensive Japanese genetic variation database. Hum Genome Var. 2022 Dec 12;9(1):44. doi: [10.1038/s41439-022-00222-9](https://doi.org/10.1038/s41439-022-00222-9). PMID: 36509753; PMCID: PMC9744889.
- Takada T, Fukuta K, Usuda D, Kushida T, Kondo S, Kawamoto S, Yoshiki A, Obata Y, Fujiyama A, Toyoda A, Noguchi H, Shiroishi T, Masuya H. MoG+: a database of genomic variations across three mouse subspecies for biomedical research. Mamm Genome. 2022 Mar;33(1):31-43. doi: [10.1007/s00335-021-09933-w](https://doi.org/10.1007/s00335-021-09933-w). Epub 2021 Nov 15. PMID: 34782917; PMCID: PMC8913468.
- Zou Z, Ohta T, Miura F, Oki S. ChIP-Atlas 2021 update: a data-mining suite for exploring epigenomic landscapes by fully integrating ChIP-seq, ATAC-seq and Bisulfite-seq data. Nucleic Acids Res. 2022 Jul 5;50(W1):W175-W182. doi: [10.1093/nar/gkac199](https://doi.org/10.1093/nar/gkac199). PMID: 35325188; PMCID: PMC9252733.
- Abugessaisa I, Ramilowski JA, Lizio M, Severin J, Hasegawa A, Harshbarger J, Kondo A, Noguchi S, Yip CW, Ooi JLC, et al. FANTOM enters 20th year: expansion of transcriptomic atlases and functional annotation of non-coding RNAs. Nucleic Acids Res. 2021 Jan 8;49(D1):D892-D898. doi: [10.1093/nar/gkaa1054](https://doi.org/10.1093/nar/gkaa1054). PMID: 33211864; PMCID: PMC7779024.
- Andersson R, Gebhard C, Miguel-Escalada I, Hoof I, Bornholdt J, Boyd M, Chen Y, Zhao X, Schmidl C, Suzuki T, et al. An atlas of active enhancers across human cell types and tissues. Nature. 2014 Mar 27;507(7493):455-461. doi: [10.1038/nature12787](https://doi.org/10.1038/nature12787). PMID: 24670763; PMCID: PMC5215096.
- Forrest AR, Kawaji H, Rehli M, Baillie JK, de Hoon MJ, Haberle V, Lassmann T, Kulakovskiy IV, Lizio M, Itoh M, et al. A promoter-level mammalian expression atlas. Nature. 2014 Mar 27;507(7493):462-70. doi: [10.1038/nature13182](https://doi.org/10.1038/nature13182). PMID: 24670764; PMCID: PMC4529748.
