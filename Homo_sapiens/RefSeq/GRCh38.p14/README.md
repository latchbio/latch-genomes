# Reproducibility Documentation

## Code Sources

- [STAR tar](https://github.com/alexdobin/STAR/archive/refs/tags/2.7.4a.tar.gz)
- [salmon tar](https://github.com/COMBINE-lab/salmon/releases/download/v1.5.2/salmon-1.5.2_linux_x86_64.tar.gz)
- [RSEM tar](https://github.com/deweylab/RSEM/archive/refs/tags/v1.3.3.tar.gz)

## Data Sources

- [genome](https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/001/405/GCF_000001405.40_GRCh38.p14/GCF_000001405.40_GRCh38.p14_genomic.fna.gz)
- [gtf](https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/001/405/GCF_000001405.40_GRCh38.p14/GCF_000001405.40_GRCh38.p14_genomic.gtf.gz)

## Snippets

### Generate Transcript FASTA

```bash

genome_ref=GCF_000001405.40_GRCh38.p14_genomic.fna
gtf=GCF_000001405.40_GRCh38.p14_genomic.gtf

rsem-prepare-reference \\
    --gtf $gtf \\
    --num-threads 96 \\
    $genome_ref \\
    rsem/genome
```


### STAR index

```bash
genome_ref=GCF_000001405.40_GRCh38.p14_genomic.fna
gtf=GCF_000001405.40_GRCh38.p14_genomic.gtf

STAR \\
    --runMode genomeGenerate \\
    --genomeDir star/ \\
    --genomeFastaFiles $genome_ref \\
    --sjdbGTFfile $gtf \\
    --runThreadN 96 \\
```

### salmon index

```bash
genome_ref=GCF_000001405.40_GRCh38.p14_genomic.fna.gz
transcipt_ref=GCF_000001405.40_GRCh38.p14_genomic.transcripts.fna.gz

grep "^>" <(gunzip -c $genome_ref) | cut -d " " -f 1 > decoys.txt
sed -i.bak -e 's/>//g' decoys.txt
cat $transcript_ref $genome_ref  > gentrome.fa.gz
salmon index -t gentrome.fa.gz -d decoys.txt -p 96 -i salmon_index
```
