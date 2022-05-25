# Reproducibility Documentation

## Code Sources

[STAR tar](https://github.com/alexdobin/STAR/archive/refs/tags/2.7.4a.tar.gz)
[salmon tar](https://github.com/COMBINE-lab/salmon/releases/download/v1.5.2/salmon-1.5.2_linux_x86_64.tar.gz)

## Data Sources

[genome](https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/001/405/GCF_000001405.40_GRCh38.p14/GCF_000001405.40_GRCh38.p14_genomic.fna.gz)
[gtf](https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/001/405/GCF_000001405.40_GRCh38.p14/GCF_000001405.40_GRCh38.p14_genomic.gtf.gz)
[transcriptome](https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/001/405/GCF_000001405.40_GRCh38.p14/GCF_000001405.40_GRCh38.p14_rna_from_genomic.fna.gz)

## Snippets

### STAR index

```bash
genome_ref=/root/genome.fna
gtf=/root/genome.gtf

STAR \\
    --runMode genomeGenerate \\
    --genomeDir star/ \\
    --genomeFastaFiles $genome_ref \\
    --sjdbGTFfile $gtf \\
    --runThreadN 96 \\
```

### salmon index

```bash
genome_ref=/root/genome.fna
transcipt_ref=/root/transcript.fna

grep "^>" <(gunzip -c $genome_ref) | cut -d " " -f 1 > decoys.txt
sed -i.bak -e 's/>//g' decoys.txt
cat $transcript_ref $genome_ref  > gentrome.fa.gz
salmon index -t gentrome.fa.gz -d decoys.txt -p 12 -i salmon_index
```
