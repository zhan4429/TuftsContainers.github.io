## Singularity

```
$ module load singularity/3.8.4
$ module list
  1) squashfs/4.4                 2) singularity/3.8.4(default)
```

It's worth to mention to points here:
- singularity/3.8.4 is the default version, meaning you can use either `module load singularity` or `module load singularity/3.8.4`. However, it is recommended to use the latter option.
- Although you only loaded singularity/3.8.4, we modified its modulefile to include squashfs/4.4 as a dependency.

## Apptainer
```
module avail apptainer
```