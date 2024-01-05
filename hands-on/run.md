## Singularity/apptainer exec
Singularity has a `run` subcommand. This can be used to run the user-defined default command within a container. However, it is recommended to use another subcommand `exec` instead of `run`. If you're interested in `run`, please check the [singularity user guide](https://docs.sylabs.io/guides/3.8/user-guide/cli/singularity_run.html#singularity-run). 

### Syntax
Download or build a container from a given URI. 
```
singularity/apptainer exec [options] image command
```

After we pulled images from public registries onto the cluster, we can use them to run analysis. If you haven't pulled images, please follow the guideline in the [Pull container images](hands-on/pull_run.md).
#### blast
##### syntax
```
blastn –db nt –query nt.fsa –out results.out  
```


## pytorch
```
$ singularity exec -B /cluster/tufts pytorch_2.1.2-cuda11.8-cudnn8-runtime.sif python torch_cpu.py 
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