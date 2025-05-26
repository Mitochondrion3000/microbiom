данные я взял отсюда https://trace.ncbi.nlm.nih.gov/Traces/?view=run_browser&acc=SRR29762644&display=data-access
![image](https://github.com/user-attachments/assets/ffba42f3-7a9b-4f1a-a07d-b9c49f4d6d8b)


conda activate sra_env
fastq-dump --split-files --gzip /home/ivan/Desktop/itmo/microbiom/SRR29762644 - так мы получили fastq файлы

меня еще очень интересует момент что анализ кала может весить как и 200мб так и 7 гб, почему так, откуда ткакая разница берется 

осталось запустить кракена

для начала хорошо бы скачать базу данных, я взял эту https://genome-idx.s3.amazonaws.com/kraken/k2_standard_08gb_20250402.tar.gz
















Kraken2. Установка

• Исходной код с GitHub

$ git clone https://github.com/DerrickWood/kraken2.git
$ cd kraken2
$ ./install_kraken2.sh $KRAKEN2_DIR
$ nano ~/.bashrc # прописать пути до исполняемых файлов
$ exec bash

• Установка для conda (не рекомендуется из-за нестабильной работы)

$ conda create -n kraken2 python=3.9
$ conda activate kraken2
$ conda install -c bioconda kraken2

Kraken2. Работа

• kraken2 --minimum-hit-groups 3 --confidence 0.5 --paired --
threads 60 --report report1gr_st_r.txt --use-names --db
/mnt/disk1/db/k2_db fed1gr_S1_L001_R1_001.fastq
fed1gr_S1_L001_R2_001.fastq > /dev/null

--minimum-hit-groups 3 и --confidence 0.5 используется для уменьшения ложноположительных
результатов
--report выдает отчет о числе прочтений, относящихся к таксонам
--classified-out выдает прочтения, которые были аннотированы
