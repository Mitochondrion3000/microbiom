данные я взял отсюда https://trace.ncbi.nlm.nih.gov/Traces/?view=run_browser&acc=SRR29762644&display=data-access
![image](https://github.com/user-attachments/assets/ffba42f3-7a9b-4f1a-a07d-b9c49f4d6d8b)

 
conda activate sra_env
fastq-dump --split-files --gzip /home/ivan/Desktop/itmo/microbiom/SRR29762644 - так мы получили fastq файлы

меня еще очень интересует момент что анализ кала может весить как и 200мб так и 7 гб, почему так, откуда ткакая разница берется 

осталось запустить кракена

для начала хорошо бы скачать базу данных, я взял эту https://genome-idx.s3.amazonaws.com/kraken/k2_standard_08gb_20250402.tar.gz
а потом нужно распоковать архив, навреное хорошо бы сделать это в отлеьную папку, но я распаковал в рабочую, это было ошибкой ибо там получилось много файлов
















Kraken2. Установка

После установки файлов с git добавить необходимиые файлы в path у меня не получилось поэтому поставил кракена через конду в отдельное окружение и все заработало 


Запуск самого кракена

 kraken2 --confidence 0.1 --paired --threads 60 --report report.txt --use-names --db /home/ivan/Desktop/itmo/microbiom/ /home/ivan/Desktop/itmo/microbiom/SRR29762644_1.fastq.gz /home/ivan/Desktop/itmo/microbiom/SRR29762644_2.fastq.gz

### Что мы поулчаем на выходе

1. **`report.txt`** — *таксономический отчёт в виде таблицы*, содержащий:

   * Долю прочтений, классифицированных к данному таксону
   * Количество прочтений
   * Таксономический уровень (D = domain, P = phylum, ... S = species)
   * Название таксона

   Пример строки в `report.txt`:

   ```
   13.21  105432  105432  U  0  unclassified
   86.79  692345  692345  R  1  root
   ...
   ```

2. **Стандартный вывод в терминал (или в `--output` файл, если указан)** — построчная классификация каждого прочтения:

📌 Подробный разбор

1. Строки классификации:

Каждая строка в начале — это результат по одному прочтению. Вот структура:

C|U   <read_id>   <taxon name (taxid)>   <read lengths>   <k-mer hits>

Примеры:

✅ Классифицированное прочтение:

C	SRR29762644.1768750	Cutibacterium acnes (taxid 1747)	151|151	1747:2 0:37 1747:3 0:6 31957:3 1747:4 0:20 1747:4 0:38 |:| 0:6 2:3 0:56 1747:4 0:20 1747:4 31957:3 0:6 1747:3 0:12

🔹 C — классифицировано🔹 ID прочтения: SRR29762644.1768750🔹 Результат: отнесено к Cutibacterium acnes (таксон ID 1747)🔹 151|151 — длина каждого рида в паре🔹 k-mer метки:

1747:2 означает, что 2 k-мера были найдены, которые сопоставились с таксоном 1747

|:| разделяет forward и reverse read

И так далее…

❌ Неклассифицированное прочтение:

U	SRR29762644.1768751	unclassified (taxid 0)	151|151	0:3 1747:4 0:44 1747:3 0:63 |:| 0:2 1747:1 0:9 1747:1 0:8 1747:3 0:19 1747:1 0:23 1747:8 0:42

🔹 U — unclassified🔹 Есть совпадения с таксоном 1747, но они недостаточно уверенные (или не преодолели порог --confidence 0.1)🔹 Поэтому прочтение не классифицировано
> ⚠️ Если вы **не указали `--output <file>`**, то эти строки появятся в терминале, а не сохранятся.

---












• Исходной код с GitHub

$ git clone https://github.com/DerrickWood/kraken2.git
$ cd kraken2
$ ./install_kraken2.sh ~/Desktop/itmo/microbiom/kraken2
$ nano ~/.bashrc
вот такую команду выполяем
потом во время уствновки программа сама выдала пути к исполяемым файлам и в конец .bashrc я добавил 

export PATH="$PATH:/home/ivan/Desktop/itmo/microbiom/kraken2/kraken2"
export PATH="$PATH:/home/ivan/Desktop/itmo/microbiom/kraken2/kraken2-build"
export PATH="$PATH:/home/ivan/Desktop/itmo/microbiom/kraken2/kraken2-inspect" ---- с этим этапом что-то не то 

$ exec bash - применяем изменения (альтернатива source ~/.bashrc)
и еще всем эти файлам нужно дать права на исполение 

проверяем

which kraken2
which kraken2-build
which kraken2-inspect ------------ не работает 

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
