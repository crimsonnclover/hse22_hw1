# hse22_hw1
## Создание ссылок
```
ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oil_R2.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq
```
## Отбор случайных чтений
```
seqtk sample -s1213 oil_R1.fastq 5000000 > sub1.fastq
seqtk sample -s1213 oil_R2.fastq 5000000 > sub2.fastq
seqtk sample -s1213 oilMP_S4_L001_R1_001.fastq 1500000 > mps1.fastq
seqtk sample -s1213 oilMP_S4_L001_R2_001.fastq 1500000 > mps2.fastq
```
## Оценка качества чтения
```
mkdir fastqc
mkdir multiqc
ls sub* mps* | xargs -tI{} fastqc -o fastqc {}
multiqc -o multiqc fastqc
```
## Результат для неподрезанных чтений:
![image](https://user-images.githubusercontent.com/99398496/194549721-b8d2ec76-df10-44a2-91e6-dc23ba2a5e49.png)
![image](https://user-images.githubusercontent.com/99398496/194550041-0620c05e-6a16-4155-aa31-ca1b3d3207ab.png)
![image](https://user-images.githubusercontent.com/99398496/194550080-8a43dde0-f624-4f58-b4d5-e737fd48bade.png)
![image](https://user-images.githubusercontent.com/99398496/194549983-f77fb102-317a-4442-8027-a517093ff430.png)
## Подрезаем чтения
```
platanus_trim sub*
platanus_internal_trim mps*
```
## Оценка качества после подрезания
```
ls *trimmed | xargs -tI{} fastqc -o fastqc {}
multiqc -o multiqc fastqc
```
## Результат для подрезанных чтений:
![image](https://user-images.githubusercontent.com/99398496/194559933-93c07234-0eb8-447f-848a-72186a7bff2d.png)
![image](https://user-images.githubusercontent.com/99398496/194559963-fd0317fe-4213-478d-a54d-011b4a8f4100.png)
![image](https://user-images.githubusercontent.com/99398496/194559989-377abca2-29f9-4b99-b602-6ffbf28ee7b4.png)
![image](https://user-images.githubusercontent.com/99398496/194560029-a53a3a45-cb5e-434a-ac39-ad1741ace567.png)
## Cбор контигов и скаффолдов
```
screen platanus assemble -o Poil -f sub1.fastq.trimmed sub2.fastq.trimmed 2> assemble.log
screen platanus scaffold -o Poil -c Poil_contig.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 mps1.fastq.int_trimmed mps2.fastq.int_trimmed 2>scaffold.log
```
Анализ контигов и скаффолдов:
https://colab.research.google.com/drive/1Z2MnNyDA8B92Zj3IiqTB_d4_QetQGCA8?usp=sharing
