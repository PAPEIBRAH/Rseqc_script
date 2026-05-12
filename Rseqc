#!/bin/bash
# Script RSeQC optimisé pour un echantillon
# Version rapide avec multithreading et parallélisation

# Variables
BAM="/home/pfaye/Bureau/BAM_Lymphocyte/2023A241.Aligned.sortedByCoord.out_dedup.bam"
SAMPLE="2023A241"
REF_BED="/home/pfaye/Bureau/Homo_sapiens.GRCh38.79.bed"
OUTDIR="${SAMPLE}_RSeQC"
THREADS=32   # nombre de threads à utiliser

# Créer le répertoire de sortie
mkdir -p $OUTDIR

echo "Filtrage, tri et indexation du BAM..."
# Filtrer reads non alignés ou de faible qualité, trier et indexer
samtools view -b -q 10 -F 4 $BAM | samtools sort -@ $THREADS -o ${OUTDIR}/${SAMPLE}.filtered.sorted.bam
samtools index ${OUTDIR}/${SAMPLE}.filtered.sorted.bam

BAM_SORTED="${OUTDIR}/${SAMPLE}.filtered.sorted.bam"

echo "Lancement des modules RSeQC en parallèle..."

# Junction saturation
junction_saturation.py -i $BAM -r $REF_BED -o ${OUTDIR}/${SAMPLE}_junctionSaturation &

# Junction annotation
junction_annotation.py -i $BAM -r $REF_BED -o ${OUTDIR}/${SAMPLE}_junctionAnnotation &

# Ajouter d'autres modules si besoin, par exemple geneBody_coverage
geneBody_coverage.py -i $BAM -r $REF_BED -o ${OUTDIR}/${SAMPLE}_geneBody &

# Attendre la fin de tous les processus
wait

echo "Analyse terminée. Tous les fichiers sont dans $OUTDIR"
