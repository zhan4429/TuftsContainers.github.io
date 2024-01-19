# singularity exec
Singularity has a `run` subcommand. This can be used to run the user-defined default command within a container. However, it is recommended to use another subcommand `exec` instead of `run`. If you're interested in `run`, please check the [singularity user guide](https://docs.sylabs.io/guides/3.8/user-guide/cli/singularity_run.html#singularity-run). 

## Syntax
Download or build a container from a given URI. 
```
singularity exec [options] image command
```

After we pulled images from public registries onto the cluster, we can use them to run analysis. If you haven't pulled images, please follow the guideline in the [Pull container images](hands-on/pull.md).

## Test datasets
I stored test datasets for blast and pytorch, please copy the datasets to your own folder.
```
$ cp -r /cluster/tufts/biocontainers/workshop/Spring2024Container/data/* .
# ls
blastdb.faa  easysfs_0.0.1.def  easySFS_examples  input.fasta
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

Why there is an error `BLAST options error: File blastdb.faa does not exist`?
The reason is I forgot to bind `/cluster/tufts` into the container. 
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
$ singularity exec pytorch_2.1.2-cuda11.8-cudnn8-runtime.sif python torch_demo.py 
```

Output
```
Using device: cpu

99 1670.406005859375
199 1174.715087890625
299 827.3037109375
399 583.6807861328125
499 412.75054931640625
599 292.76397705078125
699 208.4979705810547
799 149.29208374023438
899 107.67591857910156
999 78.41207885742188
1099 57.82627868652344
1199 43.339988708496094
1299 33.142459869384766
1399 25.961669921875
1499 20.903663635253906
1599 17.33988380432129
1699 14.828240394592285
1799 13.057659149169922
1899 11.809200286865234
1999 10.928688049316406
Result: y = 0.04767719283699989 + 0.8479017615318298 x + -0.008225110359489918 x^2 + -0.09207310527563095 x^3
```

In the workshop, we run the demo with a CPU node. If you run the torch container with a GPU node, you will see the output similar to below. Don't forget to add `--nv` in your command, otherwise GPU acceleration will not be enabled.

```
$ srun -N1 -n2 --gres=gpu:1 -p gpu -t1:00:00 --pty bash
$ singularity exec --nv /cluster/tufts/biocontainers/workshop/Spring2024Container/pytorch_2.1.2-cuda11.8-cudnn8-runtime.sif python torch_demo.py 
Using device: cuda
NVIDIA A100 80GB PCIe

99 2358.21044921875
199 1641.4652099609375
299 1144.3896484375
399 799.3317260742188
499 559.5805053710938
599 392.8489074707031
699 276.7975769042969
799 195.95428466796875
899 139.59222412109375
999 100.26740264892578
1099 72.80936431884766
1199 53.623443603515625
1299 40.20844268798828
1399 30.822301864624023
1499 24.25098419189453
1599 19.647563934326172
1699 16.420917510986328
1799 14.158018112182617
1899 12.570213317871094
1999 11.455560684204102
Result: y = 0.0517403818666935 + 0.8414013981819153 x + -0.00892607495188713 x^2 + -0.0911484807729721 x^3
```

