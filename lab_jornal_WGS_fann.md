spades --pe1-1 /home/ivan/Desktop/itmo/microbiom/skin_microbiome/SRR29762644_1.fastq.gz --pe1-2 /home/ivan/Desktop/itmo/microbiom/skin_microbiome/SRR29762644_2.fastq.gz --disable-gzip-output --threads 12 -o SPAdes_short_k33_77_111 -k 33,77,111

получаем скафолды 
вечно забываю разницу между контигами и скафолдами, поэтому вот так

Контиг (от англ. contiguous) — это непрерывная последовательность ДНК, собранная из перекрывающихся фрагментов, полученных, например, методом дробовика. 

Скаффолд (от англ. scaffold) — это упорядоченный набор контигов, соединённых вместе с известными промежутками (gap), где последовательность не определена. Эти промежутки могут быть разной длины, и они символизируют участки ДНК, которые пока не удалось секвенировать или собрать

 bwa index /home/ivan/Desktop/itmo/microbiom/skin_microbiome/SPAdes_short_k33_77_111/scaffolds.fasta индексируем наш файл 

 bwa mem -t 12 /home/ivan/Desktop/itmo/microbiom/skin_microbiome/SPAdes_short_k33_77_111/scaffolds.fasta /home/ivan/Desktop/itmo/microbiom/skin_microbiome/SRR29762644_1.fastq.gz /home/ivan/Desktop/itmo/microbiom/skin_microbiome/SRR29762644_1.fastq.gz > sample_skin.sam и потом если я правильно понял сырые риды мы выравниваем на ту сборку которая у нас получилась 

затем скачиваем metabat_env
conda create -n metabat_env -c bioconda metabat2
conda activate metabat_env



