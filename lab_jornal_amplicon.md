![image](https://github.com/user-attachments/assets/138c198a-b306-427c-8e5f-d5e2e8b156b8)


а вот ссылка на папку по микробиомам(https://drive.google.com/drive/folders/1GIVDQ8x796n2Q7BvB3RiyZbZO1ZHeKFR)

📥 Получение данных 
![image](https://github.com/user-attachments/assets/3e7009a4-65f9-466f-b8da-6b0043db0edb)


Данные я взял отсюда: https://trace.ncbi.nlm.nih.gov/Traces/?view=run_browser&acc=SRR11027332&display=data-access

Активируем окружение и скачиваем fastq-файлы:

conda activate sra_env  
fastq-dump --split-files --gzip /home/ivan/Desktop/itmo/microbiom/navel_microbiome/SRR11027332

Так мы получили .fastq.gz файлы.
Качам базу данных для 16S
![image](https://github.com/user-attachments/assets/09ed1148-c9b1-4981-86e8-8c1212051c18)

база silva отсюда https://benlangmead.github.io/aws-indexes/k2

И запускаем кракена по команде: 
kraken2 --threads 12 --confidence 0.2 --db /home/ivan/Desktop/itmo/microbiom/navel_microbiome/16S_SILVA138_k2db --paired /home/ivan/Desktop/itmo/microbiom/navel_microbiome/SRR11027332_1.fastq.gz /home/ivan/Desktop/itmo/microbiom/navel_microbiome/SRR11027332_2.fastq.gz --report /home/ivan/Desktop/itmo/microbiom/navel_microbiome/1.report

вот столько было классифицированно 

70293 sequences (42.07 Mbp) processed in 1.103s (3824.5 Kseq/m, 2288.90 Mbp/m).

  69112 sequences classified (98.32%)
  1181 sequences unclassified (1.68%)

и такой график 
![image](https://github.com/user-attachments/assets/70f717b1-9d47-4d02-a4b1-d2ce0c822017)

если ставим --confidence 0.5

70293 sequences (42.07 Mbp) processed in 1.110s (3801.3 Kseq/m, 2275.03 Mbp/m).

  55971 sequences classified (79.63%)
  14322 sequences unclassified (20.37%)


![image](https://github.com/user-attachments/assets/cdd5aa67-2e29-49fa-bbf1-b1870829171c)

если --confidence 0.7 график павиан строить не желает 

70293 sequences (42.07 Mbp) processed in 1.124s (3751.1 Kseq/m, 2245.02 Mbp/m).

  44432 sequences classified (63.21%)
  25861 sequences unclassified (36.79%)




