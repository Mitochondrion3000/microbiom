вот такое было задание 
![image](https://github.com/user-attachments/assets/1fdde9d8-846d-45e1-9746-b5419b5b9d1b)
а вот ссылка на папку по микробиомам(https://drive.google.com/drive/folders/1GIVDQ8x796n2Q7BvB3RiyZbZO1ZHeKFR)

 
 ---

📥 Получение данных
![image](https://github.com/user-attachments/assets/ffba42f3-7a9b-4f1a-a07d-b9c49f4d6d8b)Данные я взял отсюда:
[https://trace.ncbi.nlm.nih.gov/Traces/?view=run\_browser\&acc=SRR29762644\&display=data-access](https://trace.ncbi.nlm.nih.gov/Traces/?view=run_browser&acc=SRR29762644&display=data-access)

Активируем окружение и скачиваем fastq-файлы:

```bash
conda activate sra_env  
fastq-dump --split-files --gzip /home/ivan/Desktop/itmo/microbiom/SRR29762644
```

Так мы получили `.fastq.gz` файлы.

❓ Вопрос
Меня очень интересует момент: почему анализ кала может весить как 200 МБ, так и 7 ГБ? Откуда такая разница берется?

---

🧬 Kraken2 — подготовка и запуск

📦 Скачивание базы данных
Я использовал эту базу:
[https://genome-idx.s3.amazonaws.com/kraken/k2\_standard\_08gb\_20250402.tar.gz](https://genome-idx.s3.amazonaws.com/kraken/k2_standard_08gb_20250402.tar.gz)

Распаковал архив. Возможно, было бы лучше сделать это в отдельную папку, но я распаковал в рабочую директорию — это оказалось ошибкой, так как там оказалось очень много файлов.

🔧 Установка Kraken2
После установки файлов с GitHub не получилось добавить необходимые файлы в PATH.
Поэтому я установил Kraken2 через conda в отдельное окружение — так всё заработало.

▶️ Запуск Kraken2

```bash
kraken2 --confidence 0.1 --paired --threads 60 \
--report report.txt --use-names --db /home/ivan/Desktop/itmo/microbiom/ \
/home/ivan/Desktop/itmo/microbiom/SRR29762644_1.fastq.gz \
/home/ivan/Desktop/itmo/microbiom/SRR29762644_2.fastq.gz
```

Интересное:

* Не работает параметр из лекции: `--minimum-hit-groups 3`, говорит, что такого параметра нет вовсе.
* Из команды в лекции я убрал штуку, которая не даёт выводить работу Kraken в терминал.
* Поставил `--confidence 0.1`, потому что иначе у меня ничего не находило :(
* Параметр `--classified-out`, который, как было сказано в лекции, "выдаёт прочтения, которые были аннотированы", я не использовал.

---

📚 Выписка из лекции:

```
kraken2 --minimum-hit-groups 3 --confidence 0.5 --paired \
--threads 60 --report report1gr_st_r.txt --use-names \
--db /mnt/disk1/db/k2_db fed1gr_S1_L001_R1_001.fastq fed1gr_S1_L001_R2_001.fastq > /dev/null
```

Комментарии к параметрам:

* `--minimum-hit-groups 3` и `--confidence 0.5` используются для уменьшения ложноположительных результатов.
* `--report` выдаёт отчёт о числе прочтений, относящихся к таксонам.
* `--classified-out` выдаёт прочтения, которые были аннотированы.

📤 Что мы получаем на выходе:
(описание из пункта 2 раздела "что мы получаем")

---




### Что мы поулчаем на выходе

1. **`report.txt`** — *таксономический отчёт в виде таблицы*, содержащий:
---

### 📄 Формат файла `report.txt`

Каждая строка соответствует одному таксону. Пример:

```
 8.29	146660	146043	S	1747	Cutibacterium acnes
```

#### Расшифровка колонок:

| Колонка | Пример                | Значение                                                                                     |
| ------- | --------------------- | -------------------------------------------------------------------------------------------- |
| 1       | `8.29`                | **Процент** всех прочтений, отнесённых к этому таксону и его потомкам                        |
| 2       | `146660`              | **Общее количество** прочтений, отнесённых к этому таксону и его потомкам                    |
| 3       | `146043`              | Количество прочтений, **непосредственно классифицированных** к этому таксону (а не потомкам) |
| 4       | `S`                   | **Таксономический уровень** (см. ниже)                                                       |
| 5       | `1747`                | **TaxID** (уникальный идентификатор NCBI для таксона)                                        |
| 6       | `Cutibacterium acnes` | **Имя таксона**, отформатированное с отступами, отражающими иерархию                         |

---

### 🧬 Значения уровней таксона (4-я колонка)

| Символ | Уровень           |
| ------ | ----------------- |
| U      | Unclassified      |
| R      | Root              |
| R1     | Domain            |
| R2     | Superkingdom      |
| K      | Kingdom           |
| P      | Phylum            |
| C      | Class             |
| O      | Order             |
| F      | Family            |
| G      | Genus             |
| S      | Species           |
| S1     | Subspecies/strain |

---

### 📊 Пример расшифровки строки

```
 8.29	146660	146043	S	1747	Cutibacterium acnes
```

* **8.29%** всех прочтений в файле были классифицированы как *Cutibacterium acnes* или ниже (включая подвиды)
* **146660** прочтений в сумме относятся к *C. acnes* и его подвидам
* **146043** прочтений напрямую классифицированы именно как *C. acnes*, а не подвид
* **S** = уровень “вид”
* **1747** — TaxID в базе NCBI
* Отступ у имени таксона показывает, что это поддерево в таксономии

---

### 🧠 Несколько наблюдений из вашего отчёта:

* **84.77%** прочтений не классифицированы вовсе → вероятно, база Kraken2 неполная или это человеческая ДНК/шум.
* Основной классифицированный таксон — **Cutibacterium acnes (8.29%)**: типичный представитель кожного микробиома.
* Также найдены:

  * *Cutibacterium granulosum* (0.15%)
  * *Cutibacterium avidum* (0.02%)
    → все — типичные актинобактерии, ассоциированные с кожей.

---


   Пример строки в `report.txt`:

   ```
   13.21  105432  105432  U  0  unclassified
   86.79  692345  692345  R  1  root
   ...
   ```

2. **Стандартный вывод в терминал (или в `--output` файл, если указан)** — построчная классификация каждого прочтения:

##

### 1. **Строки классификации**:

Каждая строка в начале — это результат **по одному прочтению**. Вот структура:

```
C|U   <read_id>   <taxon name (taxid)>   <read lengths>   <k-mer hits>
```

Примеры:

---

#### ✅ Классифицированное прочтение:

```
C	SRR29762644.1768750	Cutibacterium acnes (taxid 1747)	151|151	1747:2 0:37 1747:3 0:6 31957:3 1747:4 0:20 1747:4 0:38 |:| 0:6 2:3 0:56 1747:4 0:20 1747:4 31957:3 0:6 1747:3 0:12
```

🔹 **C** — классифицировано
🔹 **ID прочтения**: `SRR29762644.1768750`
🔹 **Результат**: отнесено к *Cutibacterium acnes* (таксон ID 1747)
🔹 **151|151** — длина каждого рида в паре
🔹 **k-mer метки**:

* `1747:2` означает, что 2 k-мера были найдены, которые сопоставились с таксоном 1747
* `|:|` разделяет forward и reverse read
* И так далее…

---

#### ❌ Неклассифицированное прочтение:

```
U	SRR29762644.1768751	unclassified (taxid 0)	151|151	0:3 1747:4 0:44 1747:3 0:63 |:| 0:2 1747:1 0:9 1747:1 0:8 1747:3 0:19 1747:1 0:23 1747:8 0:42
```

🔹 **U** — unclassified
🔹 Есть совпадения с таксоном `1747`, но они недостаточно уверенные (или не преодолели порог `--confidence 0.1`)
🔹 Поэтому прочтение не классифицировано

---

### Потом пихаем это все в Pavian(https://shiny.hiplot.cn/pavian/)
![image](https://github.com/user-attachments/assets/a10bb8f0-a1f6-4171-b910-8b0dd3f22795)
вот что получаем



---

###


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
