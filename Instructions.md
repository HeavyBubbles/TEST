# Limpiado de secuencias
Francisco Javier Falcon Chavez  
1/abril/2016  


Primero, se corrió el software FASTQC usando los datos de secuenciación como entrada, teneniendo en cuenta que todos los archivos con la extensión .fastq están almacenados en una carpeta dentro del directorio de trabajo llamada 'RawData'


```bash
cd /home/francisco/BioComp/Tarea1/

ls ./RawData/ > ./FASTQC_Analysis/List_Files.txt

for i in $( cat ./FASTQC_Analysis/List_Files.txt ); do
fastqc -o ./FASTQC_Analysis/ ./RawData/$i
done

rm ./FASTQC_Analysis/List_Files.txt
```
Luego, se descomprimen todos los archivos hechos por FASTQC, para poder revisar el archivo summar.txt y poder decir los siguientes pasos en el tratamiento de los datos


```bash
cd /home/francisco/BioComp/Tarea1/
ls ./FASTQC_Analysis/*zip > ./FASTQC_Analysis/List_Zips.txt

for i in $( cat ./FASTQC_Analysis/List_Zips.txt ); do
unzip $i -d ./FASTQC_Analysis/
done

rm ./FASTQC_Analysis/List_Zips.txt
```
 Teniendo descomprimidos los datos, podremos revisar cual son las características a grandes rasgos de los datos.
 
 

```bash
cd /home/francisco/BioComp/Tarea1/
cat FASTQC_Analysis/Partial_SRR24671*/summary.txt| cut -f1,2| sort| uniq -c
```

```
##      11 FAIL	Per base sequence content
##      11 PASS	Adapter Content
##      11 PASS	Basic Statistics
##      11 PASS	Kmer Content
##      11 PASS	Overrepresented sequences
##      11 PASS	Per base N content
##      11 PASS	Per base sequence quality
##       7 PASS	Per sequence GC content
##      11 PASS	Per sequence quality scores
##      11 PASS	Per tile sequence quality
##      11 PASS	Sequence Duplication Levels
##      11 PASS	Sequence Length Distribution
##       4 WARN	Per sequence GC content
```

Podemos observar que todos los archivos presentan una desproporción en el contenido de bases, el cual es probable sea en las primeras bases. Copiando el ejecutable de Trimmommatic y los adaptadores en la carpeta donde se colocarán los archivos tratados, se procede hacer el tratamiento.


```bash
cd /home/francisco/BioComp/Tarea1/Trimmed
ln -s ../RawData/* ./
ls *fastq > List_Files.txt
for i in $( cat ./List_Files.txt ); do
java -jar ./trimmomatic-0.36.jar SE $i Trimmed_$i ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 HEADCROP:10 SLIDINGWINDOW:4:15 MINLEN:50
done
rm List_Files.txt
```

```
## TrimmomaticSE: Started with arguments:
##  Partial_SRR2467141.fastq Trimmed_Partial_SRR2467141.fastq ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 HEADCROP:10 SLIDINGWINDOW:4:15 MINLEN:50
## Automatically using 4 threads
## Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
## ILLUMINACLIP: Using 1 prefix pairs, 0 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
## Quality encoding detected as phred33
## Input Reads: 5000 Surviving: 4904 (98.08%) Dropped: 96 (1.92%)
## TrimmomaticSE: Completed successfully
## TrimmomaticSE: Started with arguments:
##  Partial_SRR2467142.fastq Trimmed_Partial_SRR2467142.fastq ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 HEADCROP:10 SLIDINGWINDOW:4:15 MINLEN:50
## Automatically using 4 threads
## Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
## ILLUMINACLIP: Using 1 prefix pairs, 0 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
## Quality encoding detected as phred33
## Input Reads: 5000 Surviving: 4913 (98.26%) Dropped: 87 (1.74%)
## TrimmomaticSE: Completed successfully
## TrimmomaticSE: Started with arguments:
##  Partial_SRR2467143.fastq Trimmed_Partial_SRR2467143.fastq ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 HEADCROP:10 SLIDINGWINDOW:4:15 MINLEN:50
## Automatically using 4 threads
## Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
## ILLUMINACLIP: Using 1 prefix pairs, 0 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
## Quality encoding detected as phred33
## Input Reads: 5000 Surviving: 4908 (98.16%) Dropped: 92 (1.84%)
## TrimmomaticSE: Completed successfully
## TrimmomaticSE: Started with arguments:
##  Partial_SRR2467144.fastq Trimmed_Partial_SRR2467144.fastq ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 HEADCROP:10 SLIDINGWINDOW:4:15 MINLEN:50
## Automatically using 4 threads
## Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
## ILLUMINACLIP: Using 1 prefix pairs, 0 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
## Quality encoding detected as phred33
## Input Reads: 5000 Surviving: 4897 (97.94%) Dropped: 103 (2.06%)
## TrimmomaticSE: Completed successfully
## TrimmomaticSE: Started with arguments:
##  Partial_SRR2467145.fastq Trimmed_Partial_SRR2467145.fastq ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 HEADCROP:10 SLIDINGWINDOW:4:15 MINLEN:50
## Automatically using 4 threads
## Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
## ILLUMINACLIP: Using 1 prefix pairs, 0 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
## Quality encoding detected as phred33
## Input Reads: 5000 Surviving: 4883 (97.66%) Dropped: 117 (2.34%)
## TrimmomaticSE: Completed successfully
## TrimmomaticSE: Started with arguments:
##  Partial_SRR2467146.fastq Trimmed_Partial_SRR2467146.fastq ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 HEADCROP:10 SLIDINGWINDOW:4:15 MINLEN:50
## Automatically using 4 threads
## Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
## ILLUMINACLIP: Using 1 prefix pairs, 0 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
## Quality encoding detected as phred33
## Input Reads: 5000 Surviving: 4912 (98.24%) Dropped: 88 (1.76%)
## TrimmomaticSE: Completed successfully
## TrimmomaticSE: Started with arguments:
##  Partial_SRR2467147.fastq Trimmed_Partial_SRR2467147.fastq ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 HEADCROP:10 SLIDINGWINDOW:4:15 MINLEN:50
## Automatically using 4 threads
## Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
## ILLUMINACLIP: Using 1 prefix pairs, 0 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
## Quality encoding detected as phred33
## Input Reads: 5000 Surviving: 4907 (98.14%) Dropped: 93 (1.86%)
## TrimmomaticSE: Completed successfully
## TrimmomaticSE: Started with arguments:
##  Partial_SRR2467148.fastq Trimmed_Partial_SRR2467148.fastq ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 HEADCROP:10 SLIDINGWINDOW:4:15 MINLEN:50
## Automatically using 4 threads
## Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
## ILLUMINACLIP: Using 1 prefix pairs, 0 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
## Quality encoding detected as phred33
## Input Reads: 5000 Surviving: 4903 (98.06%) Dropped: 97 (1.94%)
## TrimmomaticSE: Completed successfully
## TrimmomaticSE: Started with arguments:
##  Partial_SRR2467149.fastq Trimmed_Partial_SRR2467149.fastq ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 HEADCROP:10 SLIDINGWINDOW:4:15 MINLEN:50
## Automatically using 4 threads
## Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
## ILLUMINACLIP: Using 1 prefix pairs, 0 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
## Quality encoding detected as phred33
## Input Reads: 5000 Surviving: 4918 (98.36%) Dropped: 82 (1.64%)
## TrimmomaticSE: Completed successfully
## TrimmomaticSE: Started with arguments:
##  Partial_SRR2467150.fastq Trimmed_Partial_SRR2467150.fastq ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 HEADCROP:10 SLIDINGWINDOW:4:15 MINLEN:50
## Automatically using 4 threads
## Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
## ILLUMINACLIP: Using 1 prefix pairs, 0 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
## Quality encoding detected as phred33
## Input Reads: 5000 Surviving: 4913 (98.26%) Dropped: 87 (1.74%)
## TrimmomaticSE: Completed successfully
## TrimmomaticSE: Started with arguments:
##  Partial_SRR2467151.fastq Trimmed_Partial_SRR2467151.fastq ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 HEADCROP:10 SLIDINGWINDOW:4:15 MINLEN:50
## Automatically using 4 threads
## Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
## ILLUMINACLIP: Using 1 prefix pairs, 0 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
## Quality encoding detected as phred33
## Input Reads: 5000 Surviving: 4904 (98.08%) Dropped: 96 (1.92%)
## TrimmomaticSE: Completed successfully
```
Se usaron los parametros por las siguientes razones:  
1. ILLUMINACLIP: Se usaron los adaptadores de la última versión de TruSeq, en PE con el objetivo de que elimine en ambos lados de la secuencia.  
2. HEADCROP: Para eliminar el problema del contenido de bases en cada archivo, se eliminarán las diez primeras bases.  
3. SLIDINGWINDOW: Con este margen, se cree que se obtiendrán secuencias de alta calidad.  
4. MINLEN: Basado en el parámetro usado en Chang et al. (2014), tratando de descartar la menor cantidad de secuencias útiles y que puedan reconstruir los transcritos de la mejor manera.  

Por último, se generaron los archivos de fastqc para este nuevo set de datos.


```bash
cd /home/francisco/BioComp/Tarea1/

ls ./Trimmed/Trimmed* > ./FASTQC_Analysis/List_Files.txt

for i in $( cat ./FASTQC_Analysis/List_Files.txt ); do
fastqc -o ./FASTQC_Analysis/ $i
done

rm ./FASTQC_Analysis/List_Files.txt
```

##Referencias##
Chang, Z., Wang., Z., & Li, G. (2014). The impacts of read length and transcriptome complexity for de ovo assembly: A simulation study. *PLoS ONE*, 9(4).
