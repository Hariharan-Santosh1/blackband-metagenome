BBD Metagenome: Binning
================
Your Name
2026-11-05

<!--
Explain what this step does and why, and copy each command you ran into a
bash chunk. R chunks run from the top of the repository.
-->

Binning of the assembled contigs into genomes with MetaBAT 2. Commands
for this step were run in `metabat/`.
\#———————————————————————————————————————# Change directory from flye to
metabat for binning

``` bash
cd metabat 
```

\#———————————————————————————————————————# Extract the depth info and
store it in depth_bbd.txt

``` bash
cat ../flye/bbd/assembly_info.txt  | grep -v '#' | awk '{OFS="\t";print $1,$3}' > depths_bbd.txt
```

\#———————————————————————————————————————# Reorder the contigs based on
their and depth and store in assembly_bbd_ordered.fasta

``` bash
cat depths_bbd.txt | awk '{print $1}' | xargs samtools faidx ../flye/bbd/assembly.fasta > assembly_bbd_ordered.fasta
```

\#———————————————————————————————————————# Run metabat

``` bash
metabat2 -i assembly_bbd_ordered.fasta -a depths_bbd.txt --cvExt -o bins_bbd/metabat --seed 5
```

MetaBAT 2 (nosha) using minContig 2500, minCV 1.0, minCVSum 1.0, maxP
95%, minS 60, maxEdges 200 and minClsSize 200000. with random seed=5 71
bins (39539582 bases in total) formed.
\#———————————————————————————————————————-#
