BBD Metagenome: Binning
================
Your Name
2026-11-17

## Binning Assembled Contigs Using MetaBat 2

### Objective

In the second step , the assembled contigs are being grouped into
collections called bins using MetaBat 2 . Since the metagenomic samples
contain DNA from multiple organisms, the contigs may originate from
different microorganisms. MetaBat 2 uses two main characteristics,
sequence composition (patterns of DNA nucleotides) and sequencing depth
(the number of reads covering each contig) to group contigs that are
likely to originate from the same organism. Each resulting bin
represents a putative genome, providing a potential reconstruction of an
individual microorganism’s genome. These bins will then be used for
further taxonomic analysis to identify the microorganisms and genomic
analysis to investigate their genes and functions.

### Process

All the commands were executed in the metabat directory. Initially we
assemble the depth information extracted from FLye’s assembly_info.txt
and save it into depth_bbd.txt. These contigs were then reordered using
their identifiers and retirved from the original assembly FASTA file
using the samtools faidx.

``` bash
cd metabat 
cat ../flye/bbd/assembly_info.txt  | grep -v '#' | awk '{OFS="\t";print $1,$3}' > depths_bbd.txt
cat depths_bbd.txt | awk '{print $1}' | xargs samtools faidx ../flye/bbd/assembly.fasta > assembly_bbd_ordered.fasta
```

This command below runs MetaBat 2 using the prepared assembly and dpeth
information to group the contigs and push them into genome bins based on
their sequence composition along with coverage patterns.

``` bash
metabat2 -i assembly_bbd_ordered.fasta -a depths_bbd.txt --cvExt -o bins_bbd/metabat --seed 5
```

### Result

The resulting bins are saved in bins_bbd/, using the prefix metabat and
a fixed random seed of 5 to ensure reproducibility

| Parameter               | Result        |
|-------------------------|---------------|
| Binning tool            | MetaBAT 2     |
| Number of bins          | 71            |
| Total bases across bins | 39,539,582 bp |
| Minimum contig length   | 2,500 bp      |
| Random seed             | 5             |
| Output directory        | `bins_bbd/`   |
