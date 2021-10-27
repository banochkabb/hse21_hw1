# hse21_hw1



$ seqtk sample -s117 oil_R1.fastq 5000000 > sub_r1.fastq 
$ seqtk sample -s117 oil_R2.fastq 5000000 > sub_r2.fastq
$ seqtk sample -s117 oilMP_S4_L001_R1_001.fastq 1500000 > mp_r1.fastq
$ seqtk sample -s117 oilMP_S4_L001_R2_001.fastq 1500000 > mp_r2.fastq
$ mkdir fastqc
$ mkdir multiqc
$ ls sub_r* mp_r* | xargs -tI{} fastqc -o fastqc {}
$ multiqc -o multiqc fastqc
$ platanus_trim sub_r*
$ platanus_internal_trim mp_r*
$ ls sub_r* mp_r* | xargs -tI{} fastqc -o trim_f {}
$ multiqc -o trim_m trim_f
$ platanus assemble -f sub_r1.fastq.trimmed sub_r2.fastq.trimmed 2> assemble.log
$ platanus scaffold -c out_contig.fa -IP1 sub_r1.fastq.trimmed sub_r2.fastq.trimmed -OP2 mp_r1.fastq.int_trimmed mp_r2.fastq.int_trimmed 2> scaffold.log
$ platanus gap_close -c out_scaffold.fa -IP1 sub_r1.fastq.trimmed sub_r2.fastq.trimmed -OP2 mp_r1.fastq.int_trimmed mp_r2.fastq.int_trimmed 2> gapclose.log
$ rsync -charvz vierinova@bioinf:/home/vierinova/hw1/multiqc
