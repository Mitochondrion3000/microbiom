а вот ссылка на папку по микробиомам(https://drive.google.com/drive/folders/1GIVDQ8x796n2Q7BvB3RiyZbZO1ZHeKFR)

📥 Получение данных imageДанные я взял отсюда: https://trace.ncbi.nlm.nih.gov/Traces/?view=run_browser&acc=SRR11027332&display=data-access

Активируем окружение и скачиваем fastq-файлы:

conda activate sra_env  
fastq-dump --split-files --gzip /home/ivan/Desktop/itmo/microbiom/SRR29762644

Так мы получили .fastq.gz файлы.
