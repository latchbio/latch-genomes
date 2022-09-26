# Reproducibility Documentation

## Code Sources

- [STAR tar](https://github.com/alexdobin/STAR/archive/refs/tags/2.7.10a.tar.gz)
- [salmon tar](https://github.com/COMBINE-lab/salmon/releases/download/v1.8.0/salmon-1.8.0_linux_x86_64.tar.gz)
- [RSEM tar](https://github.com/deweylab/RSEM/archive/refs/tags/v2.3.3.tar.gz)

## Data Sources

- [genome](https://ftp.ncbi.nlm.nih.gov/genomes/all/annotation_releases/10090/109/GCF_000001635.27_GRCm39/GCF_000001635.27_GRCm39_genomic.fna.gz)
- [gtf](https://ftp.ncbi.nlm.nih.gov/genomes/all/annotation_releases/10090/109/GCF_000001635.27_GRCm39/GCF_000001635.27_GRCm39_genomic.gtf.gz)

## Snippets

### Strip non-transcript annotations

Recent GTF files have [pathological](https://github.com/gpertea/gffread/issues/84) non-transcript annotations.

```
grep -P '\btranscript_id\s+"[^"]+"' *.gtf > GCF_000001405.40_GRCh38.p14_genomic.stripped.gtf
```

### Generate Transcript FASTA

```bash

genome_ref=GCF_000001405.40_GRCh38.p14_genomic.fna
gtf=GCF_000001405.40_GRCh38.p14_genomic.stripped.gtf

/home/ubuntu/RSEM-1.3.3/rsem-prepare-reference \
    --gtf $gtf \
    --num-threads 96 \
    $genome_ref \
    rsem/genome
```


### STAR index

```bash
genome_ref=GCF_000001405.40_GRCh38.p14_genomic.fna
gtf=GCF_000001405.40_GRCh38.p14_genomic.stripped.gtf

/home/ubuntu/STAR-2.7.10a/bin/Linux_x86_64_static/STAR \
    --runMode genomeGenerate \
    --genomeDir star/ \
    --genomeFastaFiles $genome_ref \
    --sjdbGTFfile $gtf \
    --runThreadN 96 
```

### salmon index

```bash
genome_ref=GCF_000001405.40_GRCh38.p14_genomic.fna
transcipt_ref=GCF_000001405.40_GRCh38.p14_genomic.transcripts.fna

grep "^>" $genome_ref | cut -d " " -f 1 > decoys.txt
sed -i.bak -e 's/>//g' decoys.txt
cat $transcript_ref $genome_ref  > gentrome.fa.gz
/home/ubuntu/salmon-1.5.2_linux_x86_64/bin/salmon index -t gentrome.fa.gz -d decoys.txt -p 96 -i salmon_index
```
