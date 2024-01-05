# singularity/apptainer exec
Singularity has a `run` subcommand. This can be used to run the user-defined default command within a container. However, it is recommended to use another subcommand `exec` instead of `run`. If you're interested in `run`, please check the [singularity user guide](https://docs.sylabs.io/guides/3.8/user-guide/cli/singularity_run.html#singularity-run). 

## Syntax
Download or build a container from a given URI. 
```
singularity/apptainer exec [options] image command
```

After we pulled images from public registries onto the cluster, we can use them to run analysis. If you haven't pulled images, please follow the guideline in the [Pull container images](hands-on/pull_run.md).

## Test datasets
I stored test datasets for blast and pytorch, please copy the datasets to your own folder.
```
$ cp /cluster/tufts/biocontainers/workshop/Spring2024Container/data/* .
# ls
blastdb.faa input.fasta torch_cpu.py
```

## blast
#### makeblastdb
```
$ singularity exec blast_2.15.0.sif makeblastdb -in blastdb.faa -dbtype prot -out blastdb

Building a new DB, current time: 01/05/2024 12:36:38
New DB name:   /cluster/home/yzhang85/blastdb
New DB title:  blastdb.faa
Sequence type: Protein
Keep MBits: T
Maximum file size: 3000000000B
BLAST options error: File blastdb.faa does not exist
```

Why there is an error `BLAST options error: File blastdb.faa does not exist` :cry:? 
:sweat: I forgot to bind `/cluster/tufts` into the container. 
Let's try it again.


```
singularity exec -B /cluster/tufts blast_2.15.0.sif makeblastdb -in blastdb.faa -dbtype prot -out blastdb


Building a new DB, current time: 01/05/2024 12:38:17
New DB name:   /cluster/tufts/yzhang85/workshop/blastdb
New DB title:  blastdb.faa
Sequence type: Protein
Keep MBits: T
Maximum file size: 3000000000B
Adding sequences from FASTA; added 39 sequences in 0.0381441 seconds.
```

#### blastp
```
$ singularity exec -B /cluster/tufts blast_2.15.0.sif blastp -query input.fasta -db blastdb -out blastout.txt -num_threads 2
$ head -n 40 blastout.txt 
BLASTP 2.15.0+


Reference: Stephen F. Altschul, Thomas L. Madden, Alejandro A.
Schaffer, Jinghui Zhang, Zheng Zhang, Webb Miller, and David J.
Lipman (1997), "Gapped BLAST and PSI-BLAST: a new generation of
protein database search programs", Nucleic Acids Res. 25:3389-3402.


Reference for composition-based statistics: Alejandro A. Schaffer,
L. Aravind, Thomas L. Madden, Sergei Shavirin, John L. Spouge, Yuri
I. Wolf, Eugene V. Koonin, and Stephen F. Altschul (2001),
"Improving the accuracy of PSI-BLAST protein database searches with
composition-based statistics and other refinements", Nucleic Acids
Res. 29:2994-3005.



Database: blastdb.faa
           39 sequences; 9,745 total letters



Query= XP_056952759.1 Cyclase/dehydrase [Penicillium manginii]

Length=251
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

KMK63051.1 cyclase/dehydrase family protein [Aspergillus fumigatu...  270     1e-94
EDP49289.1 sreptomyces cyclase/dehydrase family protein [Aspergil...  270     1e-94
EAL88577.1 sreptomyces cyclase/dehydrase family protein [Aspergil...  269     2e-94
XP_750615.1 sreptomyces cyclase/dehydrase family protein [Aspergi...  269     2e-94
EAW15943.1 sreptomyces cyclase/dehydrase family protein [Aspergil...  264     2e-92
XP_001257840.1 sreptomyces cyclase/dehydrase family protein [Aspe...  264     2e-92
QRD88608.1 oligoketide cyclase/lipid transport protein [Aspergill...  263     6e-92
XP_001272636.1 uncharacterized protein ACLA_089010 [Aspergillus c...  259     2e-90
EAW11210.1 sreptomyces cyclase/dehydrase family protein [Aspergil...  259     2e-90
PWY79868.1 cyclase/dehydrase family protein [Aspergillus eucalypt...  247     9e-86
XP_025391015.1 cyclase/dehydrase family protein [Aspergillus euca...  247     9e-86
```


## pytorch
If you find it tedious to manually type `-B /cluster/tufts` every time you want to run a container, you can define an environment variable to handle this for you. 

```
export SINGULARITY_BIND="/cluster/tufts"
```

If you add the above line to your `.bashrc` file, and you'll never have to worry about the bind issue again.

```
$ singularity exec pytorch_2.1.2-cuda11.8-cudnn8-runtime.sif python torch_cpu.py 
```

Output
```
99 2273.6953125
199 1519.8489990234375
299 1017.45751953125
399 682.482421875
499 459.0209655761719
599 309.8703918457031
699 210.26351928710938
799 143.7040557861328
899 99.20035552978516
999 69.42457580566406
1099 49.489356994628906
1199 36.13310623168945
1299 27.178112030029297
1399 21.169414520263672
1499 17.134441375732422
1599 14.422635078430176
1699 12.59858512878418
1799 11.37055492401123
1899 10.543051719665527
1999 9.984912872314453
Result: y = -0.02127980999648571 + 0.8298792839050293 x + 0.003671120386570692 x^2 + -0.08950956165790558 x^3
```