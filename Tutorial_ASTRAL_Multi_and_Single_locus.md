## Análisis coalescente multiespcies (MSC) con ASTRAL
___

by David S. Gernandt and Rubén Montes
Versión 06 de Marzo de 2019
--

**Objetico**

Realizar análisis de máxima verosimilitud a partir de alineamientos múltiples mediante un script para RAxML.
Combinar los mejores árboles en un sólo archivo.
Realizar un análisis de MSC en ASTRAL a partir de los árboles provenientes de los loci. 

**Presentación**
ASTRAL es un método de MSC (multi-species coalescent) que resume un conjunto de árboles de genes para estimar un árbol de especie. Cuenta con propiedades deseables como consistencia, precisión y rapidez. En este ejercicio se estimarán árboles de gen con RAxML y posteriormente el árbol de especie con ASTRAL-III.

**A. Un script de RAxML para analizar una tanda de alineamientos en una carpeta**

Algunos métodos de MSC, los métodos de resumen, requieren calcular árboles para cada gen individualmente y combinarlos en un archivo. En `RAxML` existe un script (`applyRAxML2AllFilesInDirectory.pl`) para facilitar el trabajo. Para este ejemplo, el script está dentro de una carpeta con el nombre `raxml` y los alineamientos están en un subdirectorio de `raxml`. Entra al subdirectorio con los alineamientos y teclear: 

```
perl ../applyRAxML2AllFilesInDirectory.pl ./ "/usr/local/bin/raxmlHPC-PTHREADS-AVX -T 12 -f a -m GTRGAMMA -o devoDSG721 -p 685387 -x 574684 -# autoMRE"
```
**Nota**: Los resultados incluyen los mejores árboles y datos sobre valores de bootstrap


**B. Concatenar la colección de los mejores árboles para cada gen en un archivo**

Dentro del directorio con los resultados de `raxml`:

```
mkdir besttrees
mv RAxML_bestTree.* besttrees
cd besttrees
ls
ls -1 | wc -l
cat RAxML_best* > alltrees.tre

```

**C. Estimar el árbol de especie con ASTRAL**

Investiga la ubicación y versión del programa ASTRAL en tu computadora. Si no se encuentra en tu path (como en `/usr/local/bin/Astral/`) las instrucciones para instalarlo localmente están en el apéndice de este tutorial. Para este ejercicio ASTRAL está instalado localmente en la cuenta del usuario. Para confirmar que ASTRAL está funcionando correctamente, cambia al directorio donde se instaló el programa y convoca el programa sin especificar una archivo de entrada:


`java -jar Astral/astral.4.1.2.jar`

**Nota**: Si descargaste o clonaste el reposotorio de ASTRAL directamente en tu `PATH` es recomendable descomprimir el archivo.zip y luego ejecutar el archivo `make.sh` que se encuentra dentro de la carpeta de ASTRAL que descargaste. Teclea los siguiente comandos

`unzip astral.4.1.2.jar.zip`

`bash make.sh`

Como alternativa puedes correr el archivo de ejemplo:

```
java -jar Astral/astral.4.1.2.jar test_data/song_mammals.424.gene.tre -o test_data/song_mammals.tre
```

Coloca el archivo con los árboles de genes concatenados (en este ejemplo `alltrees.tre` en la carpeta `astral` y desde la misma carpeta teclea: 

```
java -jar ../astral.4.11.12.jar -i alltrees.tre -o Australes_n339_t28_Astral.tre 2>Australes_n339_t28.out.txt

```

Si el análisis corrió correctamente, el archivo de salida debe contener el árbol de especie:

`cat Australes_n339_t28.out.txt`


Se puede pasar el archivo de salida a un PC o Mac mediante FTP, o copiarlo y guardarlo como nuevo archivo, y abrirlo en un programa como FigTree.

Si los alineamientos incluyen más de individuo por especie, ASTRAL-III permite mapear cada individuo a su especie antes de calcular el árbol. 

**C. Análisis con múltiples individuos en ASTRAL-III**

Primero debes contar con los siguientes archivos:
El archivo de los todos los genes de RAxMl `alltrees.tre`
Un `archivo.txt` con la lista especies y el número de individuos asignados a cada especie.


Crea un nuevo directorio

`mkdir t70_n339`

Mueve el archivo `alltrees.tre` a `t70_n339`:

`mv alltrees.tre t70_n339`
 

El archivo lo puedes crear en `nano`, teclea:

`nano multi-individuos.txt`

Ahora utiliza el siguiente ejemplo para llenar tu archivo.txt

Especie: individuo1,individuo2


```
aristata: aris02S1,aris05S2
bungeana: bung03s3A
californiarum: caliBC01
cembroides: cemdCHI01,cemdHGO,cemdSLP01
culminicola: culmCOA01,culmCOA02,culmNL01,culmNL02
discolor: discARI01,discARI02,discSON01
edulis: edulUTAH01,edulUTAH02,edulUTAH03
fallax: fallUTAH01,fallUTAH02
gerardiana: gera01S1
johannis: johaSLP01,johaZAC01,johaZAC02
krempfii: krem03N12
lagunae: laguBCS01,laguBCS02
lambertiana: lamb1195
longaeva: longUTAH01
maximartinezii: maxtZAC01,maxtZAC02,maxtZAC03
monophylla: monoCAL01,monoUSA01,monoUTAH02,caliCAL01,caliCAL03,caliCAL04,caliCAL05
nelsonii: nelsNL01,nelsSLP01,nelsTAM01
orizabensis: orizPUE01
pinceana: pincCOA01,pincSLP01
quadrifolia: quadBC01,quadBC02,quadBC03,quadBC04,quadBC05,quadBC06
remota: remoCOA01,remoCOA02,remoCOA03,remoCOA04,remoNL01
rzedowskii: rzedMICH01,rzedMICH02,rzedMICH03,rzedMICH04
```

**Nota**: Debes dejar un especio después de los dos puntos.

Ejecuta la siguiente línea de comandos:

```
java -jar ./astral.5.5.9.jar -i alltrees.tre -a multi-individual.txt -o Australes_n339_allRAxML_Astral_multind.tre 2>Australes_n339_multiind.out.txt
```


**Apéndice**

Para instalar ASTRAL en una máquina LINUX o UNIX:

`mkdir astral`

`cd astral`

`wget https://github.com/smirarab/ASTRAL/raw/master/Astral.4.11.2.zip`

`unzip Astral.4.11.2.zip`




**Referencias**

Mirarab, S., R. Reaz, M.S. Bayzid, T. Zimmermann, M.S. Swenson, and T. Warnow. 2014. ASTRAL: genome-scale coalescent-based species tree estimation. Bioinformatics 30: i541–i548.

Mirarab, S., and T. Warnow. 2015. ASTRAL-II: coalescent-based species tree estimation with many hundreds of taxa and thousands of genes. Bioinformatics 31: i44–i52.

Stamatakis, A. 2014. RAxML version 8: a tool for phylogenetic analysis and post-analysis of large phylogenies. Bioinformatics 30: 1312–1313.

