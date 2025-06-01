а вот ссылка на папку по микробиомам(https://drive.google.com/drive/folders/1GIVDQ8x796n2Q7BvB3RiyZbZO1ZHeKFR)

📥 Получение данных 
![image](https://github.com/user-attachments/assets/3e7009a4-65f9-466f-b8da-6b0043db0edb)


Данные я взял отсюда: https://trace.ncbi.nlm.nih.gov/Traces/?view=run_browser&acc=SRR11027332&display=data-access

Активируем окружение и скачиваем fastq-файлы:

conda activate sra_env  
fastq-dump --split-files --gzip /home/ivan/Desktop/itmo/microbiom/navel_microbiome/SRR11027332

Так мы получили .fastq.gz файлы.

потом выполняем команду 
